---
name: create-markdown
description: Write markdown content for blogkit-md (@san-siva/blogkit-md). Follows the blogkit style guide — frontmatter, heading hierarchy, callouts, code blocks, and Mermaid diagrams.
user_invocable: true
---

# create-markdown

Write a markdown file for the blogkit-md renderer (`@san-siva/blogkit-md`). This is an opinionated blog post renderer — markdown is not rendered as a plain document but as a structured, visually rich blog post.

## Steps

1. Ask the user what they want to write about (topic, title, brief outline) if not already provided.
2. Draft the markdown following the style guide below.
3. Show the result and ask if they'd like any adjustments.

---

## Style Guide

### Frontmatter

Always start with YAML frontmatter:

```markdown
---
title: Your Post Title
description: A short, one or two sentence description shown below the title.
---
```

- `title` is required — rendered as the `BlogHeader` component.
- `description` is required — shown as the subtitle.
- Do NOT add an H1 heading when frontmatter title is provided (it would be stripped anyway).

---

### Heading Hierarchy

| Level | Purpose | Layout effect |
|-------|---------|---------------|
| `##` | Major section | Creates a new `BlogSection` |
| `###` | Subsection within a section | Creates a nested subsection |
| `####` `#####` `######` | Minor headings | Rendered as bold text, no layout change |

- Use `##` and `###` intentionally — they drive the visual layout.
- Don't skip levels (e.g. `##` → `####`) — it breaks the section hierarchy.
- Content before the first `##` becomes an untitled intro section.

---

### Callouts

Use GitHub-style blockquote alerts:

```markdown
> [!NOTE]
> Something the reader should know.

> [!TIP]
> A helpful suggestion.

> [!IMPORTANT]
> Critical information.

> [!WARNING]
> Something that could go wrong.

> [!CAUTION]
> A danger or destructive action.
```

Plain `>` blockquotes render as info-style callouts.

---

### Code Blocks

Always specify the language:

```markdown
    ```typescript
    const x: string = "hello";
    ```

    ```bash
    npm install -g @san-siva/blogkit-md
    ```

    ```json
    { "key": "value" }
    ```
```

---

### Mermaid Diagrams

Use fenced code blocks with `mermaid`:

```markdown
    ```mermaid
    graph TD
      A[Start] --> B[Process] --> C[End]
    ```
```

---

### Tables

Standard markdown tables are supported (header + rows):

```markdown
| Column A | Column B | Column C |
|----------|----------|----------|
| value    | value    | value    |
```

---

### Task Lists

```markdown
- [x] Completed item
- [ ] Pending item
```

---

### Inline Formatting

| Syntax | Use |
|--------|-----|
| `**bold**` | Emphasis, key terms |
| `_italic_` | Titles, subtle emphasis |
| `` `code` `` | Inline code, commands, filenames |
| `[text](url)` | Links |
| `![alt](url)` | Images |

---

### General Writing Tips

- Write for long-form reading — blogkit-md is optimised for articles, not quick docs.
- Keep intro paragraphs before the first `##` short (2–4 sentences max).
- Each `##` section should be self-contained and scannable.
- Prefer callouts over parenthetical asides for important notes.
- Use code blocks liberally for any technical content.
