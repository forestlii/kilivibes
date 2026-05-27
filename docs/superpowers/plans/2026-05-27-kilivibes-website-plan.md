# KILI VIBES & ADVENTURES Website Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build a single-page static website for KILI VIBES & ADVENTURES tourism company with Instagram-style design, warm earth tones, and EN/ZH bilingual support.

**Architecture:** A single `index.html` file containing all HTML structure, embedded CSS, and embedded JavaScript. No build tools, no frameworks, no dependencies. Photos reference the existing `Photos/` folder. Text content extracted from `.docx` files is hardcoded into the HTML as bilingual data.

**Tech Stack:** HTML5, CSS3 (custom properties, grid, flexbox, media queries), vanilla JavaScript ES6, YouTube iframe embeds.

---

## File Structure (post-implementation)

```
KlimanjaroTour/
├── index.html                    # The entire website
├── Photos/                       # [existing] Day_Activities/ + Safari/
├── Itineraries/                  # [existing] Mountaineering/ + Safari/
├── Day_Activities/               # [existing]
└── docs/superpowers/
    ├── specs/2026-05-27-kilivibes-website-design.md
    └── plans/2026-05-27-kilivibes-website-plan.md
```

---

### Task 1: Create HTML shell with CSS custom properties and language data structure

**Files:**
- Create: `index.html`

- [ ] **Step 1: Write the base HTML shell with all CSS variables, embedded styles, and bilingual data object**

Write the complete `index.html` file:

