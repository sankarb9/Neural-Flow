# NeuralFlow — AI Data Automation Platform

A premium, high-converting, responsive landing page for an advanced AI-driven data automation platform. Built for Frontend Battle Phase 1 (June 2026).

## Run & Operate

- `pnpm --filter @workspace/ai-platform run dev` — run the frontend (auto-assigned port via PORT env)
- `pnpm --filter @workspace/api-server run dev` — run the API server (port 5000)
- `pnpm run typecheck` — full typecheck across all packages
- `pnpm run build` — typecheck + build all packages

## Stack

- pnpm workspaces, Node.js 24, TypeScript 5.9
- Frontend: React + Vite (artifact: `ai-platform`)
- Styling: Tailwind CSS + custom CSS variables (no runtime CSS-in-JS)
- Animations: Native CSS Transitions/WAAPI only (per contest rules)
- API: Express 5 (shared `api-server` artifact)
- Fonts: JetBrains Mono (headers) + Inter (body) — Google Fonts

## Where things live

- `artifacts/ai-platform/` — Landing page React+Vite app (served at `/`)
- `artifacts/ai-platform/src/components/` — All UI components
- `artifacts/ai-platform/src/index.css` — Design tokens + all CSS (no CSS-in-JS)
- `artifacts/ai-platform/src/assets/svgs/` — Contest-provided SVG assets
- `artifacts/api-server/` — Express API server (served at `/api`)

## Architecture decisions

- **Feature 1 (Pricing isolation):** Currency switcher and billing toggle mutate DOM text nodes directly via refs — zero React state, zero re-renders. The `flushPrices()` function targets only 3 `<span>` textContent nodes per tier.
- **Feature 2 (Bento/Accordion):** Active index stored in a `useRef` (not `useState`). Desktop shows CSS Grid bento layout; mobile shows a pure CSS accordion using WAAPI for height transitions. On resize past 768px breakpoint, active index transfers automatically between layouts via a `resize` event listener.
- **No banned libraries:** Framer Motion, Radix, Shadcn, HeadlessUI are NOT used for Feature 1 or 2. Only native CSS transitions and Web Animations API.
- **Performance:** Loader dismissed in ~300ms (well under 500ms TTI cap). Entry animations use `animation-delay` staggers, not JavaScript. All transitions use hardware-accelerated CSS properties (transform, opacity).
- **Semantic HTML:** `<header>`, `<main>`, `<section>`, `<article>`, `<nav>`, `<footer>`, `<ul>`/`<ol>` lists, proper ARIA roles and labels throughout.

## Product

NeuralFlow landing page sections:
1. **Navbar** — sticky, blur-backdrop, mobile hamburger menu
2. **Hero** — headline, subtitle, stats row (99.98% uptime, 12ms latency, 4B+ events/day), code preview
3. **Logos Strip** — trusted companies band
4. **Features (Bento/Accordion)** — 6 features in CSS Grid bento on desktop, accordion on mobile
5. **Pricing** — 3 tiers × 2 billing cycles × 3 currencies with isolated DOM updates
6. **Testimonials** — 6 customer quotes in 3-column grid
7. **CTA Section** — glowing call-to-action card
8. **Footer** — 4-column layout with social links and legal

## User preferences

_Populate as you build — explicit user instructions worth remembering across sessions._

## Gotchas

- SVG assets are in `src/assets/svgs/` — imported as inline React components, not `<img>` tags
- Feature 2 accordion uses WAAPI (`element.animate()`) for smooth height transitions — CSS `max-height` is set directly after animation to persist state
- Pricing matrix: `PRICING_MATRIX[tier][currency]` gives base monthly price; multiply by `ANNUAL_MULTIPLIER (0.8)` for annual
- The `reveal` scroll-reveal class uses IntersectionObserver in Landing.tsx's `useEffect`
