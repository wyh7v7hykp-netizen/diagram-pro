# Brand Onboarding Spec

继承自 cathrynlavery/diagram-design 的品牌自动匹配系统。

## Flow

```
User: "onboard diagram-pro to https://yoursite.com"
  → fetch homepage
  → extract dominant palette + font stack
  → map detected values to semantic roles
  → show proposed diff
  → write tokens to brand-tokens.md
```

## Extraction Target

| Detected from site | Maps to |
|---|---|
| `<body>` background | `paper` token |
| Primary text color | `ink` token |
| Secondary/caption text | `muted` token |
| Cards/containers | `paper-2` token |
| Most-used brand color (CTA, link, heading) | `accent` token |
| `<h1>` font family | `title` font |
| `<body>` font family | `node-name` font |
| `<code>` / `<pre>` font | `sublabel` font |

## WCAG Check

Before writing tokens, verify AA contrast on `ink` over `paper`. If your site has a color that fails contrast at diagram sizes (9-12px), propose an adjusted value and explain why.

## Manual Override

Edit `brand-tokens.md` directly and update the table. Everything downstream reads from there — all diagrams inherit semantic role names, not hex codes.

## First-Run Gate

On first use in a new project, check if brand-tokens.md has been customized. If not, pause:
> "这是此项目的首张图。品牌色板还是默认值。要运行 onboarding、手动粘贴token、还是用默认值继续？"
