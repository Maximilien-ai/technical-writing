# Writing Style Guide

**Owner:** Editor | **Version:** 1.0 | **Last Updated:** 2026-03-27

## Voice & Tone

- **Clear over clever.** If a simpler word works, use it.
- **Active voice** by default. Passive only when the actor is irrelevant.
- **Second person** ("you") for tutorials and guides. Third person for reference docs.
- **Present tense** for describing current behavior. Past tense for changelogs.

## Structure

- **Title:** Concise, action-oriented. "How to Deploy with Docker" not "A Guide to Docker Deployment Methodologies"
- **Introduction:** 2-3 sentences max. State what the reader will learn and why it matters.
- **Headers:** Use H2 for major sections, H3 for subsections. Never skip levels.
- **Paragraphs:** 3-5 sentences. If longer, break it up.
- **Code blocks:** Always specify the language. Include comments for non-obvious lines.

## Formatting

- **Bold** for UI elements, key terms on first use, and warnings
- `Code` for commands, file paths, variable names, function names
- *Italic* for emphasis (sparingly)
- Use numbered lists for sequential steps, bullet lists for non-ordered items

## Technical Standards

- All code examples must be tested and runnable
- Include expected output where it helps understanding
- Specify versions for tools, libraries, and APIs
- Link to official documentation for external tools on first mention
- Define acronyms on first use

## Terminology

| Use | Don't Use |
|-----|-----------|
| run | execute |
| set up | setup (as verb) |
| command line | command-line (unless adjective) |
| open source | open-source (unless adjective) |
| API | api |

## Content Quality Checklist

- [ ] Title is clear and searchable
- [ ] Introduction states the value proposition
- [ ] All code examples tested
- [ ] No broken links
- [ ] Consistent terminology throughout
- [ ] Reviewed for accuracy by technical reviewer
- [ ] SEO metadata defined (title, description, keywords)
