# Implementation Plan: Portfolio Website Build

## Project Context

**Tech Stack:** Astro 7.3.8 + Tailwind CSS v4.3.3 + Alpine.js 3.17.4  
**Build Commands:** `npm run dev` (dev server), `npm run build` (production build)  
**Current State:** Minimal setup with global.css importing Tailwind, basic index.astro with test markup  
**Design Approach:** Mobile-responsive, placeholder images/CSS initially, hardcoded content in components

## Architecture Decisions

### 1. Alpine.js State Management Strategy
**Decision:** Use Alpine.store() for global modal state instead of x-data on body.  
**Rationale:** Cleaner separation, easier to access from any component, avoids polluting body element. Store will manage `activeModal` (string: null | 'envelope' | 'skills' | 'contact') and `closeModal()` function.

### 2. Component Organization
**Decision:** Group by feature/section (navigation/, hero/, sections/, modals/) not by type.  
**Rationale:** Matches the user's request for easy maintenance, components that change together live together.

### 3. CSS Custom Properties Location
**Decision:** Define all color palette and spacing variables in global.css as CSS custom properties.  
**Rationale:** Tailwind v4 supports CSS variables natively, allows theme consistency without Tailwind config file.

### 4. Layout Pattern
**Decision:** Single MainLayout.astro wrapping page content with Alpine.js initialization in head.  
**Rationale:** Standard Astro pattern, centralizes meta tags and global scripts, Alpine loads once.

### 5. Modal Implementation
**Decision:** All modals render in index.astro with x-show controlled by Alpine.store.  
**Rationale:** Keeps modal markup in DOM for Alpine reactivity, avoids portal complexity, simpler state flow.

## Implementation Steps

- [ ] 1. **Expand global.css with design system foundation**
      Add CSS custom properties for color palette (primary, secondary, accent, neutrals), typography scale, spacing scale, and container widths. Add base styles for smooth scrolling, box-sizing reset, and mobile-first responsive containers.
      Files: `src/styles/global.css`
      Verify: Run `npm run dev` and confirm dev server starts without errors at localhost:4321.

- [ ] 2. **Create MainLayout.astro with Alpine.js initialization**
      Build layout component with html/head/body structure, meta tags (charset, viewport, title slot, description), Alpine.js CDN script in head with x-data for Alpine.store initialization. Store should define `activeModal: null` and `closeModal()` function. Include slot for page content.
      Files: `src/layouts/MainLayout.astro`
      Verify: Run `npm run dev`, visit localhost:4321, open browser console and type `Alpine.store('modals')` — should return object with activeModal and closeModal.

- [ ] 3. **Build TopBar.astro navigation component**
      Fixed top navigation with logo/name on left, navigation links in center (hidden on mobile, visible on tablet+), theme toggle on right. Use Tailwind classes for sticky positioning, backdrop blur, responsive display. Links are anchor tags with smooth scroll to sections.
      Files: `src/components/navigation/TopBar.astro`
      Verify: Run `npm run dev`, inspect page — TopBar should render at top with responsive behavior when resizing viewport.

- [ ] 4. **Build BottomNav.astro mobile navigation component**
      Fixed bottom navigation for mobile/tablet with icon buttons (Home, Envelope, Skills, Contact). Uses Alpine.js to call Alpine.store('modals').activeModal = 'modalName' on click. Hidden on desktop breakpoint.
      Files: `src/components/navigation/BottomNav.astro`
      Verify: Inspect in mobile viewport (375px width) — BottomNav should be visible and fixed at bottom. Click buttons in console, activeModal should change.

- [ ] 5. **Build HeroCard.astro main profile card**
      Card component with profile image placeholder (div with bg-gray-300 for now), name heading, title/role subheading, short bio paragraph, and social links (GitHub, LinkedIn, Email as placeholders). Responsive grid layout, centered on page.
      Files: `src/components/hero/HeroCard.astro`
      Verify: Component renders in page with proper spacing and text hierarchy. Responsive on mobile (320px) and desktop (1024px+).

- [ ] 6. **Build EnvelopeSection.astro decorative section**
      Visual section with envelope icon/graphic (CSS-drawn or placeholder), short tagline text. Clicking this section opens the envelope modal by calling Alpine.store('modals').activeModal = 'envelope'.
      Files: `src/components/hero/EnvelopeSection.astro`
      Verify: Section renders below HeroCard, click event changes activeModal value in Alpine devtools.

- [ ] 7. **Build CareerSection.astro timeline component**
      Timeline of career/education entries with date, title, company/school, description. Hardcoded array of 2-3 entries in component frontmatter. Use CSS for timeline line and dots on left (hidden on mobile, visible on tablet+).
      Files: `src/components/sections/CareerSection.astro`
      Verify: Timeline renders with proper vertical layout and responsive adjustments.

- [ ] 8. **Build ModalEnvelope.astro modal component**
      Modal overlay with centered card showing "about me" content (longer bio, interests, current focus). Modal visibility controlled by x-show="$store.modals.activeModal === 'envelope'". Backdrop click calls $store.modals.closeModal(). Close button in top-right corner.
      Files: `src/components/modals/ModalEnvelope.astro`
      Verify: Modal hidden by default, opens when activeModal set to 'envelope', closes on backdrop/button click. Test with browser console: `Alpine.store('modals').activeModal = 'envelope'`.

