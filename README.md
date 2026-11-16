# Andriy Novan Rahman - Portfolio

Courtroom-themed portfolio website built with Astro, featuring dark/light mode and interactive elements.

## 🎨 Features

- **Courtroom Theme**: Ace Attorney-inspired design with wood paneling and courtroom aesthetics
- **Dark/Light Mode**: Toggle via hanging lamp with smooth transitions
- **Interactive Components**: 
  - Photo card with hover effects
  - Testimony plaque with typewriter effect and audio
  - Wall clock showing Jakarta time
  - Secure contact form with Web3Forms
- **Responsive Design**: Mobile-first approach with breakpoints for all devices
- **Performance**: Optimized with Astro's zero-JS by default approach

## 🚀 Project Structure

```text
/
├── public/
│   ├── documents/          # CV and downloadable files
│   ├── icons/              # UI icons
│   └── images/             # Photos and images
├── src/
│   ├── components/
│   │   ├── decoration/     # HangingLamp
│   │   ├── footer/         # JudicialFooter
│   │   ├── hero/           # HeroCard, PhotoCard, TestimonyPlaque
│   │   ├── hud/            # WallClock
│   │   ├── modals/         # ModalContact
│   │   ├── navigation/     # TopBar, BottomNav
│   │   └── sections/       # CareerPlaque, CareerTimeline
│   ├── layouts/
│   │   └── MainLayout.astro
│   ├── pages/
│   │   └── index.astro
│   └── styles/
│       └── global.css
└── package.json
```

## 🧞 Commands

| Command                   | Action                                           |
| :------------------------ | :----------------------------------------------- |
| `npm install`             | Install dependencies                             |
| `npm run dev`             | Start dev server at `localhost:4321`             |
| `npm run build`           | Build production site to `./dist/`               |
| `npm run preview`         | Preview build locally before deploying           |

## 🛠️ Tech Stack

- **Framework**: Astro 5.x
- **Styling**: Tailwind CSS + Custom CSS
- **Interactivity**: Alpine.js
- **Fonts**: Special Elite (Google Fonts), VT323
- **Form**: Web3Forms
- **Deployment**: GitHub Pages / Netlify

## 📝 Environment Variables

Create a `.env` file in the root:

```env
# Web3Forms Access Key
PUBLIC_WEB3FORMS_KEY=your_access_key_here
```

## 🎯 Key Components

### PhotoCard
Photo ID card with witness-style layout and hover effects.

### TestimonyPlaque
Text display with typewriter animation and Web Audio API sound effects.

### WallClock
Real-time clock showing Jakarta timezone (WIB) with status indicator.

### ModalContact
Secure contact form with 6-layer security (rate limiting, XSS protection, CSRF, honeypot, validation).

### JudicialFooter
Clean footer with contact links and hire button.

## 🌐 Browser Support

- Chrome/Edge (latest)
- Firefox (latest)
- Safari (latest)
- Mobile browsers (iOS Safari, Chrome Mobile)

## 📄 License

Personal portfolio project - All rights reserved

---

Built with ⚖️ by Andriy Novan Rahman

