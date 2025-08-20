---
marp: true
paginate: true
size: 16:9
math: katex
headingDivider: 2
class: lead
footer: ""
color: white
backgroundColor: '#0b1220'
theme: product-docs
---

<style>
/* @theme product-docs */
@import "default";

:root {
  --bg: #0b1220;
  --bg2: #0f172a;
  --fg: #e6edf3;
  --accent: #6ee7ff;
  --muted: #9fb3c8;
}

section {
  font-family: Inter, system-ui, -apple-system, "Segoe UI", Roboto, "Helvetica Neue", Arial,
    "Noto Sans", "Liberation Sans", sans-serif;
  color: var(--fg);
  background: linear-gradient(180deg, var(--bg) 0%, var(--bg2) 100%);
}

h1, h2, h3 {
  color: var(--accent);
  letter-spacing: .3px;
}

/* Page numbers shown bottom-right */
section::after {
  content: attr(data-marpit-pagination) " / " attr(data-marpit-pagination-total);
  position: absolute;
  right: 24px;
  bottom: 18px;
  font-size: 0.8rem;
  color: var(--muted);
}

/* Helpful layout helpers */
section.lead {
  display: grid;
  place-items: center;
  text-align: center;
}

section.tight * {
  line-height: 1.2;
}

blockquote {
  border-left: 4px solid var(--accent);
  padding-left: 12px;
  color: var(--muted);
}

code, pre code {
  background: rgba(255,255,255,0.06);
  border-radius: 10px;
}
</style>

<!-- _class: lead -->

# Product Documentation Deck

**Your Company / Product Name**

**Contact:** 23f3002227@ds.study.iitm.ac.in

---

# Why Marp for Product Docs?

- Single source of truth in version control (Git/GitHub)
- Export to **HTML**, **PDF**, **PPTX** via Marp CLI
- Reusable custom theme (**`product-docs`**) included inside this file
- Page numbers via theme CSS (bottom-right)
- KaTeX math for specifications & complexity

> Tip: Keep images in `assets/` and use relative paths so exports work everywhere.

---

<!-- _backgroundImage: url('assets/bg-architecture.jpg') -->
<!-- _backgroundSize: cover -->
<!-- _backgroundPosition: center -->



# System Architecture (Overview)

- Service boundaries, data flow, and external dependencies
- Identify ownership and SLAs per component
- Call out failure domains and retry/backoff strategies

---

# Feature Summary

- **Authentication & Authorization:** OAuth 2.1, OIDC
- **Observability:** Structured logs, traces, RED metrics
- **Data:** Postgres for OLTP, S3 for blobs, Redis cache
- **APIs:** REST + Async events (Kafka)

---

# Performance & Complexity

We target near-linear scalability with respect to input size \(n\).

- Sorting stage: \( O(n \log n) \)
- Linear scan stage: \( O(n) \)

Combined complexity (dominant term):

$$
T(n) = T\left(\frac{n}{2}\right) + O(n) \implies T(n) = O(n \log n)
$$

Throughput model:

$$
\text{QPS}_{\max} \approx \frac{C \cdot k}{L}\quad \text{where } C=\text{cores},\ k=\text{concurrency},\ L=\text{avg latency}
$$

---

# API Contract (Excerpt)

```http
GET /v1/items?page=1&limit=50
Accept: application/json
Authorization: Bearer <token>
```

```json
{
  "items": [{ "id": "it_123", "name": "Widget" }],
  "next": "...cursor..."
}
```

**Error surface**

```json
{
  "error": {
    "code": "rate_limited",
    "retry_after_sec": 30
  }
}
```

---

# Deploy & Export

```bash
# Install marp-cli (once)
npm i -g @marp-team/marp-cli

# Live preview
yarn marp --server slides.md

# Export formats
marp slides.md --html --output slides.html
marp slides.md --pdf  --output slides.pdf
marp slides.md --pptx --output slides.pptx
```

Notes:
- Ensure `assets/` is checked in so background images render in all outputs.
- Use CI to publish HTML slide to GitHub Pages on every push.

---

# Theming Cheatsheet

Use slide-scoped directives for fine control:

<!-- _class: tight -->

- `<!-- _class: lead -->` → centered hero slide
- `<!-- color: white -->` or `<!-- backgroundColor: #111827 -->`
- `<!-- backgroundImage: assets/bg-architecture.jpg -->`
- Per-slide footer: `<!-- footer: "© 2025 Your Co." -->`

---

# Roadmap & Next Steps

1. Harden SLOs and budgets per service
2. Document failure drills & runbooks
3. Add load profiles + capacity plans

**Questions?**  
\<23f3002227@ds.study.iitm.ac.in\>

