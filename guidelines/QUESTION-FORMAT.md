# Question File Format

Question files use **Markdown** for structure and **LaTeX** for math. Each file is a single question with YAML frontmatter for metadata and tags.

## Example

```markdown
---
tags: [algebra, linear-equations, bloom-apply]
difficulty: 2
type: multiple-choice
---

Solve for $x$: $2x + 5 = 13$

- [ ] $x = 3$
- [x] $x = 4$
- [ ] $x = 5$
- [ ] $x = 6$

---
**Solution:** Subtract 5 from both sides: $2x = 8$. Divide by 2: $x = 4$.
```

## Frontmatter Fields

| Field | Required | Description |
|-------|----------|-------------|
| `tags` | Yes | Array of tags for filtering (e.g. `algebra`, `word-problem`) |
| `difficulty` | Yes | 1–5 scale |
| `type` | Yes | `multiple-choice`, `short-answer`, `numeric`, etc. |

## LaTeX

- Inline math: `$x^2 + 1$`
- Block math: `$$\frac{a}{b}$$`
