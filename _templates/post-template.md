---
layout: post
title: "Your Post Title Here"
date: YYYY-MM-DD HH:MM:SS +0900
categories: []
tags: []
description: "One sentence summary for SEO and social preview cards (140–160 chars)."
author: Junseo Lee
math: true            # set false if no equations — skips the ~1MB MathJax script
published: true
# permalink: /custom-url/   # uncomment to override the default /YYYY/MM/DD/slug/
---

<!--
  USAGE
  -----
  1. Copy this file to `_posts/` and rename to `YYYY-MM-DD-your-slug.md`.
     Example: `_posts/2026-04-20-modular-typography.md`
  2. Update the front matter above (title, date with +0900 KST offset, description).
  3. Set `math: false` if the post has no equations — saves a ~1MB script load.
  4. Delete this comment block and the demo content below before publishing.
-->

A lead paragraph goes here. This is what shows up in post listings and in social
preview cards, so make it stand on its own — one to three sentences that frame
what the reader is about to get.

## A section heading

Body text inherits the site's typography: Source Serif 4 at 16px on a 1.6
line-height. Inline emphasis like **bold**, *italic*, and `inline code` follows
the same family, with code switching to JetBrains Mono. Links look
[like this](https://example.com) and open in a new tab when external.

### A sub-section

Heading hierarchy follows a perfect-fourth scale (1.333×) on desktop and a
major-third scale (1.25×) on mobile. h2 marks sections, h3 marks sub-sections.
You shouldn't normally need h4 or deeper — if you do, it's a sign the post
should split.

A bulleted list:

- First point
- Second point with a [link](https://example.com)
- Third point

A numbered list when order matters:

1. First step
2. Second step
3. Third step

> A blockquote sets off a quotation or an aside. Use it sparingly — for
> citations, key claims, or a voice that isn't yours.

## Code

Inline code: `let x = 42;`. Fenced blocks get syntax highlighting via Rouge:

```python
def fibonacci(n):
    a, b = 0, 1
    for _ in range(n):
        a, b = b, a + b
    return a
```

## Math

Inline math like $E = mc^2$ flows naturally with the prose. Display math sits
on its own line, centered:

$$
\int_{-\infty}^{\infty} e^{-x^2}\,dx = \sqrt{\pi}
$$

Aligned environments work too:

$$
\begin{aligned}
\nabla \cdot \mathbf{E} &= \frac{\rho}{\varepsilon_0} \\
\nabla \cdot \mathbf{B} &= 0
\end{aligned}
$$

## Images

Drop images in `assets/images/posts/` and reference them like this (uncomment
when ready):

<!-- ![Alt text describing the image](/assets/images/posts/example.jpg) -->

Always include alt text — it's both an accessibility requirement and what
screen readers and search engines actually read.

## Footnotes

A claim that needs sourcing can carry a footnote.[^1] Footnotes collect at the
bottom of the post automatically.

[^1]: Footnote text appears at the bottom of the post, with a back-link to the
      reference.

## Tables

| Method | Convergence | Cost |
|---|---|---|
| Newton | quadratic | high |
| Secant | superlinear | medium |
| Bisection | linear | low |

Tables wrap in a horizontal scroll container on mobile automatically.
