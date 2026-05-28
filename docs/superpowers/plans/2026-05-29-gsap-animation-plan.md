# GSAP Animation Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** 为 KILI VIBES 网站 `index.html` 添加全页面 GSAP 滚动驱动动效

**Architecture:** 内联 GSAP core + ScrollTrigger（~115KB）到 HTML，在 `</style>` 之后插入库文件，在现有 `</script>` 之前插入自定义动画代码。所有动画通过 ScrollTrigger + Timeline 驱动，GPU 加速属性（transform/opacity）。

**Tech Stack:** GSAP 3.x core + ScrollTrigger plugin，纯 JS，零构建工具

---

### Task 1: 内联 GSAP core + ScrollTrigger

**Files:**
- Modify: `index.html`（`</style>` 之后插入）

- [ ] **Step 1: 确认文件行号**

确认 `</style>` 在第 257 行，`</head>` 在第 258 行。

- [ ] **Step 2: 在 `</style>` 之后插入 GSAP core 和 ScrollTrigger**

在 `</style>` 和 `</head>` 之间插入两个 `<script>` 块。第一个包含 GSAP core（73KB），第二个包含 ScrollTrigger（44KB）。

从 `$env:TEMP\gsap-core.js` 和 `$env:TEMP\gsap-scrolltrigger.js` 读取已下载的库文件内容。

具体操作：
```powershell
# 读取已下载的库文件
$gsapCore = Get-Content "$env:TEMP\gsap-core.js" -Raw
$gsapSt = Get-Content "$env:TEMP\gsap-scrolltrigger.js" -Raw

# 在 </style> 后插入
$html = Get-Content index.html -Raw
$insert = "`n<script>`n" + $gsapCore + "`n</script>`n<script>`n" + $gsapSt + "`n</script>"
$html = $html -replace '(?<=</style>)', $insert
Set-Content index.html -Value $html -Encoding UTF8
```

注意：在 bash 工具中使用 PowerShell 语法执行时需调整。实际上直接使用 Edit 工具在 `</style>` 行后插入内容。

- [ ] **Step 3: 验证插入成功**

检查 GSAP 对象可在浏览器控制台访问。用浏览器打开 `index.html`，F12 控制台输入 `gsap` 应返回对象而非 undefined。

- [ ] **Step 4: 提交**

```bash
git add index.html
git commit -m "@ feat: inline GSAP core + ScrollTrigger (~115KB)"
```

---

### Task 2: Hero 首屏加载动画 Timeline

**Files:**
- Modify: `index.html`（在现有 `<script>` 末尾的 `</script>` 之前添加）

- [ ] **Step 1: 添加 Hero 动画代码**

在现有 `<script>` 块的 `</script>` 之前（约第 900+ 行），添加以下代码：

```javascript
// ===== GSAP ANIMATIONS =====
document.addEventListener('DOMContentLoaded', () => {
  // Register ScrollTrigger
  gsap.registerPlugin(ScrollTrigger);

  // --- Hero Load Animation ---
  const heroTl = gsap.timeline({ defaults: { ease: 'power2.out' } });

  // 背景视频 Ken Burns
  heroTl.fromTo('.hero-video', { scale: 1 }, { scale: 1.05, duration: 4, ease: 'none' }, 0);

  // 遮罩渐亮
  heroTl.fromTo('.hero-overlay', { opacity: 0.6 }, { opacity: 0.4, duration: 1.2 }, 0);

  // 图标落下
  heroTl.from('.hero-icon', { y: -30, opacity: 0, duration: 0.6, ease: 'back.out(1.7)' }, 0.2);

  // 标题滑入
  heroTl.from('.hero-title', { y: 40, opacity: 0, duration: 0.8, ease: 'power3.out' }, 0.3);

  // 副标题淡入
  heroTl.from('.hero-subtitle', { opacity: 0, duration: 0.6 }, 0.45);

  // CTA 弹性放大
  heroTl.from('.hero-cta', { scale: 0.8, opacity: 0, duration: 0.5, ease: 'back.out(1.7)' }, 0.6);
});
```

