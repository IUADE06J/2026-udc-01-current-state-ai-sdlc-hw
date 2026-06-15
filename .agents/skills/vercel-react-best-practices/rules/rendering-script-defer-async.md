---
title: Use defer or async on Script Tags
impact: HIGH
impactDescription: eliminates render-blocking
tags: rendering, script, defer, async, performance
---

## Use defer or async on Script Tags

**Impact: HIGH (eliminates render-blocking)**

Script tags without `defer` or `async` block HTML parsing while the script downloads and executes. This delays First Contentful Paint and Time to Interactive.

In Next.js, use `next/script` with the `strategy` prop instead of raw `<script>` tags to avoid render-blocking and SSR hydration issues:

- **`afterInteractive`** (replaces `async`): independent scripts like analytics
- **`beforeInteractive`** (replaces `defer`): scripts that should load before the page becomes interactive

**Incorrect (blocks rendering):**

```tsx
export default function Document() {
  return (
    <html>
      <head>
        <script src="https://example.com/analytics.js" />
        <script src="/scripts/utils.js" />
      </head>
      <body>{/* content */}</body>
    </html>
  )
}
```

**Correct (non-blocking with next/script):**

```tsx
import Script from 'next/script'

export default function Page() {
  return (
    <>
      {/* Independent script — async equivalent */}
      <Script src="https://example.com/analytics.js" strategy="afterInteractive" />
      {/* DOM-dependent script — defer equivalent */}
      <Script src="/scripts/utils.js" strategy="beforeInteractive" />
    </>
  )
}
```

Reference: [Next.js Script Component](https://nextjs.org/docs/app/api-reference/components/script)
