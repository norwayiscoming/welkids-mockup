# WelKids Campaign Mockup Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build a 6-module interactive mockup website for the WelKids AD3 E K2 campaign using Astro, with a polished glass-morphism design system.

**Architecture:** Astro static site with shared layouts (MainLayout for public pages, AdminLayout for CMS), reusable `.astro` components, global CSS design tokens + glass effects, and JSON mock data files. All interactivity is client-side vanilla JS. No backend.

**Tech Stack:** Astro 5.x, Google Fonts (Montserrat Alternates + Quicksand), vanilla CSS (no Tailwind), vanilla JS for interactions

**Design Spec:** `docs/superpowers/specs/2026-04-01-welkids-campaign-mockup-design.md`

---

## File Structure

```
welkids-mockup/
├── package.json
├── astro.config.mjs
├── tsconfig.json
├── public/
│   └── favicon.svg
├── src/
│   ├── layouts/
│   │   ├── BaseLayout.astro        # HTML shell, fonts, meta, global CSS link
│   │   ├── MainLayout.astro        # BaseLayout + Navbar + Footer
│   │   └── AdminLayout.astro       # BaseLayout + AdminSidebar
│   ├── components/
│   │   ├── Navbar.astro            # Glass pill navbar
│   │   ├── MobileMenu.astro        # Hamburger slide-out menu
│   │   ├── Footer.astro            # Full-width dark green footer
│   │   ├── Button.astro            # Reusable button (variant prop)
│   │   ├── GlassPanel.astro        # Glass container (variant: leaf/warm/frost)
│   │   ├── SectionHeader.astro     # H2 + subtitle + optional badge
│   │   ├── Card.astro              # Glass card with icon, badge, body
│   │   ├── ChoiceCard.astro        # Quiz answer option card
│   │   ├── ProgressBar.astro       # Step progress indicator
│   │   ├── VideoCard.astro         # Video thumbnail card
│   │   ├── GalleryItem.astro       # Award gallery image card
│   │   ├── AdminSidebar.astro      # CMS sidebar navigation
│   │   ├── StatsCard.astro         # Dashboard stat card
│   │   └── DataTable.astro         # Admin table with headers + rows
│   ├── pages/
│   │   ├── index.astro             # Homepage
│   │   ├── health-check/
│   │   │   ├── index.astro         # Health check intro
│   │   │   ├── quiz.astro          # Questionnaire (JS-driven steps)
│   │   │   └── results.astro       # Results page
│   │   ├── videos/
│   │   │   └── index.astro         # Video gallery
│   │   ├── ai-generator/
│   │   │   └── index.astro         # AI image generator (3 states)
│   │   ├── awards/
│   │   │   └── index.astro         # Award gallery
│   │   └── admin/
│   │       ├── index.astro         # Dashboard overview
│   │       ├── users.astro         # Users table
│   │       ├── videos.astro        # Videos table
│   │       ├── images.astro        # AI images table
│   │       ├── awards.astro        # Awards table
│   │       └── settings.astro      # Settings form
│   ├── styles/
│   │   ├── global.css              # Design tokens, glass effects, animations, typography
│   │   └── admin.css               # Admin-specific overrides
│   └── data/
│       ├── questions.json          # Health check questions + options
│       ├── videos.json             # Mock video list
│       ├── careers.json            # Career options for AI generator
│       └── gallery.json            # Mock gallery items
└── .gitignore
```

---

## Task 1: Project Scaffolding

**Files:**
- Create: `package.json`, `astro.config.mjs`, `tsconfig.json`, `.gitignore`, `public/favicon.svg`

- [ ] **Step 1: Initialize Astro project**

```bash
cd /Users/lab3/Desktop/phuonganh/welkids-mockup/.claude/worktrees/compassionate-wescoff
npm create astro@latest . -- --template minimal --no-install --no-git --typescript strict
```

If the directory isn't empty (LICENSE file exists), the CLI may prompt. Use `--skip-houston` if available, or manually init:

```bash
npm init -y
npm install astro@latest
```

- [ ] **Step 2: Configure astro.config.mjs**

```javascript
// astro.config.mjs
import { defineConfig } from 'astro/config';

export default defineConfig({
  site: 'http://localhost:4321',
});
```

- [ ] **Step 3: Configure tsconfig.json**

```json
{
  "extends": "astro/tsconfigs/strict",
  "compilerOptions": {
    "baseUrl": ".",
    "paths": {
      "@components/*": ["src/components/*"],
      "@layouts/*": ["src/layouts/*"],
      "@data/*": ["src/data/*"],
      "@styles/*": ["src/styles/*"]
    }
  }
}
```

- [ ] **Step 4: Create .gitignore**

```
node_modules/
dist/
.astro/
.superpowers/
```

- [ ] **Step 5: Create favicon placeholder**

Create `public/favicon.svg`:
```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 32 32">
  <rect width="32" height="32" rx="8" fill="#5EAA6E"/>
  <text x="16" y="22" font-size="18" text-anchor="middle" fill="white">W</text>
</svg>
```

- [ ] **Step 6: Install dependencies and verify build**

```bash
npm install
npx astro build
```