- [ ] **Step 2: 浏览器验证**

打开 `index.html`，刷新页面。确认：
- 视频缓慢放大
- 遮罩变亮
- 图标从上方落下
- 标题滑入
- CTA 弹性弹出
- 整个动画流畅无卡顿

- [ ] **Step 3: 提交**

```bash
git add index.html
git commit -m "@ feat: add GSAP hero timeline animation"
```

---

### Task 3: 导航栏 GSAP 增强 + 移动端菜单

**Files:**
- Modify: `index.html`（DOMContentLoaded 回调内追加）

- [ ] **Step 1: 添加导航栏滚动动画**

在 `DOMContentLoaded` 回调的 Hero 动画代码之后追加：

```javascript
  // --- Nav Bar Scroll ---
  const navTl = gsap.to('.nav', {
    backgroundColor: 'rgba(255,255,255,0.95)',
    boxShadow: '0 1px 0 rgba(240,235,224,1)',
    duration: 0.3,
    paused: true
  });

  // 使用 ScrollTrigger 替代原来的 scroll 事件
  ScrollTrigger.create({
    start: 'top+=80',
    end: 'bottom top',
    onEnter: () => navTl.play(),
    onLeaveBack: () => navTl.reverse()
  });

  // 移除旧 scroll 事件（避免冲突）
  // 原 window.addEventListener('scroll', ...) 仍保留但用 GSAP 接管视觉变化
  // 保留 .scrolled class 的 CSS 规则作为兜底
```

注意：需要保留现有 CSS `.nav.scrolled` 的样式作为 GSAP 动画的 fallback。GSAP 的 `backgroundColor` 动画需要移除现有 CSS transition 中关于 nav 背景的部分，或者 GSAP 直接覆盖。

简化方案：保留现有 scroll 事件的 `classList.toggle('scrolled')` 逻辑，GSAP 仅处理移动端菜单动画。

实际实现方案：
```javascript
  // --- Mobile Menu Animation ---
  const menuBtn = document.getElementById('menuBtn');
  const navLinks = document.getElementById('navLinks');
  let menuOpen = false;
  let menuTween = null;

  // 覆盖 toggleMenu 函数
  window.toggleMenu = function() {
    menuOpen = !menuOpen;
    if (menuTween) menuTween.kill();
    if (menuOpen) {
      navLinks.style.display = 'flex';
      navLinks.style.flexDirection = 'column';
      navLinks.style.position = 'absolute';
      navLinks.style.top = 'var(--nav-h)';
      navLinks.style.left = '0';
      navLinks.style.right = '0';
      navLinks.style.background = 'var(--white)';
      navLinks.style.padding = '24px 32px';
      navLinks.style.gap = '16px';
      navLinks.style.boxShadow = '0 4px 12px rgba(0,0,0,0.1)';
      menuTween = gsap.from(navLinks, { y: -20, opacity: 0, duration: 0.3, ease: 'power2.out' });
    } else {
      menuTween = gsap.to(navLinks, { y: -20, opacity: 0, duration: 0.2, ease: 'power2.in', onComplete: () => {
        navLinks.style.display = 'none';
        navLinks.style.opacity = '';
        navLinks.style.transform = '';
      }});
    }
  };
```

- [ ] **Step 2: 浏览器验证**

在移动端视口（<768px）测试菜单按钮：
- 点击汉堡按钮 → 菜单从上滑入
- 再次点击 → 菜单向上滑出
- 点击菜单内链接 → 菜单关闭并导航到对应区域

- [ ] **Step 3: 提交**

```bash
git add index.html
git commit -m "@ feat: GSAP mobile menu slide animation"
```

---

### Task 4: 四宫格导航 ScrollTrigger 入场

**Files:**
- Modify: `index.html`（DOMContentLoaded 回调内追加）

- [ ] **Step 1: 添加四宫格入场动画**

在 `DOMContentLoaded` 回调中追加：