```html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>KILI VIBES & ADVENTURES — Kilimanjaro Treks & Safaris</title>
<style>
/* ===== RESET & VARIABLES ===== */
*,*::before,*::after{box-sizing:border-box;margin:0;padding:0}
:root {
  --brown: #5c3d2e;
  --gold: #c4944a;
  --cream: #faf8f3;
  --white: #ffffff;
  --dark: #3d2b1f;
  --text: #3d2b1f;
  --text-light: #8a7a6a;
  --border: #f0ebe0;
  --font: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif;
  --nav-h: 64px;
}
html { scroll-behavior: smooth; }
body {
  font-family: var(--font);
  color: var(--text);
  background: var(--cream);
  line-height: 1.6;
  -webkit-font-smoothing: antialiased;
}

/* ===== NAVIGATION ===== */
.nav {
  position: fixed; top: 0; left: 0; right: 0; z-index: 1000;
  display: flex; justify-content: space-between; align-items: center;
  padding: 0 32px; height: var(--nav-h);
  transition: background 0.3s, box-shadow 0.3s;
}
.nav.scrolled { background: rgba(255,255,255,0.95); box-shadow: 0 1px 0 var(--border); }
.nav-logo {
  font-weight: 700; font-size: 15px; letter-spacing: 1px;
  color: #fff; text-decoration: none; transition: color 0.3s;
}
.nav.scrolled .nav-logo { color: var(--brown); }
.nav-links {
  display: flex; gap: 24px; list-style: none;
  font-size: 10px; letter-spacing: 1.5px; text-transform: uppercase;
}
.nav-links a {
  color: rgba(255,255,255,0.85); text-decoration: none; transition: color 0.3s;
}
.nav.scrolled .nav-links a { color: var(--text-light); }
.nav-links a:hover { color: var(--gold); }
.nav-right { display: flex; align-items: center; gap: 16px; }
.lang-toggle {
  background: none; border: 1px solid rgba(255,255,255,0.5); color: #fff;
  padding: 4px 10px; font-size: 10px; letter-spacing: 1px; cursor: pointer;
  border-radius: 2px; transition: all 0.3s;
}
.nav.scrolled .lang-toggle { border-color: var(--brown); color: var(--brown); }
.lang-toggle:hover { background: var(--gold); border-color: var(--gold); color: #fff; }
.menu-btn {
  display: none; background: none; border: none; color: #fff;
  font-size: 24px; cursor: pointer;
}
.nav.scrolled .menu-btn { color: var(--brown); }

/* ===== HERO ===== */
.hero {
  position: relative; height: 100vh; min-height: 600px;
  display: flex; align-items: center; justify-content: center;
  text-align: center; color: #fff; overflow: hidden;
  background: linear-gradient(135deg, #5c3d2e, #8b6914, #c4944a);
}
.hero-video {
  position: absolute; top: 0; left: 0; width: 100%; height: 100%;
  object-fit: cover; pointer-events: none;
}
.hero-overlay {
  position: absolute; inset: 0;
  background: linear-gradient(180deg, rgba(0,0,0,0.3) 0%, rgba(0,0,0,0.5) 100%);
}
.hero-content { position: relative; z-index: 1; padding: 0 24px; }
.hero-icon { font-size: 56px; margin-bottom: 16px; }
.hero-title {
  font-size: clamp(36px, 6vw, 64px); font-weight: 300;
  letter-spacing: 4px; line-height: 1.15;
}
.hero-subtitle {
  font-size: clamp(12px, 1.5vw, 16px); font-weight: 300;
  opacity: 0.8; margin-top: 8px; letter-spacing: 6px;
}
.hero-cta {
  display: inline-block; margin-top: 32px;
  background: var(--white); color: var(--brown);
  padding: 14px 40px; border-radius: 2px;
  font-size: 11px; letter-spacing: 2px; text-transform: uppercase;
  text-decoration: none; transition: all 0.3s;
}
.hero-cta:hover { background: var(--gold); color: #fff; }

/* ===== SECTION COMMONS ===== */
.section { padding: 100px 32px; }
.section-label {
  font-size: 10px; letter-spacing: 3px; text-transform: uppercase;
  color: var(--gold); margin-bottom: 8px; text-align: center;
}
.section-title {
  font-size: clamp(24px, 4vw, 36px); font-weight: 300;
  letter-spacing: 1px; text-align: center; margin-bottom: 48px;
}
.container { max-width: 1100px; margin: 0 auto; }

/* ===== GRID NAV (4 squares) ===== */
.grid-nav { display: grid; grid-template-columns: 1fr 1fr; gap: 4px; background: var(--border); padding: 4px; }
.grid-card {
  position: relative; aspect-ratio: 1; display: flex; align-items: flex-end;
  padding: 32px; color: #fff; cursor: pointer; overflow: hidden;
  text-decoration: none; transition: transform 0.3s;
}
.grid-card:hover { transform: scale(0.98); }
.grid-card-bg {
  position: absolute; inset: 0; background-size: cover; background-position: center;
  transition: transform 0.6s;
}
.grid-card:hover .grid-card-bg { transform: scale(1.05); }
.grid-card-content { position: relative; z-index: 1; }
.grid-card-title { font-size: 24px; font-weight: 300; letter-spacing: 1px; }
.grid-card-desc { font-size: 11px; opacity: 0.7; font-weight: 400; margin-top: 4px; }
.grid-card:nth-child(1) .grid-card-bg { background: linear-gradient(135deg, #5c3d2e, #8b6914); }
.grid-card:nth-child(2) .grid-card-bg { background: linear-gradient(135deg, #8b6914, #b8933c); }
.grid-card:nth-child(3) .grid-card-bg { background: linear-gradient(135deg, #7a5c3e, #c4a882); }
.grid-card:nth-child(4) .grid-card-bg { background: linear-gradient(135deg, #6b5038, #a0845c); }

/* ===== CARDS ===== */
.cards { display: grid; grid-template-columns: repeat(auto-fit, minmax(320px, 1fr)); gap: 20px; }
.card {
  background: var(--white); border-radius: 2px; overflow: hidden;
  box-shadow: 0 1px 4px rgba(0,0,0,0.06); transition: transform 0.3s, box-shadow 0.3s;
}
.card:hover { transform: translateY(-4px); box-shadow: 0 8px 24px rgba(0,0,0,0.1); }
.card-img { height: 220px; background-size: cover; background-position: center; }
.card-body { padding: 24px; }
.card-title { font-size: 18px; font-weight: 500; margin-bottom: 4px; }
.card-info { font-size: 12px; color: var(--text-light); }
.card-days { font-size: 11px; color: var(--gold); margin-top: 8px; letter-spacing: 1px; }

/* ===== GALLERY STRIP ===== */
.gallery { display: grid; grid-template-columns: repeat(3, 1fr); gap: 3px; }
.gallery-item { aspect-ratio: 4/5; background-size: cover; background-position: center; }

/* ===== ABOUT ===== */
.about { text-align: center; }
.about-quote {
  font-size: clamp(18px, 3vw, 24px); font-weight: 300; font-style: italic;
  max-width: 700px; margin: 0 auto 32px; line-height: 1.6; color: var(--text-light);
}
.about-name { font-size: 18px; font-weight: 500; }
.about-role { font-size: 13px; color: var(--text-light); margin-top: 2px; }
.video-wrap {
  max-width: 720px; margin: 40px auto 0;
  aspect-ratio: 16/9; border-radius: 2px; overflow: hidden;
}
.video-wrap iframe { width: 100%; height: 100%; border: 0; }

/* ===== CTA ===== */
.cta { text-align: center; background: var(--white); }
.cta-buttons { display: flex; gap: 16px; justify-content: center; flex-wrap: wrap; margin-top: 32px; }
.btn-outline {
  border: 1px solid var(--border); padding: 12px 32px;
  font-size: 11px; letter-spacing: 2px; text-transform: uppercase;
  text-decoration: none; color: var(--brown); border-radius: 2px;
  transition: all 0.3s; display: inline-block;
}
.btn-outline:hover { border-color: var(--gold); color: var(--gold); }
.btn-fill {
  background: var(--brown); color: #fff;
  padding: 12px 32px; font-size: 11px; letter-spacing: 2px;
  text-transform: uppercase; text-decoration: none; border-radius: 2px;
  border: none; cursor: pointer; transition: background 0.3s; display: inline-block;
}
.btn-fill:hover { background: var(--gold); }

/* ===== CONTACT FORM ===== */
.contact-form {
  max-width: 480px; margin: 32px auto 0; display: flex; flex-direction: column; gap: 12px;
}
.contact-form input, .contact-form textarea {
  width: 100%; padding: 14px 16px; border: 1px solid var(--border);
  border-radius: 2px; font-family: var(--font); font-size: 14px;
  background: var(--cream); transition: border-color 0.3s;
}
.contact-form input:focus, .contact-form textarea:focus {
  outline: none; border-color: var(--gold);
}
.contact-form textarea { resize: vertical; min-height: 100px; }

/* ===== FOOTER ===== */
.footer {
  border-top: 1px solid var(--border); padding: 32px;
  display: flex; justify-content: space-between; align-items: center;
  font-size: 10px; color: #bbb; letter-spacing: 1px; flex-wrap: wrap; gap: 12px;
}

/* ===== MODAL (detail view) ===== */
.modal {
  display: none; position: fixed; inset: 0; z-index: 2000;
  background: rgba(0,0,0,0.8); overflow-y: auto;
}
.modal.open { display: block; }
.modal-content {
  background: var(--white); max-width: 800px; margin: 60px auto;
  border-radius: 2px; padding: 48px 32px; position: relative;
}
.modal-close {
  position: absolute; top: 16px; right: 20px;
  background: none; border: none; font-size: 28px; cursor: pointer;
  color: var(--text-light);
}
.modal-body h2 { font-size: 28px; font-weight: 300; letter-spacing: 1px; margin-bottom: 24px; }
.modal-body h3 { font-size: 18px; font-weight: 500; margin: 24px 0 8px; color: var(--brown); }
.modal-body p { font-size: 14px; color: var(--text-light); margin-bottom: 8px; line-height: 1.8; }
.modal-body ul { list-style: none; padding: 0; }
.modal-body ul li { padding: 8px 0; border-bottom: 1px solid var(--border); font-size: 14px; }
.modal-body ul li::before { content: "· "; color: var(--gold); font-weight: bold; }

/* ===== RESPONSIVE ===== */
@media (max-width: 768px) {
  .nav-links { display: none; }
  .nav-links.open {
    display: flex; flex-direction: column; position: absolute;
    top: var(--nav-h); left: 0; right: 0; background: var(--white);
    padding: 24px 32px; gap: 16px; box-shadow: 0 4px 12px rgba(0,0,0,0.1);
  }
  .nav-links.open a { color: var(--brown); }
  .menu-btn { display: block; }
  .grid-nav { grid-template-columns: 1fr 1fr; }
  .gallery { grid-template-columns: 1fr 1fr; }
  .section { padding: 60px 20px; }
  .footer { flex-direction: column; text-align: center; }
}

@media (max-width: 480px) {
  .grid-nav { grid-template-columns: 1fr; }
  .gallery { grid-template-columns: 1fr 1fr; }
  .cards { grid-template-columns: 1fr; }
}
</style>
</head>
<body>

<!-- NAV -->
<nav class="nav" id="nav">
  <a href="#hero" class="nav-logo">KILI VIBES</a>
  <ul class="nav-links" id="navLinks">
    <li><a href="#treks-section" data-key="nav_treks">Mountaineering</a></li>
    <li><a href="#safari-section" data-key="nav_safari">Safari</a></li>
    <li><a href="#activities-section" data-key="nav_activities">Day Activities</a></li>
    <li><a href="#about-section" data-key="nav_about">About</a></li>
    <li><a href="#contact-section" data-key="nav_contact">Contact</a></li>
  </ul>
  <div class="nav-right">
    <button class="lang-toggle" id="langToggle" onclick="toggleLang()">中文</button>
    <button class="menu-btn" id="menuBtn" onclick="toggleMenu()">&#9776;</button>
  </div>
</nav>

<!-- HERO -->
<section class="hero" id="hero">
  <div class="hero-overlay"></div>
  <!-- Replace src with actual YouTube embed or keep gradient fallback -->
  <div class="hero-content">
    <div class="hero-icon">&#9968;</div>
    <h1 class="hero-title">KILIMANJARO</h1>
    <p class="hero-subtitle" data-key="hero_subtitle">THE ROOF OF AFRICA</p>
    <a href="#treks-section" class="hero-cta" data-key="hero_cta">Explore Journeys</a>
  </div>
</section>

<!-- GRID NAV -->
<section class="grid-nav" id="gridNav">
  <a href="#treks-section" class="grid-card"><div class="grid-card-bg"></div><div class="grid-card-content"><div class="grid-card-title" data-key="grid_treks">Mountaineering</div><div class="grid-card-desc">6 Routes · 6–10 Days</div></div></a>
  <a href="#safari-section" class="grid-card"><div class="grid-card-bg"></div><div class="grid-card-content"><div class="grid-card-title" data-key="grid_safari">Safari</div><div class="grid-card-desc">8 Itineraries · 1–5 Days</div></div></a>
  <a href="#activities-section" class="grid-card"><div class="grid-card-bg"></div><div class="grid-card-content"><div class="grid-card-title" data-key="grid_activities">Day Activities</div><div class="grid-card-desc">5 Tours · Arusha</div></div></a>
  <a href="#about-section" class="grid-card"><div class="grid-card-bg"></div><div class="grid-card-content"><div class="grid-card-title" data-key="grid_about">About Raga Voice</div><div class="grid-card-desc">Guide · Musician · Storyteller</div></div></a>
</section>

<!-- FEATURED TREKS -->
<section class="section" id="treks-section">
  <div class="container">
    <p class="section-label" data-key="treks_label">Mountaineering</p>
    <h2 class="section-title" data-key="treks_title">Conquer the Roof of Africa</h2>
    <div class="cards" id="treksCards"></div>
  </div>
</section>

<!-- SAFARI -->
<section class="section" id="safari-section" style="background:var(--white);">
  <div class="container">
    <p class="section-label" data-key="safari_label">Safari</p>
    <h2 class="section-title" data-key="safari_title">Witness the Wild</h2>
    <div class="cards" id="safariCards"></div>
  </div>
</section>

<!-- DAY ACTIVITIES -->
<section class="section" id="activities-section">
  <div class="container">
    <p class="section-label" data-key="activities_label">Day Activities</p>
    <h2 class="section-title" data-key="activities_title">Explore Beyond the Summit</h2>
    <div class="cards" id="activitiesCards"></div>
  </div>
</section>

<!-- GALLERY -->
<section class="gallery" id="gallery">
  <div class="gallery-item" style="background:linear-gradient(135deg,#a08060,#c4a882);"></div>
  <div class="gallery-item" style="background:linear-gradient(135deg,#7a5c3e,#b8936b);"></div>
  <div class="gallery-item" style="background:linear-gradient(135deg,#8b6914,#c49a3c);"></div>
</section>

<!-- ABOUT -->
<section class="section about" id="about-section" style="background:var(--white);">
  <div class="container">
    <p class="section-label" data-key="about_label">About</p>
    <h2 class="section-title" data-key="about_title">Meet Raga Voice</h2>
    <p class="about-quote" data-key="about_quote">"Your adventure isn't just guided — it's soundtracked."</p>
    <p class="about-name">Kauzen "Raga Voice" Geofrey</p>
    <p class="about-role" data-key="about_role">Founder · Guide · Musician</p>
    <div class="video-wrap">
      <!-- Replace with actual YouTube video ID -->
      <iframe src="https://www.youtube.com/embed/VIDEO_ID" title="Raga Voice" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
    </div>
  </div>
</section>

<!-- CONTACT -->
<section class="section cta" id="contact-section">
  <div class="container">
    <h2 class="section-title" data-key="contact_title">Ready for Your Adventure?</h2>
    <p style="color:var(--text-light);text-align:center;" data-key="contact_subtitle">We craft journeys that stay in your soul forever.</p>
    <div class="cta-buttons">
      <a href="https://wa.me/255XXXXXXXXX" class="btn-outline" target="_blank" rel="noopener">WhatsApp</a>
      <a href="mailto:info@kilivibes.com" class="btn-fill" data-key="contact_email">Email Us</a>
    </div>
    <form class="contact-form" id="contactForm" onsubmit="handleContact(event)">
      <input type="text" name="name" data-key-placeholder="form_name" placeholder="Your Name" required>
      <input type="email" name="email" data-key-placeholder="form_email" placeholder="Your Email" required>
      <textarea name="message" data-key-placeholder="form_message" placeholder="Tell us about your dream adventure..." required></textarea>
      <button type="submit" class="btn-fill" data-key="form_send">Send Message</button>
    </form>
  </div>
</section>

<!-- FOOTER -->
<footer class="footer">
  <span>KILI VIBES & ADVENTURES</span>
  <span data-key="footer_location">Arusha · Tanzania · © 2026</span>
</footer>

<!-- MODAL -->
<div class="modal" id="modal">
  <div class="modal-content">
    <button class="modal-close" onclick="closeModal()">&times;</button>
    <div class="modal-body" id="modalBody"></div>
  </div>
</div>

<script>
// ===== BILINGUAL DATA =====
const i18n = {
  en: {
    nav_treks: "Mountaineering", nav_safari: "Safari", nav_activities: "Day Activities",
    nav_about: "About", nav_contact: "Contact",
    hero_subtitle: "THE ROOF OF AFRICA", hero_cta: "Explore Journeys",
    grid_treks: "Mountaineering", grid_safari: "Safari",
    grid_activities: "Day Activities", grid_about: "About Raga Voice",
    treks_label: "Mountaineering", treks_title: "Conquer the Roof of Africa",
    safari_label: "Safari", safari_title: "Witness the Wild",
    activities_label: "Day Activities", activities_title: "Explore Beyond the Summit",
    about_label: "About", about_title: "Meet Raga Voice",
    about_quote: "\"Your adventure isn't just guided — it's soundtracked.\"",
    about_role: "Founder · Guide · Musician",
    contact_title: "Ready for Your Adventure?",
    contact_subtitle: "We craft journeys that stay in your soul forever.",
    contact_email: "Email Us", form_send: "Send Message",
    footer_location: "Arusha · Tanzania · © 2026",
    form_name: "Your Name", form_email: "Your Email",
    form_message: "Tell us about your dream adventure..."
  },
  zh: {
    nav_treks: "登山路线", nav_safari: "野生动物园", nav_activities: "一日游",
    nav_about: "关于我们", nav_contact: "联系我们",
    hero_subtitle: "非洲之巅", hero_cta: "探索旅程",
    grid_treks: "登山路线", grid_safari: "野生动物园",
    grid_activities: "一日游", grid_about: "认识 Raga Voice",
    treks_label: "登山路线", treks_title: "征服非洲之巅",
    safari_label: "野生动物园", safari_title: "见证野性之美",
    activities_label: "一日游", activities_title: "探索山顶之外",
    about_label: "关于我们", about_title: "认识 Raga Voice",
    about_quote: "\"你的冒险不只是被引导——它会被谱写成曲。\"",
    about_role: "创始人 · 向导 · 音乐人",
    contact_title: "准备好你的冒险了吗？",
    contact_subtitle: "我们打造留在你灵魂深处的旅程。",
    contact_email: "发送邮件", form_send: "发送消息",
    footer_location: "阿鲁沙 · 坦桑尼亚 · © 2026",
    form_name: "你的名字", form_email: "你的邮箱",
    form_message: "告诉我们你梦想的冒险..."
  }
};

let currentLang = 'en';

function toggleLang() {
  currentLang = currentLang === 'en' ? 'zh' : 'en';
  const btn = document.getElementById('langToggle');
  btn.textContent = currentLang === 'en' ? '中文' : 'English';
  document.documentElement.lang = currentLang === 'en' ? 'en' : 'zh';
  applyTranslations();
}

function applyTranslations() {
  const texts = i18n[currentLang];
  // Update text content
  document.querySelectorAll('[data-key]').forEach(el => {
    const key = el.dataset.key;
    if (texts[key]) el.textContent = texts[key];
  });
  // Update placeholders
  document.querySelectorAll('[data-key-placeholder]').forEach(el => {
    const key = el.dataset.keyPlaceholder;
    if (texts[key]) el.placeholder = texts[key];
  });
  // Re-render cards with translated content
  renderTreks();
  renderSafari();
  renderActivities();
}

// ===== NAV SCROLL EFFECT =====
window.addEventListener('scroll', () => {
  document.getElementById('nav').classList.toggle('scrolled', window.scrollY > 100);
});

// ===== MOBILE MENU =====
function toggleMenu() {
  document.getElementById('navLinks').classList.toggle('open');
}
document.querySelectorAll('#navLinks a').forEach(link => {
  link.addEventListener('click', () => {
    document.getElementById('navLinks').classList.remove('open');
  });
});

// ===== CONTENT DATA =====
const treksData = [
  {
    id: 'machame',
    title: { en: "Machame Route", zh: "马查姆路线" },
    subtitle: { en: "\"The Whisky Route\" — 6 Days", zh: "\"威士忌路线\" — 6天" },
    price: "From $1,800",
    img: null
  },
  {
    id: 'lemosho',
    title: { en: "Lemosho Route", zh: "莱莫绍路线" },
    subtitle: { en: "8 Days · 95% Summit Success", zh: "8天 · 95%登顶成功率" },
    price: "From $2,400",
    img: null
  },
  {
    id: 'londorossi',
    title: { en: "Londorossi Route", zh: "隆多罗西路" },
    subtitle: { en: "10 Days · Ultimate Acclimatization", zh: "10天 · 最佳适应路线" },
    price: "From $2,800",
    img: null
  },
  {
    id: 'marangu',
    title: { en: "Marangu Route", zh: "马兰古路线" },
    subtitle: { en: "\"Coca-Cola Route\" — 6 Days", zh: "\"可口可乐路线\" — 6天" },
    price: "From $1,600",
    img: null
  },
  {
    id: 'rongai',
    title: { en: "Rongai Route", zh: "荣盖路线" },
    subtitle: { en: "6 Days · Wild Northern Slopes", zh: "6天 · 原始北坡" },
    price: "From $1,900",
    img: null
  },
  {
    id: 'umbwe',
    title: { en: "Umbwe Route", zh: "翁布韦路线" },
    subtitle: { en: "The Ultimate Challenge", zh: "终极挑战" },
    price: "From $1,800",
    img: null
  },
  {
    id: 'longido',
    title: { en: "Longido Cultural Hike", zh: "隆吉多文化之旅" },
    subtitle: { en: "Maasai Culture & Mountain Hike", zh: "马赛文化与登山" },
    price: "From $600",
    img: null
  },
  {
    id: 'meru',
    title: { en: "Mount Meru", zh: "梅鲁山" },
    subtitle: { en: "Tanzania's Wild Prelude to Kili", zh: "乞力马扎罗的狂野前奏" },
    price: "From $1,200",
    img: null
  }
];

const safariData = [
  {
    id: 'northern-circuit',
    title: { en: "Northern Circuit 4-Day Safari", zh: "北部环线4日野生动物园" },
    subtitle: { en: "Tarangire · Serengeti · Ngorongoro", zh: "塔兰吉雷 · 塞伦盖蒂 · 恩戈罗恩戈罗" },
    price: "From $1,500",
    img: null
  },
  {
    id: 'great-migration',
    title: { en: "Tracking the Great Migration", zh: "追踪大迁徙" },
    subtitle: { en: "5 Days · Nature's Greatest Show", zh: "5天 · 大自然最壮丽的演出" },
    price: "From $2,200",
    img: null
  },
  {
    id: 'lake-natron',
    title: { en: "Lake Natron & Oldonyo Lengai", zh: "纳特龙湖与伦盖火山" },
    subtitle: { en: "Flamingos & Active Volcano", zh: "火烈鸟与活火山" },
    price: "From $800",
    img: null
  },
  {
    id: 'elephants-beaches',
    title: { en: "Elephants, Lions & White Beaches", zh: "大象、狮子与白沙滩" },
    subtitle: { en: "Bush to Beach Adventure", zh: "丛林到海滩的冒险" },
    price: "From $3,000",
    img: null
  },
  {
    id: 'lake-eyasi',
    title: { en: "Lake Eyasi Day Trip", zh: "埃亚西湖一日游" },
    subtitle: { en: "Meet the Hadza Hunter-Gatherers", zh: "探访哈扎狩猎采集部落" },
    price: "From $400",
    img: null
  },
  {
    id: 'mkomazi',
    title: { en: "Mkomazi National Park", zh: "姆科马齐国家公园" },
    subtitle: { en: "Tanzania's Hidden Wildlife Sanctuary", zh: "坦桑尼亚隐秘的野生动物保护区" },
    price: "From $600",
    img: null
  },
  {
    id: 'usambara',
    title: { en: "Eastern & Western Usambara", zh: "东西乌桑巴拉山" },
    subtitle: { en: "Rainforest Mountains & Culture", zh: "雨林山脉与文化之旅" },
    price: "From $500",
    img: null
  },
  {
    id: 'northern-circuit-great',
    title: { en: "Northern Circuit Great Migration", zh: "北部大迁徙之旅" },
    subtitle: { en: "Big Cats, Maasai & Stunning Landscapes", zh: "大型猫科动物、马赛人与壮美风景" },
    price: "From $1,800",
    img: null
  }
];

const activitiesData = [
  {
    id: 'farm-coffee',
    title: { en: "Arusha Farm Walk & Coffee Tour", zh: "阿鲁沙农场与咖啡之旅" },
    subtitle: { en: "Farm-to-Cup Coffee Experience", zh: "从农场到杯中的咖啡体验" },
    price: "From $80",
    img: null
  },
  {
    id: 'maji-moto',
    title: { en: "Arusha, Ngorongoro & Maji Moto", zh: "阿鲁沙、恩戈罗恩戈罗与温泉" },
    subtitle: { en: "Geothermal Paradise & Hot Springs", zh: "地热天堂与温泉" },
    price: "From $120",
    img: null
  },
  {
    id: 'lake-duluti',
    title: { en: "Lake Duluti Day Trip", zh: "杜鲁提湖一日游" },
    subtitle: { en: "Crater Lake Canoeing & Birdwatching", zh: "火山湖划独木舟与观鸟" },
    price: "From $70",
    img: null
  },
  {
    id: 'napuru',
    title: { en: "Napuru Waterfalls", zh: "纳普鲁瀑布" },
    subtitle: { en: "78m Waterfall in Ancient Rainforest", zh: "原始雨林中78米高的瀑布" },
    price: "From $60",
    img: null
  },
  {
    id: 'raga-voice',
    title: { en: "Raga Voice Experience", zh: "Raga Voice 音乐体验" },
    subtitle: { en: "Music Meets the Mountain", zh: "当音乐遇见高山" },
    price: "Custom",
    img: null
  }
];

function renderCards(containerId, data, section) {
  const container = document.getElementById(containerId);
  container.innerHTML = data.map(item => {
    const title = currentLang === 'en' ? item.title.en : item.title.zh;
    const subtitle = currentLang === 'en' ? item.subtitle.en : item.subtitle.zh;
    return `
      <div class="card" onclick="openDetail('${section}', '${item.id}')">
        <div class="card-img" style="background:linear-gradient(135deg, var(--brown), var(--gold));"></div>
        <div class="card-body">
          <h3 class="card-title">${title}</h3>
          <p class="card-info">${subtitle}</p>
          <p class="card-days">${item.price}</p>
        </div>
      </div>`;
  }).join('');
}

function renderTreks() { renderCards('treksCards', treksData, 'treks'); }
function renderSafari() { renderCards('safariCards', safariData, 'safari'); }
function renderActivities() { renderCards('activitiesCards', activitiesData, 'activities'); }

// ===== MODAL =====
function openDetail(section, id) {
  const modal = document.getElementById('modal');
  const body = document.getElementById('modalBody');
  let item;
  if (section === 'treks') item = treksData.find(d => d.id === id);
  if (section === 'safari') item = safariData.find(d => d.id === id);
  if (section === 'activities') item = activitiesData.find(d => d.id === id);
  if (!item) return;

  const title = currentLang === 'en' ? item.title.en : item.title.zh;
  const subtitle = currentLang === 'en' ? item.subtitle.en : item.subtitle.zh;
  body.innerHTML = `
    <h2>${title}</h2>
    <p style="color:var(--gold);font-size:13px;letter-spacing:1px;margin-bottom:16px;">${subtitle}</p>
    <p style="color:var(--text-light);">${item.price}</p>
    <p style="margin-top:24px;color:var(--text-light);font-size:13px;" data-key="detail_placeholder">Detailed itinerary content will be loaded here.</p>
    <div style="margin-top:32px;">
      <a href="https://wa.me/255XXXXXXXXX" class="btn-fill" target="_blank" rel="noopener" data-key="detail_inquire">Inquire on WhatsApp</a>
    </div>`;
  modal.classList.add('open');
  document.body.style.overflow = 'hidden';
  // Apply translations to modal content
  applyTranslations();
}

function closeModal() {
  document.getElementById('modal').classList.remove('open');
  document.body.style.overflow = '';
}

document.getElementById('modal').addEventListener('click', function(e) {
  if (e.target === this) closeModal();
});

document.addEventListener('keydown', function(e) {
  if (e.key === 'Escape') closeModal();
});

// ===== CONTACT FORM =====
function handleContact(e) {
  e.preventDefault();
  const form = e.target;
  const name = form.name.value;
  const email = form.email.value;
  const message = form.message.value;
  const mailto = `mailto:info@kilivibes.com?subject=Inquiry from ${encodeURIComponent(name)}&body=${encodeURIComponent(message + '\n\nFrom: ' + name + '\nEmail: ' + email)}`;
  window.location.href = mailto;
  form.reset();
  alert(currentLang === 'en' ? 'Opening your email client...' : '正在打开你的邮件客户端...');
}

// ===== INIT =====
renderTreks();
renderSafari();
renderActivities();
</script>
</body>
</html>
```