- [ ] 9. **Build ModalSkills.astro modal component**
      Modal showing skills categorized by type (Frontend, Backend, Tools, etc.). Hardcoded array in component frontmatter. Each category has title and list of skills with proficiency indicators (basic CSS bars or badges). Same x-show pattern as ModalEnvelope.
      Files: `src/components/modals/ModalSkills.astro`
      Verify: Modal opens when activeModal = 'skills', displays categorized skills list, closes properly.

- [ ] 10. **Build ModalContact.astro modal component**
      Modal with contact information (email, GitHub, LinkedIn) and simple hardcoded contact form (Name, Email, Message fields with Submit button — non-functional, just markup). Same x-show pattern.
      Files: `src/components/modals/ModalContact.astro`
      Verify: Modal opens when activeModal = 'contact', form fields render, modal closes properly.

- [ ] 11. **Rewrite index.astro to compose all components**
      Import MainLayout and all components. Structure: MainLayout wrapper, TopBar, main element with HeroCard, EnvelopeSection, CareerSection, then all three modals at end of body, BottomNav at very end. Import global.css in layout head.
      Files: `src/pages/index.astro`
      Verify: Run `npm run dev`, visit localhost:4321 — full page renders with all sections, modals open/close correctly from BottomNav buttons, page scrolls smoothly, responsive on mobile and desktop.

- [ ] 12. **Final integration test and responsive verification**
      Test complete user flow: load page, scroll through sections, open each modal from BottomNav, close with backdrop and button, verify TopBar navigation (when implemented), test on mobile (375px), tablet (768px), and desktop (1280px) viewports. Check console for errors.
      Files: (no file changes, testing only)
      Verify: Run `npm run build` — build completes without errors. Run `npm run preview` — production preview works correctly at localhost:4321 with all interactive features functional.

## Key Gotchas: Tailwind v4 + Alpine.js in Astro

1. **Alpine.js CDN Loading:** Alpine must load before x-data/x-show directives are parsed. Include script in layout head with defer, or at end of body.

2. **Tailwind v4 @import:** Already correctly using `@import "tailwindcss"` in global.css. No tailwind.config.js needed for v4 unless customizing.

3. **Alpine.store Access:** Store must be initialized before components try to reference it. Initialize in root x-data on body or in Alpine.data() call in head script.

4. **Modal z-index:** Modals need z-index higher than navigation (TopBar and BottomNav). Use z-40 for nav, z-50 for modal overlays.

5. **Astro Component Scripts:** Alpine directives (x-data, x-show, @click) work directly in Astro template HTML, no special Astro script needed.

6. **CSS Custom Properties in Tailwind:** Define custom properties in @theme or :root, then reference with theme() function or directly in arbitrary values like `bg-[var(--color-primary)]`.

7. **Mobile-First Responsive:** Write mobile styles first (no breakpoint prefix), then add sm:, md:, lg: for larger screens. BottomNav hidden on lg+, TopBar links hidden until md+.

8. **Smooth Scroll Anchors:** Add `scroll-behavior: smooth;` to html in global.css. Section IDs should match href="#sectionId" in TopBar links.

## File Responsibilities Summary

| File | Purpose |
|------|---------|
| `global.css` | Design tokens (colors, spacing, typography), base styles, Tailwind import |
| `MainLayout.astro` | HTML structure, meta tags, Alpine.store initialization, global scripts |
| `TopBar.astro` | Desktop navigation, logo, menu links, theme toggle |
| `BottomNav.astro` | Mobile navigation bar with modal trigger buttons |
| `HeroCard.astro` | Main profile card with photo, name, bio, social links |
| `EnvelopeSection.astro` | Decorative section that opens envelope modal on click |
| `CareerSection.astro` | Career/education timeline with hardcoded entries |
| `ModalEnvelope.astro` | "About me" modal with extended bio |
| `ModalSkills.astro` | Skills showcase modal with categories and proficiency |
| `ModalContact.astro` | Contact info and form modal (form non-functional) |
| `index.astro` | Page composition, imports and arranges all components |

## Alpine.js State Flow Diagram

```
BottomNav Button Click
    ↓
@click="$store.modals.activeModal = 'envelope'"
    ↓
Alpine.store('modals').activeModal = 'envelope'
    ↓
ModalEnvelope x-show="$store.modals.activeModal === 'envelope'" → true
    ↓
Modal renders (display: block)
    ↓
User clicks backdrop or close button
    ↓
@click="$store.modals.closeModal()"
    ↓
Alpine.store('modals').activeModal = null
    ↓
All modals x-show → false, hidden again
```

## CSS Variables to Define in global.css

```css
:root {
  /* Color Palette */
  --color-primary: #3b82f6;      /* blue-500 */
  --color-secondary: #8b5cf6;    /* violet-500 */
  --color-accent: #f59e0b;       /* amber-500 */
  --color-neutral-50: #fafafa;
  --color-neutral-100: #f4f4f5;
  --color-neutral-800: #27272a;
  --color-neutral-900: #18181b;
  
  /* Typography */
  --font-sans: system-ui, -apple-system, sans-serif;
  --font-mono: 'Courier New', monospace;
  
  /* Spacing */
  --spacing-section: 4rem;       /* 64px */
  --spacing-container: 1rem;     /* 16px */
  
  /* Container */
  --container-max: 1280px;
  
  /* Z-index layers */
  --z-nav: 40;
  --z-modal: 50;
}
```