```javascript
  // --- Grid Nav Cards ---
  gsap.from('.grid-card', {
    scrollTrigger: {
      trigger: '.grid-nav',
      start: 'top 85%',
      toggleActions: 'play none none none'
    },
    y: 40,
    opacity: 0,
    duration: 0.5,
    stagger: 0.12,
    ease: 'back.out(1.4)'
  });
```

- [ ] **Step 2: 浏览器验证**

滚动到四宫格区域：
- 4 张卡片从底部逐个弹入
- refresh 后再次滚动仍触发（scrollTrigger 默认 once: false，需要设置 `once: true` 避免重复触发）

修正：添加 `once: true` 避免用户来回滚动时重复播放：

```javascript
  gsap.from('.grid-card', {
    scrollTrigger: {
      trigger: '.grid-nav',
      start: 'top 85%',
      once: true
    },
    y: 40,
    opacity: 0,
    duration: 0.5,
    stagger: 0.12,
    ease: 'back.out(1.4)'
  });
```

- [ ] **Step 3: 提交**

---

### Task 5: 各区标题入场动画

**Files:**
- Modify: `index.html`（DOMContentLoaded 回调内追加）

- [ ] **Step 1: 添加标题动画**

```javascript
  // --- Section Labels & Titles ---
  document.querySelectorAll('.section-label').forEach(el => {
    gsap.from(el, {
      scrollTrigger: { trigger: el, start: 'top 90%', once: true },
      x: -20,
      opacity: 0,
      duration: 0.5,
      ease: 'power2.out'
    });
  });

  document.querySelectorAll('.section-title').forEach(el => {
    gsap.from(el, {
      scrollTrigger: { trigger: el, start: 'top 90%', once: true },
      y: 20,
      opacity: 0,
      duration: 0.5,
      ease: 'power2.out'
    });
  });
```

- [ ] **Step 2: 浏览器验证**

滚动到各区域，确认 label 从左滑入、title 从下浮入。

- [ ] **Step 3: 提交**

---

### Task 6: 卡片区 stagger 入场 + Hover 增强

**Files:**
- Modify: `index.html`（DOMContentLoaded 回调内追加）

- [ ] **Step 1: 添加卡片入场动画**

```javascript
  // --- Card Stagger Reveal ---
  ['#treksCards', '#safariCards', '#activitiesCards'].forEach(sel => {
    const container = document.querySelector(sel);
    if (!container) return;
    gsap.from(container.querySelectorAll('.card'), {
      scrollTrigger: {
        trigger: container,
        start: 'top 85%',
        once: true
      },
      y: 30,
      opacity: 0,
      duration: 0.5,
      stagger: 0.1,
      ease: 'power3.out'
    });
  });
```

- [ ] **Step 2: 添加桌面端 hover 微动**

```javascript
  // --- Card Hover Tilt (desktop only) ---
  if (window.matchMedia('(hover: hover)').matches) {
    document.querySelectorAll('.card').forEach(card => {
      const qX = gsap.quickTo(card, 'x', { duration: 0.3, ease: 'power2.out' });
      const qY = gsap.quickTo(card, 'y', { duration: 0.3, ease: 'power2.out' });

      card.addEventListener('mousemove', e => {
        const rect = card.getBoundingClientRect();
        const x = (e.clientX - rect.left - rect.width / 2) / rect.width * 10;
        const y = (e.clientY - rect.top - rect.height / 2) / rect.height * 10;
        qX(x);
        qY(y);
      });

      card.addEventListener('mouseleave', () => {
        qX(0);
        qY(0);
      });
    });
  }
```

注意：`quickTo` 需要去掉现有的 CSS hover `transform: translateY(-4px)` 因为 GSAP 的 transform 会覆盖它。修改 `.card:hover` 规则，移除 `transform` 部分，保留 `box-shadow` 过渡：

```css
.card:hover { box-shadow: 0 8px 24px rgba(0,0,0,0.1); }
```

- [ ] **Step 3: 浏览器验证**

滚动到卡片区域 → 卡片逐个滑入。鼠标悬停卡片 → 跟随鼠标微移。移动端检查 hover 效果被禁用。