Expected: Build succeeds (may warn about no pages yet — that's fine).

- [ ] **Step 7: Commit**

```bash
git add package.json package-lock.json astro.config.mjs tsconfig.json .gitignore public/favicon.svg src/
git commit -m "feat: scaffold Astro project with config and favicon"
```

---

## Task 2: Design System — Global CSS

**Files:**
- Create: `src/styles/global.css`

- [ ] **Step 1: Create global.css with all design tokens**

Create `src/styles/global.css`:

```css
/* ═══════════════════════════════════════
   WelKids Design System
   ═══════════════════════════════════════ */

/* ── Google Fonts ── */
@import url('https://fonts.googleapis.com/css2?family=Montserrat+Alternates:wght@400;500;600;700;800&family=Quicksand:wght@400;500;600;700&display=swap');

/* ── CSS Custom Properties ── */
:root {
  /* Neutrals (60%) */
  --color-cream: #FAFDF9;
  --color-mint-gray: #F0F5F1;
  --color-leaf-gray: #E3ECE5;
  --color-white: #FFFFFF;

  /* Primary (30%) */
  --color-primary-dark: #3D8B4F;
  --color-primary: #5EAA6E;
  --color-primary-light: #7DC08A;
  --color-primary-tint: #D4EDDA;

  /* Accent (10%) */
  --color-accent-dark: #E8AA20;
  --color-accent: #F5C842;
  --color-accent-light: #F8D76E;
  --color-accent-tint: #FFF8E1;

  /* Warm (secondary CTA) */
  --color-warm: #FFB74D;
  --color-warm-dark: #E69520;
  --color-warm-light: #FFCC80;

  /* Text */
  --color-heading: #2D4A35;
  --color-body: #5A7D66;
  --color-muted: #9AB5A3;

  /* Semantic */
  --color-error: #E57373;
  --color-error-tint: #FDECEA;

  /* Typography */
  --font-heading: 'Montserrat Alternates', sans-serif;
  --font-body: 'Quicksand', sans-serif;

  /* Spacing */
  --space-micro: 4px;
  --space-tight: 8px;
  --space-base: 16px;
  --space-comfortable: 24px;
  --space-spacious: 32px;
  --space-section: 48px;
  --space-page: 72px;

  /* Radius */
  --radius-input: 12px;
  --radius-icon: 16px;
  --radius-card: 20px;
  --radius-card-lg: 24px;
  --radius-pill: 50px;

  /* Shadows */
  --shadow-1: 0 1px 3px rgba(45, 74, 53, 0.06);
  --shadow-2: 0 2px 0 var(--color-leaf-gray), 0 4px 12px rgba(45, 74, 53, 0.06);
  --shadow-3: 0 4px 0 var(--color-leaf-gray), 0 8px 24px rgba(45, 74, 53, 0.08);
  --shadow-4: 0 8px 0 var(--color-leaf-gray), 0 16px 40px rgba(45, 74, 53, 0.1);

  /* Layout */
  --max-width: 1200px;
  --container-padding: 24px;
}

/* ── Reset ── */
*,
*::before,
*::after {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

html {
  scroll-behavior: smooth;
  -webkit-font-smoothing: antialiased;
}

body {
  font-family: var(--font-body);
  font-weight: 500;
  font-size: 16px;
  line-height: 1.6;
  color: var(--color-body);
  background-color: var(--color-cream);
}

h1, h2, h3, h4, h5, h6 {
  font-family: var(--font-heading);
  color: var(--color-heading);
  line-height: 1.2;
}

h1 { font-size: 36px; font-weight: 700; }
h2 { font-size: 26px; font-weight: 600; }
h3 { font-family: var(--font-body); font-size: 20px; font-weight: 700; }

a {
  color: var(--color-primary);
  text-decoration: none;
}

img {
  max-width: 100%;
  height: auto;
  display: block;
}

/* ── Container ── */
.container {
  max-width: var(--max-width);
  margin: 0 auto;
  padding: 0 var(--container-padding);
}

/* ── Section spacing ── */
.section {
  padding: var(--space-page) 0;
}

/* ═══════════════════════════════════════
   Glass Effects
   ═══════════════════════════════════════ */

.leaf-glass {
  background: rgba(255, 255, 255, 0.45);
  backdrop-filter: blur(16px);
  -webkit-backdrop-filter: blur(16px);
  border: none;
  box-shadow:
    inset 0 1px 2px rgba(255, 255, 255, 0.6),
    0 4px 24px rgba(45, 74, 53, 0.08);
  position: relative;
  overflow: hidden;
}

.leaf-glass::before {
  content: '';
  position: absolute;
  inset: 0;
  border-radius: inherit;
  padding: 1.2px;
  background: linear-gradient(180deg,
    rgba(255,255,255,0.7) 0%,
    rgba(255,255,255,0.25) 30%,
    rgba(255,255,255,0) 50%,
    rgba(94,170,110,0.1) 80%,
    rgba(94,170,110,0.25) 100%);
  -webkit-mask: linear-gradient(#fff 0 0) content-box, linear-gradient(#fff 0 0);
  -webkit-mask-composite: xor;
  mask-composite: exclude;
  pointer-events: none;
}

.warm-glass {
  background: rgba(255, 248, 225, 0.5);
  backdrop-filter: blur(16px);
  -webkit-backdrop-filter: blur(16px);
  border: none;
  box-shadow:
    inset 0 1px 2px rgba(255, 255, 255, 0.5),
    0 4px 24px rgba(213, 165, 32, 0.08);
  position: relative;
  overflow: hidden;
}

.warm-glass::before {
  content: '';
  position: absolute;
  inset: 0;
  border-radius: inherit;
  padding: 1.2px;
  background: linear-gradient(180deg,
    rgba(255,255,255,0.7) 0%,
    rgba(255,255,255,0.2) 30%,
    rgba(255,255,255,0) 50%,
    rgba(245,200,66,0.15) 80%,
    rgba(245,200,66,0.3) 100%);
  -webkit-mask: linear-gradient(#fff 0 0) content-box, linear-gradient(#fff 0 0);
  -webkit-mask-composite: xor;
  mask-composite: exclude;
  pointer-events: none;
}

.frost-glass {
  background: rgba(255, 255, 255, 0.35);
  backdrop-filter: blur(12px);
  -webkit-backdrop-filter: blur(12px);
  box-shadow:
    inset 0 1px 1px rgba(255, 255, 255, 0.5),
    0 2px 16px rgba(0, 0, 0, 0.04);
  position: relative;
  overflow: hidden;
}

.frost-glass::before {
  content: '';
  position: absolute;
  inset: 0;
  border-radius: inherit;
  padding: 1px;
  background: linear-gradient(180deg,
    rgba(255,255,255,0.6) 0%,
    rgba(255,255,255,0.15) 40%,
    rgba(255,255,255,0) 100%);
  -webkit-mask: linear-gradient(#fff 0 0) content-box, linear-gradient(#fff 0 0);
  -webkit-mask-composite: xor;
  mask-composite: exclude;
  pointer-events: none;
}

/* ═══════════════════════════════════════
   Animations
   ═══════════════════════════════════════ */

@keyframes float {
  0%, 100% { transform: translateY(0px); }
  50% { transform: translateY(-12px); }
}

@keyframes floatSlow {
  0%, 100% { transform: translateY(0px); }
  50% { transform: translateY(-8px); }
}

@keyframes fadeUp {
  from {
    opacity: 0;
    transform: translateY(20px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.animate-float { animation: float 4s ease-in-out infinite; }
.animate-float-slow { animation: floatSlow 4s ease-in-out infinite; }
.animate-float-delay-1 { animation: floatSlow 4s ease-in-out infinite 0.5s; }
.animate-float-delay-2 { animation: floatSlow 4s ease-in-out infinite 1s; }

.fade-up {
  opacity: 0;
  transform: translateY(20px);
}

.fade-up.visible {
  animation: fadeUp 0.6s ease-out forwards;
}

/* ═══════════════════════════════════════
   Utility Classes
   ═══════════════════════════════════════ */

.text-heading { color: var(--color-heading); }
.text-body { color: var(--color-body); }
.text-muted { color: var(--color-muted); }
.text-primary { color: var(--color-primary); }
.text-accent { color: var(--color-accent-dark); }

.font-heading { font-family: var(--font-heading); }
.font-body { font-family: var(--font-body); }

.text-center { text-align: center; }

/* ── Gradient backgrounds for sections ── */
.bg-gradient-green {
  background: linear-gradient(160deg, #E8F5E9 0%, #F0F5F1 40%, #FFF8E1 80%, #D4EDDA 100%);
}

.bg-gradient-hero {
  background: linear-gradient(160deg, #7DC08A 0%, #5EAA6E 30%, #D4EDDA 60%, #FFF8E1 100%);
}

.bg-gradient-warm {
  background: linear-gradient(160deg, #FFF8E1 0%, #F0F5F1 50%, #D4EDDA 100%);
}

/* ── Decorative blob ── */
.blob {
  position: absolute;
  border-radius: 50%;
  pointer-events: none;
}

.blob-green {
  background: radial-gradient(circle, rgba(94,170,110,0.2) 0%, transparent 70%);
}

.blob-yellow {
  background: radial-gradient(circle, rgba(245,200,66,0.2) 0%, transparent 70%);
}

.blob-white {
  background: radial-gradient(circle, rgba(255,255,255,0.15) 0%, transparent 70%);
}

/* ═══════════════════════════════════════
   Responsive
   ═══════════════════════════════════════ */

@media (max-width: 1024px) {
  h1 { font-size: 28px; }
  h2 { font-size: 22px; }
}

@media (max-width: 640px) {
  :root {
    --container-padding: 16px;
  }
  h1 { font-size: 24px; }
  h2 { font-size: 20px; }
  .section {
    padding: var(--space-section) 0;
  }
}
```

- [ ] **Step 2: Verify CSS loads correctly**

```bash
npx astro dev
```

Open http://localhost:4321 — page should load without errors (may be blank, but no console errors about CSS).

- [ ] **Step 3: Commit**

```bash
git add src/styles/global.css
git commit -m "feat: add design system global CSS with tokens, glass effects, and animations"
```

---

## Task 3: Base Layout + Shared Components

**Files:**
- Create: `src/layouts/BaseLayout.astro`, `src/layouts/MainLayout.astro`, `src/components/Navbar.astro`, `src/components/MobileMenu.astro`, `src/components/Footer.astro`, `src/components/Button.astro`, `src/components/GlassPanel.astro`, `src/components/SectionHeader.astro`

- [ ] **Step 1: Create BaseLayout.astro**

```astro
---
// src/layouts/BaseLayout.astro
interface Props {
  title: string;
  description?: string;
}

const { title, description = 'WelKids AD3 E K2 — Vững nền miễn dịch, Bé khỏe vươn cao' } = Astro.props;
---

<!DOCTYPE html>
<html lang="vi">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <meta name="description" content={description} />
  <link rel="icon" type="image/svg+xml" href="/favicon.svg" />
  <title>{title} | WelKids</title>
  <link rel="stylesheet" href="/src/styles/global.css" />
</head>
<body>
  <slot />

  <script>
    // Fade-up on scroll observer
    const observer = new IntersectionObserver((entries) => {
      entries.forEach(entry => {
        if (entry.isIntersecting) {
          entry.target.classList.add('visible');
        }
      });
    }, { threshold: 0.1, rootMargin: '-50px' });

    document.querySelectorAll('.fade-up').forEach(el => observer.observe(el));
  </script>
</body>
</html>
```

- [ ] **Step 2: Create Navbar.astro**

```astro
---
// src/components/Navbar.astro
const navLinks = [
  { label: 'Trang chủ', href: '/' },
  { label: 'Phòng khám', href: '/health-check/' },
  { label: 'Lớp học', href: '/videos/' },
  { label: 'Thực hành', href: '/ai-generator/' },
  { label: 'Giải thưởng', href: '/awards/' },
];

const currentPath = Astro.url.pathname;
---

<nav class="navbar">
  <div class="navbar-inner leaf-glass">
    <a href="/" class="navbar-brand">
      <div class="navbar-logo">🦕</div>
      <span class="navbar-title">WelKids</span>
    </a>

    <div class="navbar-links">
      {navLinks.map(link => (
        <a
          href={link.href}
          class:list={['navbar-link', { active: currentPath === link.href }]}
        >
          {link.label}
        </a>
      ))}
    </div>

    <a href="#contact" class="navbar-cta">Liên hệ</a>

    <button class="navbar-hamburger" aria-label="Menu" onclick="document.getElementById('mobile-menu').classList.toggle('open')">
      <span></span>
      <span></span>
      <span></span>
    </button>
  </div>
</nav>

<div id="mobile-menu" class="mobile-menu">
  <div class="mobile-menu-overlay" onclick="this.parentElement.classList.remove('open')"></div>
  <div class="mobile-menu-panel leaf-glass">
    <div class="mobile-menu-header">
      <a href="/" class="navbar-brand">
        <div class="navbar-logo">🦕</div>
        <span class="navbar-title">WelKids</span>
      </a>
      <button class="mobile-menu-close" onclick="document.getElementById('mobile-menu').classList.remove('open')" aria-label="Close">✕</button>
    </div>
    <div class="mobile-menu-links">
      {navLinks.map(link => (
        <a href={link.href} class:list={['mobile-link', { active: currentPath === link.href }]}>
          {link.label}
        </a>
      ))}
    </div>
    <a href="#contact" class="mobile-menu-cta">Liên hệ</a>
  </div>
</div>

<style>
  .navbar {
    position: fixed;
    top: 16px;
    left: 50%;
    transform: translateX(-50%);
    z-index: 50;
    width: calc(100% - 48px);
    max-width: var(--max-width);
  }

  .navbar-inner {
    border-radius: var(--radius-pill);
    padding: 10px 24px;
    display: flex;
    align-items: center;
    justify-content: space-between;
  }

  .navbar-brand {
    display: flex;
    align-items: center;
    gap: 10px;
    text-decoration: none;
  }

  .navbar-logo {
    width: 36px;
    height: 36px;
    background: linear-gradient(135deg, var(--color-primary), var(--color-primary-light));
    border-radius: var(--radius-input);
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 18px;
    box-shadow: 0 2px 8px rgba(94, 170, 110, 0.3);
  }

  .navbar-title {
    font-family: var(--font-heading);
    font-weight: 700;
    font-size: 18px;
    color: var(--color-heading);
  }

  .navbar-links {
    display: flex;
    gap: 28px;
  }

  .navbar-link {
    color: var(--color-body);
    font-weight: 600;
    font-size: 14px;
    text-decoration: none;
    transition: color 0.2s;
  }

  .navbar-link:hover,
  .navbar-link.active {
    color: var(--color-primary);
  }

  .navbar-link.active {
    border-bottom: 2px solid var(--color-primary);
    padding-bottom: 2px;
  }

  .navbar-cta {
    background: linear-gradient(180deg, #6DB87A 0%, #5EAA6E 50%, #4F9C60 100%);
    color: #fff;
    border: none;
    padding: 10px 22px;
    border-radius: var(--radius-pill);
    font-size: 13px;
    font-weight: 700;
    font-family: var(--font-body);
    text-decoration: none;
    box-shadow: 0 2px 0 var(--color-primary-dark), 0 4px 12px rgba(61, 139, 79, 0.2);
    transition: transform 0.15s;
  }

  .navbar-cta:hover { transform: translateY(-1px); }

  .navbar-hamburger {
    display: none;
    flex-direction: column;
    gap: 5px;
    background: none;
    border: none;
    cursor: pointer;
    padding: 8px;
  }

  .navbar-hamburger span {
    display: block;
    width: 22px;
    height: 2px;
    background: var(--color-heading);
    border-radius: 2px;
    transition: 0.2s;
  }

  /* Mobile menu */
  .mobile-menu {
    display: none;
    position: fixed;
    inset: 0;
    z-index: 100;
  }

  .mobile-menu.open { display: block; }

  .mobile-menu-overlay {
    position: absolute;
    inset: 0;
    background: rgba(0, 0, 0, 0.3);
  }

  .mobile-menu-panel {
    position: absolute;
    right: 0;
    top: 0;
    bottom: 0;
    width: 280px;
    border-radius: 0;
    padding: 24px;
    display: flex;
    flex-direction: column;
    gap: 24px;
  }

  .mobile-menu-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
  }

  .mobile-menu-close {
    background: none;
    border: none;
    font-size: 20px;
    color: var(--color-body);
    cursor: pointer;
  }

  .mobile-menu-links {
    display: flex;
    flex-direction: column;
    gap: 16px;
  }

  .mobile-link {
    color: var(--color-body);
    font-weight: 600;
    font-size: 16px;
    text-decoration: none;
    padding: 8px 0;
  }

  .mobile-link.active { color: var(--color-primary); }

  .mobile-menu-cta {
    background: linear-gradient(180deg, #6DB87A 0%, #5EAA6E 50%, #4F9C60 100%);
    color: #fff;
    padding: 14px;
    border-radius: var(--radius-pill);
    font-weight: 700;
    text-align: center;
    text-decoration: none;
    box-shadow: 0 2px 0 var(--color-primary-dark);
  }

  @media (max-width: 1024px) {
    .navbar-links, .navbar-cta { display: none; }
    .navbar-hamburger { display: flex; }
  }
</style>
```

- [ ] **Step 3: Create Footer.astro**

```astro
---
// src/components/Footer.astro
const exploreLinks = [
  { label: 'Trang chủ', href: '/' },
  { label: 'Phòng khám', href: '/health-check/' },
  { label: 'Lớp học', href: '/videos/' },
];

const activityLinks = [
  { label: 'Thực hành AI', href: '/ai-generator/' },
  { label: 'Giải thưởng', href: '/awards/' },
  { label: 'Liên hệ', href: '#contact' },
];
---

<footer class="footer">
  <div class="blob blob-green" style="width:200px;height:200px;top:-60px;right:-40px;"></div>

  <div class="container footer-grid">
    <div class="footer-brand">
      <div class="footer-brand-row">
        <div class="navbar-logo">🦕</div>
        <span class="footer-brand-name">WelKids</span>
      </div>
      <p class="footer-desc">Bộ Tứ Vàng AD₃ E K₂ — Vững nền miễn dịch, Bé khỏe vươn cao.</p>
    </div>

    <div class="footer-col">
      <h4 class="footer-col-title">Khám phá</h4>
      {exploreLinks.map(link => <a href={link.href} class="footer-link">{link.label}</a>)}
    </div>

    <div class="footer-col">
      <h4 class="footer-col-title">Hoạt động</h4>
      {activityLinks.map(link => <a href={link.href} class="footer-link">{link.label}</a>)}
    </div>

    <div class="footer-col">
      <h4 class="footer-col-title">Theo dõi</h4>
      <div class="footer-social">
        <a href="#" class="footer-social-icon">📘</a>
        <a href="#" class="footer-social-icon">📷</a>
        <a href="#" class="footer-social-icon">▶️</a>
      </div>
    </div>
  </div>

  <div class="container footer-bottom">
    <span>© 2026 WelKids. All rights reserved.</span>
    <span>Điều khoản · Chính sách bảo mật</span>
  </div>
</footer>

<style>
  .footer {
    background: #2D4A35;
    color: #fff;
    position: relative;
    overflow: hidden;
    padding: 48px 0 0;
  }

  .footer-grid {
    display: grid;
    grid-template-columns: 2fr 1fr 1fr 1fr;
    gap: 32px;
    position: relative;
    z-index: 1;
  }

  .footer-brand-row {
    display: flex;
    align-items: center;
    gap: 10px;
    margin-bottom: 12px;
  }

  .footer-brand-name {
    font-family: var(--font-heading);
    font-weight: 700;
    font-size: 18px;
  }

  .footer-desc {
    font-size: 13px;
    color: rgba(255,255,255,0.5);
    line-height: 1.7;
    max-width: 280px;
  }

  .footer-col {
    display: flex;
    flex-direction: column;
    gap: 8px;
  }

  .footer-col-title {
    font-family: var(--font-body);
    font-weight: 700;
    font-size: 13px;
    color: rgba(255,255,255,0.7);
    margin-bottom: 4px;
  }

  .footer-link {
    font-size: 13px;
    color: rgba(255,255,255,0.4);
    text-decoration: none;
    transition: color 0.2s;
  }

  .footer-link:hover { color: rgba(255,255,255,0.8); }

  .footer-social {
    display: flex;
    gap: 8px;
  }

  .footer-social-icon {
    width: 36px;
    height: 36px;
    background: rgba(255,255,255,0.08);
    border-radius: 10px;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 16px;
    text-decoration: none;
    transition: background 0.2s;
  }

  .footer-social-icon:hover { background: rgba(255,255,255,0.15); }

  .footer-bottom {
    border-top: 1px solid rgba(255,255,255,0.08);
    margin-top: 32px;
    padding: 20px 0;
    display: flex;
    justify-content: space-between;
    font-size: 12px;
    color: rgba(255,255,255,0.3);
    position: relative;
    z-index: 1;
  }

  @media (max-width: 1024px) {
    .footer-grid { grid-template-columns: 1fr 1fr; gap: 24px; }
  }

  @media (max-width: 640px) {
    .footer-grid { grid-template-columns: 1fr; }
    .footer-bottom { flex-direction: column; gap: 8px; text-align: center; }
  }
</style>
```

- [ ] **Step 4: Create Button.astro**

```astro
---
// src/components/Button.astro
interface Props {
  variant?: 'primary' | 'accent' | 'warm' | 'secondary' | 'ghost' | 'glass';
  size?: 'sm' | 'md' | 'lg';
  href?: string;
  fullWidth?: boolean;
  icon?: string;
}

const {
  variant = 'primary',
  size = 'md',
  href,
  fullWidth = false,
  icon,
} = Astro.props;

const Tag = href ? 'a' : 'button';
---

<Tag
  href={href}
  class:list={['btn', `btn-${variant}`, `btn-${size}`, { 'btn-full': fullWidth }]}
>
  {icon && <span class="btn-icon">{icon}</span>}
  <slot />
</Tag>

<style>
  .btn {
    display: inline-flex;
    align-items: center;
    gap: 8px;
    border: none;
    border-radius: var(--radius-pill);
    font-family: var(--font-body);
    font-weight: 700;
    cursor: pointer;
    text-decoration: none;
    transition: transform 0.15s;
    white-space: nowrap;
  }

  .btn:hover { transform: translateY(-1px); }
  .btn:active { transform: translateY(0); }

  /* Sizes */
  .btn-sm { padding: 9px 20px; font-size: 13px; }
  .btn-md { padding: 12px 28px; font-size: 15px; }
  .btn-lg { padding: 14px 32px; font-size: 16px; letter-spacing: 0.3px; }
  .btn-full { width: 100%; justify-content: center; }

  /* Variants */
  .btn-primary {
    background: linear-gradient(180deg, #6DB87A 0%, #5EAA6E 50%, #4F9C60 100%);
    color: #fff;
    box-shadow: 0 2px 0 #3D8B4F, 0 4px 12px rgba(61,139,79,0.25);
  }

  .btn-accent {
    background: linear-gradient(180deg, #F8D76E 0%, #F5C842 50%, #E8B830 100%);
    color: #6B5010;
    box-shadow: 0 2px 0 #D4A520, 0 4px 12px rgba(213,165,32,0.3);
  }

  .btn-warm {
    background: linear-gradient(180deg, #FFCC80 0%, #FFB74D 50%, #FFA726 100%);
    color: #6B4010;
    box-shadow: 0 2px 0 #E69520, 0 4px 12px rgba(230,149,32,0.25);
  }

  .btn-secondary {
    background: #fff;
    color: var(--color-primary);
    border: 2px solid #C8E0CC;
    box-shadow: 0 2px 0 #C8E0CC, 0 3px 8px rgba(0,0,0,0.04);
  }

  .btn-ghost {
    background: var(--color-mint-gray);
    color: var(--color-body);
    box-shadow: 0 2px 0 #DCE5DD;
  }

  .btn-glass {
    background: rgba(255,255,255,0.6);
    backdrop-filter: blur(8px);
    -webkit-backdrop-filter: blur(8px);
    color: var(--color-body);
    box-shadow: inset 0 1px 2px rgba(255,255,255,0.8), 0 2px 8px rgba(0,0,0,0.04);
  }

  .btn-icon { display: flex; align-items: center; }
</style>
```

- [ ] **Step 5: Create GlassPanel.astro**

```astro
---
// src/components/GlassPanel.astro
interface Props {
  variant?: 'leaf' | 'warm' | 'frost';
  radius?: string;
  padding?: string;
  class?: string;
}

const {
  variant = 'leaf',
  radius = 'var(--radius-card-lg)',
  padding = '32px',
  class: className = '',
} = Astro.props;
---

<div class:list={[`${variant}-glass`, className]} style={`border-radius: ${radius}; padding: ${padding};`}>
  <slot />
</div>
```

- [ ] **Step 6: Create SectionHeader.astro**

```astro
---
// src/components/SectionHeader.astro
interface Props {
  title: string;
  subtitle?: string;
  badge?: string;
  centered?: boolean;
}

const { title, subtitle, badge, centered = true } = Astro.props;
---

<div class:list={['section-header', { 'text-center': centered }]}>
  {badge && (
    <div class="section-badge frost-glass">
      <span class="section-badge-dot"></span>
      <span class="section-badge-text">{badge}</span>
    </div>
  )}
  <h2 class="section-title">{title}</h2>
  {subtitle && <p class="section-subtitle">{subtitle}</p>}
</div>

<style>
  .section-header { margin-bottom: var(--space-spacious); }

  .section-badge {
    display: inline-flex;
    align-items: center;
    gap: 6px;
    border-radius: var(--radius-pill);
    padding: 6px 16px;
    margin-bottom: var(--space-base);
  }

  .section-badge-dot {
    width: 8px;
    height: 8px;
    background: var(--color-primary);
    border-radius: 50%;
    box-shadow: 0 0 8px rgba(94, 170, 110, 0.5);
  }

  .section-badge-text {
    font-size: 12px;
    font-weight: 700;
    color: var(--color-heading);
  }

  .section-title { margin-bottom: var(--space-tight); }

  .section-subtitle {
    font-size: 16px;
    color: var(--color-muted);
    max-width: 600px;
  }

  .text-center .section-subtitle { margin: 0 auto; }
</style>
```

- [ ] **Step 7: Create MainLayout.astro**

```astro
---
// src/layouts/MainLayout.astro
import BaseLayout from './BaseLayout.astro';
import Navbar from '../components/Navbar.astro';
import Footer from '../components/Footer.astro';

interface Props {
  title: string;
  description?: string;
}

const { title, description } = Astro.props;
---

<BaseLayout title={title} description={description}>
  <Navbar />
  <main style="padding-top: 80px;">
    <slot />
  </main>
  <Footer />
</BaseLayout>
```

- [ ] **Step 8: Verify with a test page**

Create a minimal `src/pages/index.astro`:

```astro
---
import MainLayout from '../layouts/MainLayout.astro';
import Button from '../components/Button.astro';
import GlassPanel from '../components/GlassPanel.astro';
import SectionHeader from '../components/SectionHeader.astro';
---

<MainLayout title="Trang chủ">
  <div class="section bg-gradient-hero" style="min-height: 60vh; display: flex; align-items: center; justify-content: center;">
    <div class="container text-center">
      <SectionHeader
        title="Bộ Tứ Vàng WelKids AD₃ E K₂"
        subtitle="Vững nền miễn dịch · Bé khỏe vươn cao"
        badge="Campaign 2026"
      />
      <div style="display: flex; gap: 12px; justify-content: center;">
        <Button variant="glass" size="lg">Khám phá ngay</Button>
        <Button variant="glass" size="lg">Xem video</Button>
      </div>
    </div>
  </div>
</MainLayout>
```

Run: `npx astro dev` — verify navbar, hero, footer all render correctly at http://localhost:4321.

- [ ] **Step 9: Commit**

```bash
git add src/layouts/ src/components/Navbar.astro src/components/Footer.astro src/components/Button.astro src/components/GlassPanel.astro src/components/SectionHeader.astro src/pages/index.astro
git commit -m "feat: add base layouts, navbar, footer, and core components"
```

---

## Task 4: Mock Data Files

**Files:**
- Create: `src/data/questions.json`, `src/data/videos.json`, `src/data/careers.json`, `src/data/gallery.json`

- [ ] **Step 1: Create questions.json**

```json
[
  {
    "id": 1,
    "question": "Bé bao nhiêu tuổi?",
    "subtitle": "Chọn nhóm tuổi phù hợp với bé",
    "options": [
      { "emoji": "👶", "label": "0 - 6 tháng" },
      { "emoji": "🍼", "label": "6 - 12 tháng" },
      { "emoji": "🧒", "label": "1 - 3 tuổi" },
      { "emoji": "👦", "label": "3 - 6 tuổi" }
    ]
  },
  {
    "id": 2,
    "question": "Chế độ ăn của bé?",
    "subtitle": "Bé thường ăn gì trong bữa chính?",
    "options": [
      { "emoji": "🥦", "label": "Rau củ đa dạng" },
      { "emoji": "🍚", "label": "Cơm / cháo chủ yếu" },
      { "emoji": "🥛", "label": "Sữa là chính" },
      { "emoji": "🍖", "label": "Thịt cá đầy đủ" }
    ]
  },
  {
    "id": 3,
    "question": "Bé có hay ốm vặt không?",
    "subtitle": "Tần suất bé bị ốm trong 6 tháng gần đây",
    "options": [
      { "emoji": "💪", "label": "Rất ít" },
      { "emoji": "🤧", "label": "Thỉnh thoảng" },
      { "emoji": "🤒", "label": "Thường xuyên" },
      { "emoji": "😷", "label": "Rất thường xuyên" }
    ]
  },
  {
    "id": 4,
    "question": "Bé có tiếp xúc ánh nắng thường xuyên?",
    "subtitle": "Ánh nắng giúp tổng hợp Vitamin D3 tự nhiên",
    "options": [
      { "emoji": "☀️", "label": "Hàng ngày" },
      { "emoji": "🌤️", "label": "2-3 lần/tuần" },
      { "emoji": "☁️", "label": "Hiếm khi" },
      { "emoji": "🏠", "label": "Không" }
    ]
  },
  {
    "id": 5,
    "question": "Bé có đang bổ sung vitamin?",
    "subtitle": "Tình trạng bổ sung vitamin hiện tại của bé",
    "options": [
      { "emoji": "✅", "label": "Có, đầy đủ" },
      { "emoji": "⚠️", "label": "Có, không đều" },
      { "emoji": "❌", "label": "Chưa bổ sung" },
      { "emoji": "❓", "label": "Không biết" }
    ]
  }
]
```

- [ ] **Step 2: Create videos.json**

```json
[
  {
    "id": 1,
    "title": "Tầm quan trọng của Vitamin D3 cho trẻ",
    "category": "Vitamin & Khoáng chất",
    "duration": "5:30",
    "description": "Tìm hiểu vai trò của Vitamin D3 trong sự phát triển xương và hệ miễn dịch của trẻ nhỏ."
  },
  {
    "id": 2,
    "title": "Cách xây dựng thực đơn dinh dưỡng cho bé 1-3 tuổi",
    "category": "Thực đơn cho bé",
    "duration": "8:15",
    "description": "Hướng dẫn chi tiết cách lên thực đơn cân bằng dinh dưỡng cho bé trong giai đoạn tập ăn."
  },
  {
    "id": 3,
    "title": "Vitamin K2 và sự phát triển xương",
    "category": "Vitamin & Khoáng chất",
    "duration": "4:45",
    "description": "Vai trò của Vitamin K2 trong việc hỗ trợ canxi đến đúng vị trí xương, giúp bé cao lớn."
  },
  {
    "id": 4,
    "title": "Hỏi đáp cùng bác sĩ: Miễn dịch trẻ nhỏ",
    "category": "Chuyên gia tư vấn",
    "duration": "12:00",
    "description": "Bác sĩ giải đáp các thắc mắc phổ biến của phụ huynh về hệ miễn dịch của trẻ."
  },
  {
    "id": 5,
    "title": "5 dấu hiệu bé thiếu vitamin A",
    "category": "Dinh dưỡng cơ bản",
    "duration": "3:50",
    "description": "Nhận biết sớm các dấu hiệu thiếu vitamin A để kịp thời bổ sung cho bé."
  },
  {
    "id": 6,
    "title": "Hướng dẫn bổ sung vitamin đúng cách",
    "category": "Dinh dưỡng cơ bản",
    "duration": "6:20",
    "description": "Liều lượng, thời điểm và cách kết hợp vitamin hiệu quả cho trẻ nhỏ."
  },
  {
    "id": 7,
    "title": "Dinh dưỡng cho bé trong mùa dịch",
    "category": "Chuyên gia tư vấn",
    "duration": "9:10",
    "description": "Chế độ ăn uống giúp tăng cường sức đề kháng cho bé trong mùa dịch bệnh."
  },
  {
    "id": 8,
    "title": "Thực đơn ăn dặm giàu vitamin cho bé 6-12 tháng",
    "category": "Thực đơn cho bé",
    "duration": "7:40",
    "description": "Các công thức ăn dặm đơn giản nhưng giàu dinh dưỡng cho bé giai đoạn đầu."
  }
]
```

- [ ] **Step 3: Create careers.json**

```json
[
  { "id": 1, "emoji": "👨‍⚕️", "label": "Bác sĩ" },
  { "id": 2, "emoji": "🚀", "label": "Phi hành gia" },
  { "id": 3, "emoji": "👨‍🍳", "label": "Đầu bếp" },
  { "id": 4, "emoji": "🎤", "label": "Ca sĩ" },
  { "id": 5, "emoji": "👩‍🎨", "label": "Họa sĩ" },
  { "id": 6, "emoji": "👩‍🏫", "label": "Giáo viên" },
  { "id": 7, "emoji": "👷", "label": "Kỹ sư" },
  { "id": 8, "emoji": "✈️", "label": "Phi công" }
]
```

- [ ] **Step 4: Create gallery.json**

```json
[
  { "id": 1, "name": "Minh Anh", "career": "Bác sĩ", "ticket": "WK-001", "award": "first", "hasVideo": true },
  { "id": 2, "name": "Gia Bảo", "career": "Phi hành gia", "ticket": "WK-002", "award": "second", "hasVideo": true },
  { "id": 3, "name": "Thảo Vy", "career": "Họa sĩ", "ticket": "WK-003", "award": "third", "hasVideo": true },
  { "id": 4, "name": "Đức Minh", "career": "Đầu bếp", "ticket": "WK-004", "award": null, "hasVideo": false },
  { "id": 5, "name": "Hạ Vy", "career": "Ca sĩ", "ticket": "WK-005", "award": null, "hasVideo": false },
  { "id": 6, "name": "Tuấn Kiệt", "career": "Kỹ sư", "ticket": "WK-006", "award": null, "hasVideo": false },
  { "id": 7, "name": "Ngọc Trân", "career": "Giáo viên", "ticket": "WK-007", "award": null, "hasVideo": false },
  { "id": 8, "name": "Bảo Long", "career": "Phi công", "ticket": "WK-008", "award": null, "hasVideo": false },
  { "id": 9, "name": "Mai Chi", "career": "Bác sĩ", "ticket": "WK-009", "award": null, "hasVideo": false },
  { "id": 10, "name": "Hữu Nam", "career": "Phi hành gia", "ticket": "WK-010", "award": null, "hasVideo": false }
]
```

- [ ] **Step 5: Commit**

```bash
git add src/data/
git commit -m "feat: add mock data files for health check, videos, careers, and gallery"
```

---

## Task 5: Reusable Module Components

**Files:**
- Create: `src/components/Card.astro`, `src/components/ChoiceCard.astro`, `src/components/ProgressBar.astro`, `src/components/VideoCard.astro`, `src/components/GalleryItem.astro`

- [ ] **Step 1: Create Card.astro**

```astro
---
// src/components/Card.astro
interface Props {
  title: string;
  description: string;
  icon: string;
  badge?: string;
  badgeVariant?: 'primary' | 'accent' | 'muted';
  gradient: string;
  href?: string;
  buttonLabel?: string;
  buttonVariant?: 'primary' | 'accent' | 'secondary';
  glassVariant?: 'leaf' | 'warm';
}

const {
  title, description, icon, badge, badgeVariant = 'primary',
  gradient, href, buttonLabel, buttonVariant = 'primary',
  glassVariant = 'leaf',
} = Astro.props;

const badgeColors: Record<string, string> = {
  primary: 'background: var(--color-primary); color: #fff;',
  accent: 'background: var(--color-accent); color: #6B5010;',
  muted: 'background: rgba(255,255,255,0.7); backdrop-filter: blur(4px); color: var(--color-body);',
};
---

<div class={`card ${glassVariant}-glass`}>
  <div class="card-header" style={`background: ${gradient};`}>
    <div class="card-icon animate-float-slow">{icon}</div>
    {badge && (
      <div class="card-badge frost-glass">
        <span style={badgeColors[badgeVariant]}>{badge}</span>
      </div>
    )}
  </div>
  <div class="card-body">
    <h3 class="card-title font-heading">{title}</h3>
    <p class="card-desc">{description}</p>
    {buttonLabel && (
      <a href={href || '#'} class={`btn btn-sm btn-${buttonVariant}`}>{buttonLabel}</a>
    )}
  </div>
</div>

<style>
  .card {
    border-radius: var(--radius-card-lg);
    overflow: hidden;
  }

  .card-header {
    height: 160px;
    display: flex;
    align-items: center;
    justify-content: center;
    position: relative;
  }

  .card-icon {
    width: 72px;
    height: 72px;
    background: rgba(255,255,255,0.6);
    backdrop-filter: blur(8px);
    border-radius: var(--radius-card);
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 36px;
    box-shadow: 0 4px 16px rgba(0,0,0,0.08), inset 0 1px 2px rgba(255,255,255,0.8);
  }

  .card-badge {
    position: absolute;
    top: 14px;
    right: 14px;
    border-radius: var(--radius-pill);
    padding: 0;
  }

  .card-badge span {
    display: block;
    padding: 3px 10px;
    border-radius: var(--radius-pill);
    font-size: 11px;
    font-weight: 700;
  }

  .card-body { padding: 22px; }

  .card-title {
    font-weight: 600;
    color: var(--color-heading);
    font-size: 18px;
    margin-bottom: 8px;
  }

  .card-desc {
    font-size: 13px;
    color: var(--color-muted);
    line-height: 1.7;
    margin-bottom: 18px;
  }
</style>
```

- [ ] **Step 2: Create ChoiceCard.astro**

```astro
---
// src/components/ChoiceCard.astro
interface Props {
  emoji: string;
  label: string;
  selected?: boolean;
  value: string;
}

const { emoji, label, selected = false, value } = Astro.props;
---

<button
  class:list={['choice-card', { selected }]}
  data-value={value}
  type="button"
>
  <span class="choice-emoji">{emoji}</span>
  <span class="choice-label">{label}</span>
  {selected && <span class="choice-check">✓ Đã chọn</span>}
</button>

<style>
  .choice-card {
    background: rgba(255,255,255,0.35);
    backdrop-filter: blur(12px);
    border: 2px solid var(--color-leaf-gray);
    border-radius: 16px;
    padding: 18px;
    text-align: center;
    cursor: pointer;
    transition: all 0.2s;
    font-family: var(--font-body);
    width: 100%;
  }

  .choice-card:hover {
    border-color: var(--color-primary-light);
    background: rgba(212, 237, 218, 0.2);
  }

  .choice-card.selected {
    background: rgba(94, 170, 110, 0.12);
    border-color: var(--color-primary);
    box-shadow: 0 0 0 2px rgba(94, 170, 110, 0.2);
  }

  .choice-emoji { display: block; font-size: 28px; margin-bottom: 8px; }
  .choice-label { display: block; font-size: 14px; font-weight: 600; color: var(--color-heading); }
  .choice-check { display: block; font-size: 10px; color: var(--color-primary); font-weight: 700; margin-top: 4px; }
</style>
```

- [ ] **Step 3: Create ProgressBar.astro**

```astro
---
// src/components/ProgressBar.astro
interface Props {
  current: number;
  total: number;
}

const { current, total } = Astro.props;
const percent = (current / total) * 100;
---

<div class="progress-wrapper">
  <div class="progress-bar">
    <div class="progress-fill" style={`width: ${percent}%`}></div>
  </div>
  <span class="progress-label">{current}/{total}</span>
</div>

<style>
  .progress-wrapper {
    display: flex;
    align-items: center;
    gap: 8px;
  }

  .progress-bar {
    flex: 1;
    height: 6px;
    background: rgba(94, 170, 110, 0.15);
    border-radius: 3px;
    overflow: hidden;
  }

  .progress-fill {
    height: 100%;
    background: linear-gradient(90deg, var(--color-primary), var(--color-primary-light));
    border-radius: 3px;
    transition: width 0.3s ease;
  }

  .progress-label {
    font-size: 12px;
    font-weight: 700;
    color: var(--color-primary);
    min-width: 30px;
  }
</style>
```

- [ ] **Step 4: Create VideoCard.astro**

```astro
---
// src/components/VideoCard.astro
interface Props {
  title: string;
  category: string;
  duration: string;
  description: string;
}

const { title, category, duration, description } = Astro.props;
---

<div class="video-card leaf-glass">
  <div class="video-thumb">
    <div class="video-play-btn">▶</div>
    <div class="video-duration frost-glass">{duration}</div>
  </div>
  <div class="video-body">
    <span class="video-category">{category}</span>
    <h3 class="video-title">{title}</h3>
    <p class="video-desc">{description}</p>
  </div>
</div>

<style>
  .video-card {
    border-radius: var(--radius-card-lg);
    overflow: hidden;
    cursor: pointer;
    transition: transform 0.2s;
  }

  .video-card:hover { transform: translateY(-4px); }

  .video-thumb {
    aspect-ratio: 16/9;
    background: linear-gradient(135deg, var(--color-primary-tint), var(--color-primary-light));
    position: relative;
    display: flex;
    align-items: center;
    justify-content: center;
  }

  .video-play-btn {
    width: 48px;
    height: 48px;
    background: rgba(255,255,255,0.8);
    backdrop-filter: blur(8px);
    border-radius: 50%;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 18px;
    color: var(--color-primary);
    box-shadow: 0 4px 16px rgba(0,0,0,0.1);
  }

  .video-duration {
    position: absolute;
    bottom: 10px;
    right: 10px;
    border-radius: var(--radius-pill);
    padding: 3px 10px;
    font-size: 11px;
    font-weight: 700;
    color: var(--color-heading);
  }

  .video-body { padding: 18px; }

  .video-category {
    font-size: 11px;
    font-weight: 700;
    color: var(--color-primary);
    text-transform: uppercase;
    letter-spacing: 0.5px;
  }

  .video-title {
    font-family: var(--font-body);
    font-size: 16px;
    font-weight: 700;
    color: var(--color-heading);
    margin: 6px 0;
    line-height: 1.4;
  }

  .video-desc {
    font-size: 13px;
    color: var(--color-muted);
    line-height: 1.6;
    display: -webkit-box;
    -webkit-line-clamp: 2;
    -webkit-box-orient: vertical;
    overflow: hidden;
  }
</style>
```

- [ ] **Step 5: Create GalleryItem.astro**

```astro
---
// src/components/GalleryItem.astro
interface Props {
  name: string;
  career: string;
  ticket: string;
  award: string | null;
  hasVideo: boolean;
}

const { name, career, ticket, award, hasVideo } = Astro.props;

const awardLabels: Record<string, { label: string; color: string }> = {
  first: { label: '🥇 Giải Nhất', color: '#FFD700' },
  second: { label: '🥈 Giải Nhì', color: '#C0C0C0' },
  third: { label: '🥉 Giải Ba', color: '#CD7F32' },
};

const awardInfo = award ? awardLabels[award] : null;
---

<div class:list={['gallery-item leaf-glass', { 'has-award': !!award }]}>
  <div class="gallery-image">
    <div class="gallery-placeholder">🎨</div>
    {hasVideo && <div class="gallery-play">▶</div>}
    {awardInfo && (
      <div class="gallery-award frost-glass">
        <span>{awardInfo.label}</span>
      </div>
    )}
  </div>
  <div class="gallery-info">
    <div class="gallery-name">{name}</div>
    <div class="gallery-career">{career}</div>
    <div class="gallery-ticket">{ticket}</div>
  </div>
</div>

<style>
  .gallery-item {
    border-radius: var(--radius-card);
    overflow: hidden;
    cursor: pointer;
    transition: transform 0.2s;
  }

  .gallery-item:hover { transform: translateY(-4px); }

  .gallery-image {
    aspect-ratio: 1;
    background: linear-gradient(135deg, var(--color-primary-tint), var(--color-mint-gray));
    position: relative;
    display: flex;
    align-items: center;
    justify-content: center;
  }

  .gallery-placeholder { font-size: 48px; opacity: 0.5; }

  .gallery-play {
    position: absolute;
    inset: 0;
    background: rgba(0,0,0,0.2);
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 32px;
    color: white;
    opacity: 0;
    transition: opacity 0.2s;
  }

  .gallery-item:hover .gallery-play { opacity: 1; }

  .gallery-award {
    position: absolute;
    top: 10px;
    left: 10px;
    border-radius: var(--radius-pill);
    padding: 4px 12px;
    font-size: 11px;
    font-weight: 700;
    color: var(--color-heading);
  }

  .gallery-info { padding: 14px; }
  .gallery-name { font-weight: 700; color: var(--color-heading); font-size: 15px; }
  .gallery-career { font-size: 13px; color: var(--color-primary); margin-top: 2px; }
  .gallery-ticket { font-size: 11px; color: var(--color-muted); margin-top: 4px; }
</style>
```

- [ ] **Step 6: Commit**

```bash
git add src/components/Card.astro src/components/ChoiceCard.astro src/components/ProgressBar.astro src/components/VideoCard.astro src/components/GalleryItem.astro
git commit -m "feat: add reusable module components (Card, ChoiceCard, ProgressBar, VideoCard, GalleryItem)"
```

---

## Task 6: Homepage (Module 1)

**Files:**
- Modify: `src/pages/index.astro` (replace test page with full homepage)

- [ ] **Step 1: Build complete homepage with all sections**

Replace `src/pages/index.astro` with the full homepage containing: Hero, Product Intro, Campaign Overview (5 module cards), Contact Form, all using the glass design system. Include gradient backgrounds with decorative blobs, fade-up animations on scroll.

The hero section should use `bg-gradient-hero`, centered layout with badge ("Campaign 2026"), H1, subtitle, and two glass CTA buttons. Product intro uses a split layout with image placeholder left and 4 vitamin glass cards right. Campaign overview section uses `bg-gradient-green` with 5 module cards (Card component) linking to each section. Contact form uses leaf-glass panel with form fields (name, email, phone, message) and primary submit button. All sections use the `section` class for spacing and `container` for max-width.

- [ ] **Step 2: Verify all sections render**

Run: `npx astro dev` — check http://localhost:4321, scroll through all sections, verify glass effects, gradient backgrounds, and animations work.

- [ ] **Step 3: Commit**

```bash
git add src/pages/index.astro
git commit -m "feat: build complete homepage with hero, product intro, campaign overview, and contact form"
```

---

## Task 7: Health Check Tool (Module 2)

**Files:**
- Create: `src/pages/health-check/index.astro`, `src/pages/health-check/quiz.astro`, `src/pages/health-check/results.astro`

- [ ] **Step 1: Build health check intro page**

`src/pages/health-check/index.astro`: Landing page with leaf-glass panel on gradient background. Brief explanation, estimated time ("Chỉ mất 2 phút"), and "Bắt đầu kiểm tra" primary CTA button linking to `/health-check/quiz/`.

- [ ] **Step 2: Build quiz page with JS-driven steps**

`src/pages/health-check/quiz.astro`: Import questions from `src/data/questions.json`. Render all 5 questions but show one at a time using vanilla JS. Use ProgressBar component at top, leaf-glass main panel, ChoiceCard components in 2x2 grid for options, and navigation buttons (ghost "Quay lại" + primary "Tiếp tục"). The JS handles: showing/hiding steps, tracking selected answers, enabling/disabling next button, and redirecting to `/health-check/results/` on final step.

```astro
<script>
  let currentStep = 0;
  const totalSteps = 5;
  const answers: (string | null)[] = Array(totalSteps).fill(null);

  // ... step navigation, choice selection, progress update logic
</script>
```

- [ ] **Step 3: Build results page**

`src/pages/health-check/results.astro`: Three leaf-glass panels: (a) Nutrition chart — a mock radar/bar chart built with CSS (colored bars for Vitamin A, D3, E, K2 with percentage labels), (b) Text conclusion with summary and highlighted findings in accent-tint boxes, (c) Product recommendation card with image placeholder, benefits list, and CTA buttons. Bottom area: "Chia sẻ kết quả" and "Làm lại bài kiểm tra" buttons.

- [ ] **Step 4: Verify complete flow**

Navigate through: intro → quiz (click answers, progress through 5 steps) → results. All glass effects and transitions should work.

- [ ] **Step 5: Commit**

```bash
git add src/pages/health-check/
git commit -m "feat: build health check tool with intro, quiz flow, and results page"
```

---

## Task 8: Video Learning (Module 3)

**Files:**
- Create: `src/pages/videos/index.astro`

- [ ] **Step 1: Build video gallery page**

Import videos from `src/data/videos.json`. Page structure: SectionHeader with title "Lớp Học Dinh Dưỡng", frost-glass search input, category filter pills (frost-glass, horizontally scrollable — extract unique categories from data), and 3-column grid of VideoCard components. Add vanilla JS for: filtering by category pill click (toggle active state, show/hide matching cards) and search filtering by title. Include a simple modal overlay for video detail when clicking a card (video placeholder 16:9, title, description, close button).

- [ ] **Step 2: Verify filtering and modal**

Click category pills — cards filter. Type in search — cards filter by title. Click a card — modal opens. Close modal — returns to gallery.

- [ ] **Step 3: Commit**

```bash
git add src/pages/videos/
git commit -m "feat: build video learning gallery with category filter, search, and video modal"
```

---

## Task 9: AI Image Generator (Module 4)

**Files:**
- Create: `src/pages/ai-generator/index.astro`

- [ ] **Step 1: Build AI generator with 3 states**

Import careers from `src/data/careers.json`. Single page with 3 states managed by vanilla JS:

**State 1 — Upload & Options**: Warm-glass container. Dashed upload zone with float-animated camera emoji. Career chips (frost-glass pills, selected state with accent ring). Full-width accent "Tạo ảnh bằng AI" button (disabled until "photo uploaded" — simulate with a click on upload zone that adds a placeholder thumbnail).

**State 2 — Processing**: Leaf-glass centered container. CSS spinner animation. "AI đang tạo ảnh..." text. Auto-transition to State 3 after 3 seconds (setTimeout).

**State 3 — Result**: Large image placeholder in leaf-glass frame. Career badge. "Tải về" accent button + "Thử nghề khác" ghost button (resets to State 1). "Gửi ảnh dự thi giải thưởng →" link to `/awards/`.

- [ ] **Step 2: Verify all state transitions**

Click upload zone → placeholder appears. Select career → chip highlights. Click generate → loading state → result state. Click "Thử nghề khác" → back to upload.

- [ ] **Step 3: Commit**

```bash
git add src/pages/ai-generator/
git commit -m "feat: build AI image generator with upload, processing, and result states"
```

---

## Task 10: Award Gallery (Module 5)

**Files:**
- Create: `src/pages/awards/index.astro`

- [ ] **Step 1: Build award gallery page**

Import gallery items from `src/data/gallery.json`. Page structure: SectionHeader "Giải Thưởng & Cuộc Thi", filter tabs (frost-glass pills: Tất cả, Giải Nhất, Giải Nhì, Giải Ba, Yêu thích), 3-4 column grid of GalleryItem components. Filter JS: clicking a tab shows only matching award level items ("Tất cả" shows all). Video modal: clicking an award-winning item (hasVideo=true) opens a modal with video placeholder, story text area, and close button. Bottom CTA section: "Tham gia cuộc thi" accent button linking to `/ai-generator/`.

- [ ] **Step 2: Verify filters and video modal**

Click filter tabs — items filter. Click award winner — video modal opens. Click "Tham gia cuộc thi" — navigates to AI generator.

- [ ] **Step 3: Commit**

```bash
git add src/pages/awards/
git commit -m "feat: build award gallery with filter tabs, video modal, and contest CTA"
```

---

## Task 11: Admin CMS Dashboard (Module 6)

**Files:**
- Create: `src/styles/admin.css`, `src/layouts/AdminLayout.astro`, `src/components/AdminSidebar.astro`, `src/components/StatsCard.astro`, `src/components/DataTable.astro`, `src/pages/admin/index.astro`, `src/pages/admin/users.astro`, `src/pages/admin/videos.astro`, `src/pages/admin/images.astro`, `src/pages/admin/awards.astro`, `src/pages/admin/settings.astro`

- [ ] **Step 1: Create admin.css**

```css
/* src/styles/admin.css */
.admin-body {
  background: #fff;
}

.admin-content {
  margin-left: 260px;
  padding: var(--space-spacious);
  min-height: 100vh;
}

.admin-table {
  width: 100%;
  border-collapse: collapse;
  font-size: 14px;
}

.admin-table th {
  text-align: left;
  padding: 12px 16px;
  font-weight: 700;
  color: var(--color-heading);
  border-bottom: 2px solid var(--color-leaf-gray);
  font-size: 12px;
  text-transform: uppercase;
  letter-spacing: 0.5px;
}

.admin-table td {
  padding: 12px 16px;
  color: var(--color-body);
  border-bottom: 1px solid var(--color-leaf-gray);
}

.admin-table tr:nth-child(even) td {
  background: var(--color-mint-gray);
}

.admin-table .status-badge {
  padding: 3px 10px;
  border-radius: var(--radius-pill);
  font-size: 11px;
  font-weight: 700;
}

.admin-table .status-active { background: var(--color-primary-tint); color: var(--color-primary-dark); }
.admin-table .status-pending { background: var(--color-accent-tint); color: var(--color-accent-dark); }
.admin-table .status-inactive { background: var(--color-error-tint); color: var(--color-error); }

.admin-actions {
  display: flex;
  gap: 8px;
}

.admin-actions button {
  padding: 4px 10px;
  border-radius: var(--radius-input);
  font-size: 12px;
  font-weight: 600;
  font-family: var(--font-body);
  cursor: pointer;
  border: 1px solid var(--color-leaf-gray);
  background: #fff;
  color: var(--color-body);
  transition: background 0.15s;
}

.admin-actions button:hover { background: var(--color-mint-gray); }

@media (max-width: 1024px) {
  .admin-content { margin-left: 72px; }
}

@media (max-width: 640px) {
  .admin-content { margin-left: 0; padding-top: 72px; }
}
```

- [ ] **Step 2: Create AdminSidebar.astro**

Sidebar with dark green background (#2D4A35), logo + "WelKids Admin", nav items with emoji icons (Dashboard, Người dùng, Video, Ảnh AI, Giải thưởng, Cài đặt). Active state highlights. Collapses to icon-only on tablet, hides on mobile with hamburger.

- [ ] **Step 3: Create AdminLayout.astro**

Wraps BaseLayout + AdminSidebar + admin.css import. Adds `admin-body` class to body, `admin-content` wrapper for main slot.

- [ ] **Step 4: Create StatsCard.astro**

Simple card: icon (emoji), number, label, trend indicator (↑ or ↓ with percentage). Light shadow, no glass effect (clean admin style).

- [ ] **Step 5: Create DataTable.astro**

Props: `headers: string[]`, `rows: Record<string, string>[]`. Renders admin-table with proper styling. Each row has action buttons (Xem, Sửa, Xóa — non-functional).

- [ ] **Step 6: Build admin dashboard (index)**

`src/pages/admin/index.astro`: 4 StatsCards in a row (Tổng người dùng: 1,247 / Lượt kiểm tra: 856 / Video đã xem: 3,420 / Ảnh AI: 523). Two chart placeholder boxes. Recent activity list (5 mock items).

- [ ] **Step 7: Build admin list pages (users, videos, images, awards)**

Each page: search bar + filter dropdowns at top, DataTable with appropriate columns and mock data (5-8 rows each), pagination controls at bottom.

- Users: Tên, Email, SĐT, Ngày ĐK, Trạng thái
- Videos: Tiêu đề, Phân loại, Thời lượng, Lượt xem, Trạng thái
- AI Images: Người dùng, Nghề, Ngày tạo, Trạng thái
- Awards: Mã, Người dùng, Nghề, Giải, Video

- [ ] **Step 8: Build admin settings page**

Form layout with sections: Thông tin chung (tên campaign, mô tả, ngày bắt đầu/kết thúc), Campaign config (max ảnh AI, giải thưởng), Email templates (subject + body textarea). All placeholder inputs with labels.

- [ ] **Step 9: Verify admin pages**

Navigate through all admin pages. Sidebar highlights active page. Tables render. Stats cards show. Forms display.

- [ ] **Step 10: Commit**

```bash
git add src/styles/admin.css src/layouts/AdminLayout.astro src/components/AdminSidebar.astro src/components/StatsCard.astro src/components/DataTable.astro src/pages/admin/
git commit -m "feat: build admin CMS dashboard with sidebar, stats, tables, and settings"
```

---

## Task 12: Polish & Responsive

**Files:**
- Modify: Various component and page files

- [ ] **Step 1: Test all pages on mobile viewport (640px)**

Open dev tools, set viewport to 375px width. Navigate every page. Fix any overflow, broken layouts, or unreadable text.

Key responsive fixes to verify:
- Navbar: hamburger visible, links hidden
- Hero: text scales down, buttons stack vertically
- Cards: single column grid
- Health check quiz: 2x1 choice grid instead of 2x2
- Admin: sidebar collapses
- Footer: single column, centered

- [ ] **Step 2: Test tablet viewport (1024px)**

Set viewport to 768px. Verify 2-column grids, navbar still works.

- [ ] **Step 3: Add hover animations**

Ensure all interactive elements have hover states: buttons translateY(-1px), cards translateY(-4px) with enhanced shadow, links color transition.

- [ ] **Step 4: Final build test**

```bash
npx astro build
```

Expected: Build succeeds with 0 errors. Check `dist/` folder contains all pages.

- [ ] **Step 5: Commit**

```bash
git add -A
git commit -m "fix: polish responsive layouts and hover animations across all pages"
```

---

## Task 13: Final Verification

- [ ] **Step 1: Run production build**

```bash
npx astro build && npx astro preview
```

Open http://localhost:4321 and navigate every page:
- [ ] Homepage: hero, product, campaign cards, contact form, footer
- [ ] Health Check: intro → quiz (all 5 steps) → results
- [ ] Videos: gallery, filter, search, modal
- [ ] AI Generator: upload → process → result → retry
- [ ] Awards: gallery, filter, video modal
- [ ] Admin: dashboard, all list pages, settings

- [ ] **Step 2: Commit any final fixes**

```bash
git add -A
git commit -m "chore: final verification and polish"
```