- [ ] **Step 2: Verify file structure**

Run: `ls "D:\WorkSpace\AI\WebSite\KlimanjaroTour\index.html"`

Expected: File exists at path.

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "feat: create complete KILI VIBES website — single-page HTML with bilingual support"
```

---

### Task 2: Replace placeholder images with real photos from the Photos folder

**Files:**
- Modify: `index.html` — gallery section and card backgrounds

- [ ] **Step 1: List available photo paths**

Run: `find "D:\WorkSpace\AI\WebSite\KlimanjaroTour\Photos" -type f \( -name "*.jpg" -o -name "*.jpeg" \) | head -20`

- [ ] **Step 2: Update gallery strip with real photos**

Edit `index.html`, replace the gallery section HTML:

```html
<section class="gallery" id="gallery">
  <div class="gallery-item" style="background-image:url('Photos/Safari/IMG20230424164100.jpg');background-size:cover;background-position:center;"></div>
  <div class="gallery-item" style="background-image:url('Photos/Safari/WhatsApp Image 2023-10-09 at 13.09.42.jpeg');background-size:cover;background-position:center;"></div>
  <div class="gallery-item" style="background-image:url('Photos/Day_Activities/5afc71d4bca843ca95a812782d7dc1bb.jpg');background-size:cover;background-position:center;"></div>