- [ ] **Step 4: 提交**

---

### Task 7: 画廊条视差 + 入场

**Files:**
- Modify: `index.html`（DOMContentLoaded 回调内追加）

- [ ] **Step 1: 添加画廊入场动画**

```javascript
  // --- Gallery ---
  gsap.from('.gallery-item', {
    scrollTrigger: {
      trigger: '.gallery',
      start: 'top 85%',
      once: true
    },
    scale: 0.9,
    opacity: 0,
    duration: 0.6,
    stagger: 0.15,
    ease: 'power2.out'
  });

  // Gallery parallax
  gsap.to('.gallery-item', {
    scrollTrigger: {
      trigger: '.gallery',
      start: 'top bottom',
      end: 'bottom top',
      scrub: 1
    },
    y: -40,
    ease: 'none'
  });
```

- [ ] **Step 2: 浏览器验证**

滚动到画廊 → 3 张图错落放大进入。继续滚动 → 图片有轻微向上视差偏移。

- [ ] **Step 3: 提交**

---

### Task 8: About 区动画

**Files:**
- Modify: `index.html`（DOMContentLoaded 回调内追加）

- [ ] **Step 1: 添加 About 区动画**

```javascript
  // --- About Section ---
  gsap.from('.about-quote', {
    scrollTrigger: {
      trigger: '.about',
      start: 'top 85%',
      once: true
    },
    clipPath: 'inset(0 100% 0 0)',
    duration: 1.2,
    ease: 'power3.inOut'
  });

  gsap.from('.about-name, .about-role', {
    scrollTrigger: {
      trigger: '.about-name',
      start: 'top 90%',
      once: true
    },
    y: 20,
    opacity: 0,
    duration: 0.5,
    stagger: 0.1,
    ease: 'power2.out'
  });

  gsap.from('.video-wrap', {
    scrollTrigger: {
      trigger: '.video-wrap',
      start: 'top 88%',
      once: true
    },
    scale: 0.85,
    opacity: 0,
    duration: 0.7,
    ease: 'power3.out'
  });
```

- [ ] **Step 2: 浏览器验证**

滚动到 About 区 → 引用文字从左到右揭示 → 名字和角色浮现 → 视频容器放大进入。

- [ ] **Step 3: 提交**

---

### Task 9: CTA / Contact 区动画

**Files:**
- Modify: `index.html`（DOMContentLoaded 回调内追加）

- [ ] **Step 1: 添加 CTA 区动画**

```javascript
  // --- CTA Buttons ---
  gsap.from('.cta-buttons .btn-outline, .cta-buttons .btn-fill', {
    scrollTrigger: {
      trigger: '.cta-buttons',
      start: 'top 88%',
      once: true
    },
    scale: 0,
    opacity: 0,
    duration: 0.5,
    stagger: 0.15,
    ease: 'back.out(1.7)'
  });

  // --- Contact Form Inputs ---
  gsap.from('.contact-form input, .contact-form textarea, .contact-form button', {
    scrollTrigger: {
      trigger: '.contact-form',
      start: 'top 88%',
      once: true
    },
    x: -20,
    opacity: 0,
    duration: 0.4,
    stagger: 0.08,
    ease: 'power2.out'
  });
```

- [ ] **Step 2: 浏览器验证**

滚动到 Contact → 按钮弹跳出现 → 表单输入框从左逐个滑入。

- [ ] **Step 3: 提交**

---

### Task 10: Footer 淡入 + 模态框动画

**Files:**
- Modify: `index.html`（DOMContentLoaded 回调内追加 + 替换现有 openDetail/closeModal 函数）

- [ ] **Step 1: 添加 Footer 淡入**

```javascript
  // --- Footer ---
  gsap.from('.footer', {
    scrollTrigger: {
      trigger: '.footer',
      start: 'top 95%',
      once: true
    },
    opacity: 0,
    duration: 0.6,
    ease: 'power2.out'
  });
```

- [ ] **Step 2: 替换模态框函数**

替换现有的 `openDetail` 和 `closeModal` 函数，加入 GSAP 过渡。

