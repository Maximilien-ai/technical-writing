# Content Generation Workflow

## Pipeline Overview

```
Brand Kit (identity) + Content Type (format) + Prompt (question)
                              │
                              v
                    Content Generation
                              │
                              v
              Generated Markdown + SEO Title + Slug
                              │
                              v
                  Publish / Draft / Review
```

## Prerequisites Checklist

| Prerequisite | Check Command | What to Look For |
|---|---|---|
| Brand kit configured | `senso brand-kit get` | Non-empty `guidelines` object |
| At least 1 content type | `senso content-types list` | Array with at least one item |
| At least 1 prompt | `senso prompts list` | Array with at least one item |
| Sufficient credits | `senso credits` | Positive `balance` value |

## Prompt Types

| Type | Purpose | Example |
|---|---|---|
| `awareness` | Top-of-funnel — introduce concepts | "What is mortgage refinancing?" |
| `consideration` | Mid-funnel — compare options | "How does our product compare to competitors?" |
| `decision` | Bottom-funnel — drive action | "What are the pricing plans for our service?" |
| `evaluation` | Post-purchase — validate choice | "How do I get the most out of my subscription?" |

## Content Type Config Fields

| Field | Purpose |
|---|---|
| `template` | The main generation instruction — tells the AI what to produce |
| `cta_text` | Call-to-action button text (e.g., "Start free trial") |
| `cta_destination` | CTA URL |
| `writing_rules` | Array of format-specific rules (e.g., "Include a comparison table") |

## Single vs. Batch Generation

| Scenario | Command | Behavior |
|---|---|---|
| Test one prompt + one template | `senso generate sample` | Synchronous — returns content inline (30-90s) |
| Run all prompts | `senso generate run` | Async — returns run ID, poll for results |
| Run subset of prompts | `senso generate run --prompt-ids` | Async — same as above |

## Run Statuses

| Status | Meaning |
|---|---|
| `queued` | Run is waiting to start |
| `in_progress` | Actively generating content |
| `completed` | All items finished successfully |
| `failed` | Run encountered errors |
| `partial` | Some items succeeded, some failed |