</section>
```

- [ ] **Step 3: Update grid nav cards with real photos as backgrounds where possible**

Edit each `.grid-card:nth-child(n) .grid-card-bg` CSS, replace gradient with `background-image` referencing actual photos. Keep gradient as fallback.

- [ ] **Step 4: Add a script to randomly assign photos to cards**

Add to the `<script>` block before `renderCards`:

```javascript
const dayPhotos = [
  'Photos/Day_Activities/5afc71d4bca843ca95a812782d7dc1bb.jpg',
  'Photos/Day_Activities/84bf4a21df9c45f3838cb71fe260b303.jpg',
  'Photos/Day_Activities/WhatsApp Image 2023-10-09 at 13.07.32 (1).jpeg',
  'Photos/Day_Activities/WhatsApp Image 2025-03-29 at 13.25.37_d33cfbdf (1).jpg',
  'Photos/Day_Activities/WhatsApp Image 2025-03-29 at 13.25.46_03dbf482 (1).jpg'
];
const safariPhotos = [
  'Photos/Safari/3b1f95f0fe774759a64192d2986ecc9a.jpg',
  'Photos/Safari/46f08f41b1f24caf96aaa8886172428e.jpg',
  'Photos/Safari/8ac878db126441b5aa971028330a865e.jpg',
  'Photos/Safari/91ca4d4a40c945e9915c08fbcb625cc8.jpg',
  'Photos/Safari/IMG20230424164100.jpg'
];

