# Writing Style Guide

**Owner:** Editor | **Version:** 4.0 | **Last Updated:** 2026-04-06

## Audience

Collectors. Write for readers who already know what a reference number is, understand basic mechanical-watch vocabulary, and care about condition, provenance, originality, service history, and market context.

## Standard

Follow the **Microsoft Style Guide** spirit:

- Be clear first.
- Prefer plain language over prestige fluff.
- Use active voice unless passive is clearer.
- Front-load useful information.
- Keep sentences controlled and readable.
- Use consistent terminology.
- Respect the reader's time.

## Voice and Tone

- **Authoritative, not breathless.** Avoid luxury-brand worship.
- **Specific, not vague.** Name references, movements, eras, and trade-offs when known.
- **Collector-minded.** Emphasize how a detail affects desirability, usability, originality, risk, or price.
- **Measured.** Avoid claiming certainty where the market is subjective.
- **Modern and clean.** No purple prose.

## Structure

- **Title:** Clear, searchable, useful.
- **Introduction:** 2-3 sentences that frame the collector problem.
- **Body:** Use descriptive H2s and H3s.
- **Conclusion:** End with a practical takeaway, not generic summary filler.
- **Output format:** HTML.

## HTML Requirements

All deliverables should be valid, clean HTML suitable for publishing workflow handoff.

Required structure:

```html
<article>
  <h1>Title</h1>
  <p>Intro paragraph...</p>
  <section>
    <h2>Section heading</h2>
    <p>...</p>
  </section>
</article>
```

Preferred elements:

- `<article>` for the document wrapper
- `<section>` for major sections
- `<h2>` and `<h3>` in order; do not skip levels
- `<ul>` and `<ol>` for lists
- `<table>` only when comparison is materially helpful
- `<blockquote>` only for attributed source quotations

Do not:

- inline presentational styling
- use unnecessary `<div>` wrappers
- paste markdown into HTML drafts
- use heading levels for visual emphasis instead of structure

## Formatting Rules

- Spell out a term on first mention when needed, then use the shorter form consistently.
- Use en dashes for ranges when appropriate.
- Use numerals for reference numbers, calibers, years, dimensions, and prices.
- Use sentence case for headings.
- Link only when the source materially helps the reader or supports a claim.

## Watch-Specific Standards

- Distinguish clearly between **reference**, **caliber/movement**, **case material**, **dial variant**, and **complication**.
- Be careful with terms like **rare**, **investment-grade**, **museum-quality**, and **mint**. Use only when justified.
- If authenticity cannot be established from public evidence alone, say so.
- Differentiate **vintage**, **neo-vintage**, and **modern** only when the cutoff is explained.
- State when parts may have been replaced or refinished if that affects originality.
- Avoid overgeneralizing auction results into market law.
- When discussing value, prefer wording like **market demand**, **collector interest**, or **pricing trends** over promises of appreciation.

## Sources and Evidence

Preferred source types, in rough order:

1. Patek Philippe official materials and archive/extract guidance
2. Major auction house catalogs and results
3. Reputable dealer documentation with transparent reference details
4. Specialist watch publications with editorial standards
5. Primary interviews or brand statements

Every draft must include:

- source list
- fact-sensitive claims marked for review where needed
- unresolved questions called out explicitly

## Content Quality Checklist

- [ ] Title is clear and search-friendly
- [ ] HTML structure is clean and valid
- [ ] Claims about references, calibers, and production context are sourced
- [ ] Market language is careful and non-hypey
- [ ] Condition and originality terms are used precisely
- [ ] No unsupported authenticity claims
- [ ] No broken links
- [ ] Reviewed for accuracy by reviewer
- [ ] SEO metadata defined
