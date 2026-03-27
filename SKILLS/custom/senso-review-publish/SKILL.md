---
name: senso-review-publish
description: >
  Manage the Senso content verification and publishing workflow. List items awaiting review,
  approve or reject content versions, publish to external destinations, save drafts, manage
  content ownership, and unpublish content. Use when the user wants to review, approve, reject,
  or publish generated content.
license: MIT
compatibility: Requires @senso-ai/cli installed and SENSO_API_KEY set
metadata:
  author: senso
  version: "1.0.0"
---

# Senso Review & Publish

Manage the content verification pipeline and publish content to external destinations. Content flows through: **draft** -> **review** -> **published** (or **rejected** -> **restored to draft**).

## Prerequisites

```bash
npm install -g @senso-ai/cli
export SENSO_API_KEY=<your-key>
```

## Always Use These Flags

Every `senso` command must include `--output json --quiet`.

## Content Lifecycle

```
Draft  ──>  Review  ──>  Published
                │
                v
            Rejected  ──>  Restored (back to Draft)
```

Published content can also be **unpublished** (reverted to draft).

## Viewing Content in the Pipeline

### List Items by Status

```bash
# All items in the verification workflow
senso content verification --output json --quiet

# Filter by status
senso content verification --status draft --output json --quiet
senso content verification --status review --output json --quiet
senso content verification --status rejected --output json --quiet
senso content verification --status published --output json --quiet

# Search by title
senso content verification --search "refund policy" --output json --quiet

# With pagination
senso content verification --limit 20 --offset 0 --output json --quiet
```

### Inspect a Specific Content Item

```bash
senso content get <content_id> --output json --quiet
```

Returns the full content detail including all versions, metadata, processing status, and publish status.

## Publishing Content

### Publish to External Destinations

```bash
senso engine publish --data '{
  "geo_question_id": "<prompt_uuid>",
  "raw_markdown": "# Article Title\n\nArticle content here...",
  "seo_title": "SEO-Optimized Title for the Article",
  "summary": "Brief summary of the article content"
}' --output json --quiet
```

Required fields:
- `geo_question_id` — the prompt ID this content was generated for
- `raw_markdown` — the content body in markdown
- `seo_title` — the SEO title

Optional fields:
- `summary` — a brief summary

### Save as Draft

Save content for review before publishing:

```bash
senso engine draft --data '{
  "geo_question_id": "<prompt_uuid>",
  "raw_markdown": "# Draft Article\n\nContent still being refined...",
  "seo_title": "Draft Title",
  "summary": "Work in progress"
}' --output json --quiet
```

Same fields as publish — the content is saved but not pushed to external destinations.

## Rejecting Content

Reject a content version with an optional reason:

```bash
# Reject with a reason
senso content reject <version_id> --reason "Needs more supporting data in section 3" --output json --quiet

# Reject without a reason
senso content reject <version_id> --output json --quiet
```

**Note:** The reject command uses a `version_id`, not a `content_id`. Get version IDs from `senso content get <content_id>`.

## Restoring Rejected Content

Restore a rejected version back to draft status for further editing:

```bash
senso content restore <version_id> --output json --quiet
```

## Unpublishing Content

Remove published content from external destinations and revert to draft:

```bash
senso content unpublish <content_id> --output json --quiet
```

## Deleting Content

Permanently remove a content item:

```bash
senso content delete <content_id> --output json --quiet
```

**This cannot be undone.** Always confirm with the user before deleting.

## Managing Content Ownership

Owners are responsible for reviewing and approving content.

### View Owners

```bash
senso content owners <content_id> --output json --quiet
```

### Set Owners (Replace All)

```bash
senso content set-owners <content_id> --user-ids <user_id_1> <user_id_2> --output json --quiet
```

This replaces all existing owners with the specified user IDs.

### Remove a Single Owner

```bash
senso content remove-owner <content_id> <user_id> --output json --quiet
```

### Finding User IDs

To find user IDs for setting ownership:

```bash
senso users list --output json --quiet
senso members list --output json --quiet
```

## Typical Review Workflow

When the user asks to "review content" or "check what's pending":

1. **List pending items:**
   ```bash
   senso content verification --status review --output json --quiet
   ```

2. **Show each item to the user** — display the title, summary, and content

3. **For each item, ask the user:** "Approve (publish), reject, or skip?"

4. **Act on their decision:**
   - **Approve:** Publish with `senso engine publish`
   - **Reject:** Reject with `senso content reject <version_id> --reason "..."`
   - **Skip:** Move to the next item

5. **Summary:** Tell the user how many items were published, rejected, and skipped

## Typical Publish Workflow (After Generation)

After using the **senso-content-gen** skill to generate content:

1. The `senso generate sample` command returns generated content inline
2. Show the generated content to the user for review
3. If approved, publish with `senso engine publish` using the generated markdown, SEO title, and the prompt ID
4. If the user wants changes, they can edit and re-publish, or reject and regenerate

## Error Handling

| HTTP Status | Meaning | Action |
|-------------|---------|--------|
| 401 | Invalid or missing API key | Check `SENSO_API_KEY` |
| 404 | Content or version not found | Verify the ID with `senso content verification` or `senso content get` |
| 409 | Conflict — operation already in progress | Another publish/reject operation is running. Wait and retry. |
| 422 | Unprocessable | Check that `geo_question_id`, `raw_markdown`, and `seo_title` are all provided for publish/draft |