在 `openDetail` 函数中，在设置 body.innerHTML 和 `modal.classList.add('open')` 之后添加：

```javascript
  // GSAP modal open animation (in openDetail function, after modal.classList.add('open'))
  gsap.fromTo('.modal-content', 
    { scale: 0.9, opacity: 0 },
    { scale: 1, opacity: 1, duration: 0.35, ease: 'back.out(1.4)' }
  );
  gsap.fromTo('.modal', 
    { opacity: 0 },
    { opacity: 1, duration: 0.25, ease: 'power2.out' },
    0
  );
```

替换 `closeModal` 函数：

```javascript
function closeModal() {
  gsap.to('.modal-content', {
    scale: 0.9, opacity: 0, duration: 0.2, ease: 'power2.in',
    onComplete: () => {
      document.getElementById('modal').classList.remove('open');
      document.body.style.overflow = '';
      gsap.set('.modal-content', { clearProps: 'all' });
      gsap.set('.modal', { clearProps: 'all' });
    }
  });
}
```

- [ ] **Step 3: 浏览器验证**

点击任一活动卡片 → 模态框从中心弹性放大弹出 → 点击关闭 → 缩小淡出。按下 ESC → 同样关闭动画。

- [ ] **Step 4: 提交**

```bash
git add index.html
git commit -m "@ feat: add GSAP footer, modal, CTA and about animations"
```

---

### Task 11: 全局收尾 — 代码整理与最终验证

**Files:**
- Modify: `index.html`

- [ ] **Step 1: 代码整理**

确保所有动画代码都在 `DOMContentLoaded` 回调内，`ScrollTrigger` 注册在最顶部。最终完整结构：

```html
<!-- GSAP 库 -->
<script>/* gsap.min.js content */</script>
<script>/* ScrollTrigger.min.js content */</script>

<!-- 现有 <script> 块 -->
<script>
// ... 现有 i18n / toggleLang / applyTranslations / renderCards 等 ...

// ===== GSAP ANIMATIONS =====
document.addEventListener('DOMContentLoaded', () => {
  gsap.registerPlugin(ScrollTrigger);

  // 1. Hero Timeline
  // 2. Mobile Menu
  // 3. Grid Nav
  // 4. Section Labels & Titles
  // 5. Card Stagger + Hover
  // 6. Gallery
  // 7. About
  // 8. CTA / Contact
  // 9. Footer
});
</script>
```

- [ ] **Step 2: 全功能验证清单**

浏览器打开 `index.html`，逐项确认：

| 检查项 | 预期 |
|--------|------|
| GSAP 加载 | 控制台 `gsap` 返回对象 |
| Hero 动画 | 页面加载后图标/标题/CTA 自动播放 |
| 导航栏滚动 | 滚动后背景变白，logo 颜色切换 |
| 移动端菜单 | 汉堡菜单 GSAP 滑入滑出 |
| 四宫格 | 滚动到区域时卡片逐个弹入 |
| 标题 | 各区 label/title 滚动入场 |
| 卡片区 | 卡片 stagger 滑入，hover 微移 |
| 画廊 | 错落放大 + 视差 |
| About | 引用文字揭示 + 视频放大 |
| CTA | 按钮弹跳，表单滑入 |
| Footer | 淡入 |
| 模态框 | 弹性打开 / 缩小关闭 |
| 移动端 | hover 效果不触发，滚动动画正常 |
| 语言切换 | 切换语言后动画仍正常 |

- [ ] **Step 3: 修复发现的问题**

如有任何动画不生效或冲突，排查并修复。

- [ ] **Step 4: 最终提交**

```bash
git add index.html
git commit -m "@ feat: complete GSAP page animations — all sections animated"
```

---

### Task 12: 推送部署

**Files:**
- None (push only)

- [ ] **Step 1: 推送到 origin**

```bash
git push
```

- [ ] **Step 2: 线上验证**

等待 1-2 分钟，访问 `https://www.kilivibesadventures.com`，逐项检查所有动画。
