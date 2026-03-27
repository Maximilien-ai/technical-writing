---
name: senso-content-gen
description: >
  Generate brand-aligned content using the Senso content engine. Orchestrates brand kit
  verification, content type selection, prompt management, and generation runs. Use when
  the user wants to generate content like blog posts, FAQs, comparison pages, or any
  templated output from their knowledge base.
license: MIT
compatibility: Requires @senso-ai/cli installed and SENSO_API_KEY set
metadata:
  author: senso
  version: "1.0.0"
---

# Senso Content Generation

Generate brand-aligned content using the Senso content engine. This skill orchestrates the full pipeline: verifying prerequisites, selecting prompts and content types, triggering generation, and monitoring results.

## Prerequisites

```bash
npm install -g @senso-ai/cli
export SENSO_API_KEY=<your-key>
```

## Always Use These Flags

Every `senso` command must include `--output json --quiet`.

## Pre-Flight Checks (Critical)

Before generating content, **always verify these three prerequisites exist**. Generation will fail or produce poor results without them.

### 1. Check Brand Kit

```bash
senso brand-kit get --output json --quiet
```

If the response is empty or has no `guidelines`, the user needs to set up their brand kit first. Tell them: "Your brand kit is not configured. Would you like to set it up? I need your brand name, description, voice/tone, and any writing rules."

If the brand kit needs setup, use the **senso-brand-setup** skill.

### 2. Check Content Types

```bash
senso content-types list --output json --quiet
```

At least one content type must exist. If the list is empty, the user needs to create one:

```bash
senso content-types create --data '{
  "name": "Blog Post",
  "config": {
    "template": "Write a comprehensive blog post that answers the question directly.",
    "writing_rules": ["Use subheadings", "Include examples"]
  }
}' --output json --quiet
```

### 3. Check Prompts

```bash
senso prompts list --output json --quiet
```

At least one prompt must exist. If empty, create one:

```bash
senso prompts create --data '{
  "question_text": "What are the key features of our product?",
  "type": "awareness"
}' --output json --quiet
```

Prompt types: `awareness`, `consideration`, `decision`, `evaluation`.

### 4. Check Credits (recommended)

```bash
senso credits --output json --quiet
```

Generation costs credits. Warn the user if their balance is low before triggering a run.

## Generating Content

### Single Sample — one prompt + one content type

Use this to generate a single piece of content for a specific prompt and content type:

```bash
senso generate sample \
  --prompt-id <geo_question_id> \
  --content-type-id <content_type_id> \
  --output json --quiet
```

**Important:** The `--prompt-id` flag corresponds to the `geo_question_id` field in the prompt object.

Optional: publish immediately by adding `--destination <publisher_slug>`.

The response includes:
- Generated markdown content
- SEO title
- URL slug
- Publish results (if `--destination` was provided)

**Warning:** This call takes **30–90 seconds**. It may return a 504 timeout on very long generations — if so, tell the user the generation is still running server-side.

### Full Run — all prompts (or a subset)

Trigger a generation run across multiple prompts:

```bash
# Run all prompts
senso generate run --output json --quiet

# Run specific prompts only
senso generate run --prompt-ids <id1> <id2> --output json --quiet

# Override the default content type for this run
senso generate run --content-type-id <id> --output json --quiet

# Restrict publishing to specific publishers
senso generate run --publisher-ids <id1> <id2> --output json --quiet
```

This is async — it returns a run ID immediately. Monitor with the commands below.

## Monitoring Generation Runs

### List runs

```bash
senso generate runs-list --output json --quiet

# Filter by status or date
senso generate runs-list --status completed --output json --quiet
senso generate runs-list --active-only --output json --quiet
senso generate runs-list --start-date 2025-01-01 --end-date 2025-12-31 --output json --quiet
```

### Get run details

```bash
senso generate runs-get <run_id> --output json --quiet
```

### List items in a run

```bash
senso generate runs-items <run_id> --output json --quiet
```

Returns per-prompt status within the run.

### View run logs

```bash
senso generate runs-logs <run_id> --output json --quiet
```

## Generation Settings

View and update org-wide generation configuration:

```bash
# View current settings
senso generate settings --output json --quiet

# Update settings
senso generate update-settings --data '{
  "enable_content_generation": true,
  "content_auto_publish": false,
  "content_schedule": [1, 3, 5],
  "selected_content_type_id": "<uuid>"
}' --output json --quiet
```

The `content_schedule` is an array of days of the week (0=Sunday, 6=Saturday).

## Job Context

To see the full picture of what will be generated — all prompts with their queue status and content state:

```bash
senso generate job-context --output json --quiet
```

This shows which prompts are queued for creation vs. update, and summarizes queue counts.

## Typical Workflow

1. **Pre-flight:** Verify brand kit, content types, and prompts exist
2. **Check credits:** Ensure sufficient balance
3. **Confirm with user:** "I'm about to generate content for [N] prompts using the [content type name] template. This will use credits. Proceed?"
4. **Generate:** Run `senso generate sample` (single) or `senso generate run` (batch)
5. **Monitor:** For batch runs, poll `senso generate runs-get <id>` until status is complete
6. **Review:** Show generated content to the user
7. **Publish:** Use the **senso-review-publish** skill to publish or draft the output

## Error Handling

| HTTP Status | Meaning | Action |
|-------------|---------|--------|
| 401 | Invalid or missing API key | Check `SENSO_API_KEY` |
| 402 | Insufficient credits / spend limit | Tell user to check their plan — `senso credits --output json --quiet` |
| 404 | Prompt or content type not found | Verify the IDs exist with `senso prompts list` / `senso content-types list` |
| 409 | Generation already in progress | Wait for the current run to finish — check with `senso generate runs-list --active-only` |
| 504 | Gateway timeout | Generation is still running server-side. Poll `senso generate runs-list --active-only` to check status |
