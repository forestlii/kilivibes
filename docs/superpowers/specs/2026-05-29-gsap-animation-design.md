# KILI VIBES ADVENTURES — GSAP Animation Design Spec

## Overview

为 `index.html` 添加 GSAP（GreenSock Animation Platform）全页面动效。内联 GSAP core + ScrollTrigger，零外部依赖。风格：大气震撼型，滚动叙事驱动。

## Technical Approach

- **加载方式：** GSAP core + ScrollTrigger 压缩后内联到 `<script>` 块，约 110KB
- **插入位置：** `</style>` 之后内联 GSAP 库文件；自定义动画代码放在现有 `<script>` 块末尾
- **初始化时机：** `DOMContentLoaded` 后注册 ScrollTrigger，避免元素未渲染
- **移动端：** 与桌面端统一体验，不做区分

## Animation Spec by Section

### 1. Hero（首屏加载动画）

页面打开后自动播放，不依赖滚动：

| 元素 | 动画 | 时长 | 缓动 |
|------|------|------|------|
| 背景视频 | scale 1→1.05 Ken Burns | 4s | `none` (linear) |
| 暗色遮罩 | opacity 0.6→0.4 渐亮 | 1.2s | `power2.out` |
| 山形图标 `.hero-icon` | y: -30→0 + fade in | 0.6s | `back.out(1.7)` |
| 标题 `.hero-title` | y: 40→0 + fade in | 0.8s | `power3.out` |
| 副标题 `.hero-subtitle` | fade in, delay 0.15s | 0.6s | `power2.out` |
| CTA `.hero-cta` | scale 0.8→1, delay 0.3s | 0.5s | `back.out(1.7)` |

全部串联为一个 GSAP Timeline，总时长约 1.5s。

### 2. 导航栏

- 滚动超过 80px：背景透明度 0→0.95（GSAP `to()` 驱动，替代现有 CSS transition）
- Logo 和链接颜色同步过渡
- 移动端菜单：GSAP 从上方滑入 + 淡出替代 `display` 切换

### 3. 四宫格导航

进入视口时触发：
- 4 张卡片从底部 40px 逐个滑入，stagger 0.12s，`back.out(1.4)` 缓出

### 4. 各区标题（treks/safari/activities/about/contact）

- `.section-label`：从左侧 20px 淡入，`power2.out`
- `.section-title`：从下方 20px 淡入，delay 0.1s，`power2.out`

### 5. 卡片区（登山/游猎/一日游）

- 卡片 stagger 入场：从下方 30px 滑入，间隔 0.1s，`power3.out`
- Hover 增强（桌面端）：`quickTo()` 实现卡片跟随鼠标微移（x/y 各 ±5px）
- 移动端保持现有 tap 效果

### 6. 画廊条

- 3 张图进入视口时错落放大（scale 0.9→1），stagger 0.15s
- 背景层轻微视差：图片 `background-position-y` 随滚动偏移

### 7. About 区

- 引言文字：clip-path 从左到右揭示动画（`clip-path: inset(0 100% 0 0 → 0 0 0 0)`）
- YouTube 视频容器：scale 0.85→1 + 淡入

### 8. CTA / Contact

- 按钮群：stagger 弹跳（scale 0→1.05→1），间隔 0.15s，`back.out(1.7)`
- 表单输入框：从左滑入，stagger 0.08s，`power2.out`

### 9. Footer

- 简单淡入（opacity 0→1）

### 10. 模态框

- 打开：`scale(0.9→1)` + `opacity(0→1)`，`back.out(1.4)`，0.35s
- 背景遮罩同步淡入
- 关闭：反向动画 0.2s，然后 `display: none`

## Performance

- GSAP 使用 GPU 加速属性（transform + opacity），避免 layout thrashing
- ScrollTrigger 使用 `scrub` 不参与帧循环，仅在滚动时更新
- 总共约 15-20 个 ScrollTrigger 实例
- 移动端统一体验：GSAP 在小屏设备上性能表现一致

## File Impact

- 文件大小：147KB → ~257KB（GSAP core ~60KB + ScrollTrigger ~50KB 内联）
- 不新增任何外部依赖文件

## Implementation Order

1. 添加 GSAP core（CDN 压缩版内联）
2. 添加 ScrollTrigger 插件（CDN 压缩版内联）
3. Hero 加载动画
4. ScrollTrigger 注册 + 各区域入场动画
5. 卡片 hover 增强
6. 模态框动画
7. 导航栏 GSAP 化
8. 画廊视差