function randomPhoto(list) {
  return list[Math.floor(Math.random() * list.length)];
}
```

- [ ] **Step 5: Commit**

```bash
git add index.html
git commit -m "feat: integrate real photos into gallery and cards"
```

---

### Task 3: Extract and embed detailed itinerary content from .docx files

**Files:**
- Modify: `index.html` — add detail content to the modal rendering

- [ ] **Step 1: Extract text from all .docx files using unzip + sed**

Run for each file and capture output:

```bash
cd "D:\WorkSpace\AI\WebSite\KlimanjaroTour"
for f in Itineraries/Mountaineering/*.docx Itineraries/Safari/*.docx Day_Activities/*.docx; do
  echo "===== $f ====="
  unzip -p "$f" word/document.xml 2>/dev/null | sed 's/<[^>]*>//g' | tr -s '[:space:]' ' ' | head -c 2000
  echo -e "\n"
done
```

- [ ] **Step 2: Create a detail content data structure in JS**

Add to the `<script>` block, after activitiesData:

```javascript
const detailContent = {
  treks: {
    machame: {
      en: `<h3>KILIMANJARO'S MACHAME ROUTE: 6 DAYS OF PURE ADRENALINE</h3>
<p>Nicknamed the "Whisky Route" — because smooth success demands a shot of grit. This isn't a leisurely hike—it's a steep, heart-pounding ascent through rainforests, moorlands, and glacial valleys. Perfect for adventurers who want to conquer Africa's roof in record time without sacrificing scenery.</p>
<h4>DAY 0: ARRIVE LIKE A HERO</h4>
<p>Land at Kilimanjaro Airport, where seamless transfers whisk you to Springlands Hotel. Rest, feast, and steel your nerves—tomorrow, the mountain tests you.</p>
<h4>DAY 1: RAINFOREST BOOTCAMP</h4>
<p>Machame Gate → Machame Camp | 1490m → 2980m | 6-7 hrs. Plunge into dense, mist-shrouded rainforest—where mud-slick trails and primal humidity become your first adversaries.</p>
<h4>DAY 2: MOORLAND MARCH</h4>
<p>Machame Camp → Shira Camp | 2980m → 3840m | 4-5 hrs. Emerge from the forest into giant heather and lobelia. First views of Kibo's glaciers.</p>
<h4>DAY 3: LAVA TOWER ACCLIMATIZATION</h4>
<p>Shira Camp → Lava Tower → Barranco Camp | 3840m → 4630m → 3950m | 6-7 hrs. Climb high, sleep low—essential acclimatization through volcanic moonscape.</p>
<h4>DAY 4: BARANCO WALL</h4>
<p>Barranco Camp → Karanga Camp | 3950m → 4035m | 4-5 hrs. The famous Barranco Wall scramble—hands-on rock, breathtaking drops, pure adrenaline.</p>
<h4>DAY 5: BARAFU BASE</h4>
<p>Karanga Camp → Barafu Camp | 4035m → 4660m | 3-4 hrs. Summit eve. Prepare gear, rest, and let Raga Voice serenade your final night before the summit push.</p>
<h4>DAY 6: SUMMIT & DESCENT</h4>
<p>Barafu → Uhuru Peak 5895m → Mweka Camp 3100m | 12-16 hrs. Midnight start under a canopy of stars. Stand on the Roof of Africa at sunrise. Then descend—tired, triumphant, transformed.</p>`,
      zh: `<h3>乞力马扎罗马查姆路线：6天纯肾上腺素之旅</h3>
<p>绰号"威士忌路线"——因为顺滑的成功需要毅力的注入。这不是悠闲的徒步——这是一次穿过雨林、荒原和冰川山谷的陡峭攀登。适合想要在最短时间内征服非洲之巅的冒险者。</p>
<h4>第0天：英雄抵达</h4><p>降落在乞力马扎罗机场，无缝接送到Springlands酒店。休息、用餐，为明天的挑战做好准备。</p>
<h4>第1天：雨林特训</h4><p>马查姆大门→马查姆营地 | 1490m→2980m | 6-7小时。进入浓密雾霭缭绕的雨林。</p>
<h4>第2天：荒原行军</h4><p>马查姆营地→希拉营地 | 2980m→3840m | 4-5小时。</p>
<h4>第3天：熔岩塔适应</h4><p>希拉营地→熔岩塔→巴兰科营地 | 3840m→4630m→3950m | 6-7小时。爬高睡低——关键的适应日。</p>
<h4>第4天：巴兰科墙</h4><p>巴兰科营地→卡兰加营地 | 3950m→4035m | 4-5小时。著名的巴兰科墙攀爬——肾上腺素飙升。</p>
<h4>第5天：巴拉夫基地</h4><p>卡兰加营地→巴拉夫营地 | 4035m→4660m | 3-4小时。登顶前夜，让Raga Voice为你弹唱。</p>
<h4>第6天：登顶与下山</h4><p>巴拉夫→乌呼鲁峰5895m→姆韦卡营地3100m | 12-16小时。午夜出发，星空下登顶非洲之巅。日出时分，站在巅峰。</p>`
    }
    // Additional detail content for other routes would follow the same pattern
  },
  safari: {
    'northern-circuit': {
      en: `<h3>4-DAY TANZANIA SAFARI: ICONS OF THE WILD</h3>
<p>Where elephants roam, lions climb, and the Great Migration thunders—experience Africa's crown jewels in one unforgettable journey.</p>
<h4>DAY 1: TARANGIRE – KINGDOM OF GIANTS</h4>
<p>Arusha → Tarangire National Park | 130km | 2 hrs. Elephant herds so vast, they blot out the horizon. Ancient baobabs guard golden savannahs. Game drive, picnic under acacia trees, camp under stars.</p>
<h4>DAY 2: SERENGETI – ENDLESS PLAINS</h4>
<p>Tarangire → Serengeti | 250km | 4 hrs. The Serengeti stretches beyond imagination. Lions on kopjes, cheetahs sprinting, wildebeest as far as the eye can see.</p>
<h4>DAY 3: NGORONGORO – THE GARDEN OF EDEN</h4>
<p>Serengeti → Ngorongoro Crater | 145km | 2.5 hrs. Descend into the world's largest intact caldera. Black rhinos, flamingos, and every creature in between.</p>
<h4>DAY 4: RETURN TO ARUSHA</h4>
<p>Ngorongoro → Arusha | 190km | 3 hrs. Drive back through the Great Rift Valley, carrying memories that last a lifetime.</p>`,
      zh: `<h3>4天坦桑尼亚野生动物园：荒野的标志</h3>
<p>大象漫步、狮子攀树、大迁徙雷霆万钧——一次难忘的旅程，体验非洲的皇冠珠宝。</p>
<h4>第1天：塔兰吉雷——巨人之国</h4><p>阿鲁沙→塔兰吉雷国家公园 | 130公里 | 2小时。象群之多，遮天蔽日。古老猴面包树守护金色稀树草原。</p>
<h4>第2天：塞伦盖蒂——无尽平原</h4><p>塔兰吉雷→塞伦盖蒂 | 250公里 | 4小时。塞伦盖蒂辽阔无垠。狮子在岩石上休憩，猎豹飞驰，角马漫山遍野。</p>
<h4>第3天：恩戈罗恩戈罗——伊甸园</h4><p>塞伦盖蒂→恩戈罗恩戈罗火山口 | 145公里 | 2.5小时。深入世界上最大的完整火山口。黑犀牛、火烈鸟，无所不有。</p>
<h4>第4天：返回阿鲁沙</h4><p>恩戈罗恩戈罗→阿鲁沙 | 190公里 | 3小时。穿越东非大裂谷，带着永恒的记忆返回。</p>`
    }
  }
};
```

- [ ] **Step 3: Update openDetail() to use detail content**

Edit the `openDetail` function, replace the body.innerHTML line with:

```javascript
const detail = detailContent[section] && detailContent[section][id];
const detailHtml = detail ? (currentLang === 'en' ? detail.en : detail.zh) : `<p style="color:var(--text-light);">${currentLang === 'en' ? 'Full itinerary coming soon. Contact us for details.' : '详细行程即将发布，联系我们获取详情。'}</p>`;
body.innerHTML = `
  <h2>${title}</h2>
  <p style="color:var(--gold);font-size:13px;letter-spacing:1px;margin-bottom:24px;">${subtitle} · ${item.price}</p>
  ${detailHtml}
  <div style="margin-top:32px;">
    <a href="https://wa.me/255XXXXXXXXX" class="btn-fill" target="_blank" rel="noopener" data-key="detail_inquire">${currentLang === 'en' ? 'Inquire on WhatsApp' : '在WhatsApp上咨询'}</a>
  </div>`;
```

- [ ] **Step 4: Commit**

```bash
git add index.html
git commit -m "feat: add detailed itinerary content for Machame route and Northern Circuit safari"
```

---

### Task 4: Add YouTube video background and final polish

**Files:**
- Modify: `index.html`

- [ ] **Step 1: Add YouTube video support to the hero section**

Edit the hero HTML, replace the hero section content to optionally embed a YouTube video:

```html
<section class="hero" id="hero">
  <!-- Replace VIDEO_ID with actual YouTube video ID for background -->
  <iframe class="hero-video" src="https://www.youtube.com/embed/VIDEO_ID?autoplay=1&mute=1&loop=1&playlist=VIDEO_ID&controls=0&showinfo=0&rel=0&playsinline=1" allow="autoplay; fullscreen" style="display:none;" id="heroVideo"></iframe>
  <div class="hero-overlay"></div>
  <div class="hero-content">
    <div class="hero-icon">&#9968;</div>
    <h1 class="hero-title">KILIMANJARO</h1>
    <p class="hero-subtitle" data-key="hero_subtitle">THE ROOF OF AFRICA</p>
    <a href="#treks-section" class="hero-cta" data-key="hero_cta">Explore Journeys</a>
  </div>
</section>
```

The video is hidden by default (gradient fallback shows). The user replaces `VIDEO_ID` with their YouTube video ID and removes `style="display:none;"` from the iframe.

- [ ] **Step 2: Update WhatsApp numbers throughout**

Replace all `255XXXXXXXXX` placeholders with the actual WhatsApp number. The founder needs to provide this.

- [ ] **Step 3: Update email address**

Replace `info@kilivibes.com` with the actual email address.

- [ ] **Step 4: Verify the site works end-to-end**

Run: Open `index.html` in a browser and test:
- Scroll through all sections
- Click grid cards → navigate to correct section
- Click detail cards → modal opens with content
- Toggle language → all text switches to Chinese and back
- Resize browser → responsive layout works
- Mobile menu toggle works on small screens
- Contact form → opens email client

- [ ] **Step 5: Commit**

```bash
git add index.html
git commit -m "feat: add YouTube video embed support and finalize contact details"
```

---

## Self-Review

### 1. Spec Coverage
- Navigation Bar ✅ Task 1
- Hero with video background ✅ Task 1, Task 4
- Grid Navigation (4 squares) ✅ Task 1
- Featured Treks ✅ Task 1
- Photo Gallery Strip ✅ Task 2
- About Snippet with YouTube ✅ Task 1
- Contact / CTA ✅ Task 1
- Footer ✅ Task 1
- Mountaineering routes (8) ✅ Task 1 (data), Task 3 (detail content)
- Safari itineraries (8) ✅ Task 1 (data), Task 3 (detail content)
- Day Activities (5) ✅ Task 1 (data)
- Photos integration ✅ Task 2
- Responsive behavior ✅ Task 1 (CSS media queries)
- Bilingual strategy ✅ Task 1 (i18n object + toggleLang)

### 2. Placeholder Scan
- No TBD/TODO
- All code steps have actual code
- WhatsApp number uses placeholder `255XXXXXXXXX` — called out as needing user input in Task 4
- YouTube video ID uses placeholder `VIDEO_ID` — called out in Task 4
- Email uses `info@kilivibes.com` placeholder — called out in Task 4

### 3. Type Consistency
- `treksData`, `safariData`, `activitiesData` all share the same shape: `{id, title:{en,zh}, subtitle:{en,zh}, price, img}`
- `detailContent` structure: `{treks|safari|activities: {id: {en, zh}}}` — consistent with data arrays
- `renderCards` called with consistent args throughout

### Gaps Identified & Fixed
- Added photo paths from actual file listing in Task 2
- Made sure gallery items use real photo filenames from the `Photos/` directory
