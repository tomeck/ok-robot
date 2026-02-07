# OK ROBOT — Website

Marketing site for [ok-robot.co](https://ok-robot.co). Built with [Astro](https://astro.build) + [Tailwind CSS v4](https://tailwindcss.com).

## Quick Start

```bash
# Install dependencies
npm install

# Start dev server (http://localhost:4321)
npm run dev

# Build for production
npm run build

# Preview production build locally
npm run preview
```

## Project Structure

```
ok-robot/
├── public/
│   └── favicon.svg              # Site favicon
├── src/
│   ├── components/
│   │   ├── Nav.astro            # Top navigation (desktop + mobile)
│   │   ├── Hero.astro           # Landing page hero section
│   │   ├── Products.astro       # Product cards overview
│   │   ├── HowItWorks.astro     # 4-step integration flow
│   │   ├── Pricing.astro        # Pricing tiers (Free/Pro/Enterprise)
│   │   ├── CTA.astro            # Bottom call-to-action banner
│   │   └── Footer.astro         # Site footer with links
│   ├── layouts/
│   │   └── Layout.astro         # Base HTML layout (head, fonts, meta)
│   ├── pages/
│   │   ├── index.astro          # Home / landing page
│   │   ├── products.astro       # Detailed product pages
│   │   ├── docs.astro           # Documentation hub
│   │   ├── pricing.astro        # Pricing + FAQ
│   │   ├── about.astro          # About the company
│   │   └── contact.astro        # Contact form
│   └── styles/
│       └── global.css           # Tailwind imports + custom CSS vars + animations
├── astro.config.mjs             # Astro configuration
├── package.json
├── tsconfig.json
└── README.md
```

## Editing Guide

### Content changes
- **Text/copy**: Edit directly in the `.astro` files. All content is inline (no CMS).
- **Navigation links**: Edit `src/components/Nav.astro` → `navLinks` array.
- **Products**: Edit `src/components/Products.astro` and `src/pages/products.astro`.
- **Pricing tiers**: Edit `src/components/Pricing.astro` → `plans` array.
- **Footer links**: Edit `src/components/Footer.astro` → `footerSections` array.

### Styling
- **Colors**: All colors are CSS custom properties in `src/styles/global.css` under `@theme`.
- **Fonts**: Loaded from Google Fonts in `Layout.astro`. Change the `--font-display`, `--font-body`, `--font-mono` vars.
- **Tailwind**: Standard Tailwind v4 utility classes throughout. Custom colors work as `text-accent`, `bg-surface`, etc.

### Adding pages
Create a new `.astro` file in `src/pages/`. Astro uses file-based routing:
- `src/pages/blog.astro` → `/blog`
- `src/pages/docs/quickstart.astro` → `/docs/quickstart`

### Images
Place images in `public/` and reference as `/image-name.png`. For optimized images, use Astro's `<Image>` component with files in `src/assets/`.

## Architecture Decisions

### Why Astro (not Next.js, plain HTML, etc.)
- **Static output**: Generates pure HTML/CSS with zero JS by default = fastest possible site
- **Component model**: Reusable `.astro` components without needing React for a marketing site
- **File-based routing**: Add a page by adding a file
- **Tailwind v4 integration**: First-class support
- **Easy to learn**: If you know HTML, you know 90% of Astro

### API products live elsewhere
This site is the **marketing/docs front door**. Your actual APIs should be on separate infrastructure:
- `api.ok-robot.co` → Your API servers (e.g., Railway, Fly.io, AWS)
- `dashboard.ok-robot.co` → Customer dashboard (separate app)
- `docs.ok-robot.co` → Full API reference (Mintlify, Docusaurus, or self-hosted)
- `status.ok-robot.co` → Uptime monitoring (Betteruptime, Instatus)

The DNS setup keeps everything decoupled — you can change any backend without touching the marketing site.

## Deployment

### Vercel (Recommended)
```bash
# Install Vercel CLI
npm i -g vercel

# Deploy (first time — will link to your Vercel account)
vercel

# Deploy to production
vercel --prod
```

Then point `ok-robot.co` DNS:
1. In your domain registrar, set nameservers to Vercel's, **or**
2. Add a CNAME record: `@` → `cname.vercel-dns.com`

### Cloudflare Pages (Alternative)
1. Push this repo to GitHub
2. In Cloudflare dashboard → Pages → Create project → Connect repo
3. Build command: `npm run build`
4. Output directory: `dist`
5. Point your domain via Cloudflare DNS

## TODO / Next Steps

- [ ] Wire contact form to a backend (Formspree, custom endpoint, or Resend)
- [ ] Add blog with Astro content collections
- [ ] Add real API documentation (consider Mintlify at `docs.ok-robot.co`)
- [ ] Set up analytics (Plausible, PostHog, or Vercel Analytics)
- [ ] Add OpenGraph images for social sharing
- [ ] Create a `robots.txt` and `sitemap.xml` (Astro has plugins for this)
- [ ] Replace placeholder social links with real ones
- [ ] Set up the customer dashboard at `dashboard.ok-robot.co`
