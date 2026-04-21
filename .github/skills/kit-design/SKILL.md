---
name: kit-design
description: |
  HTML 设计工件全流程生成 Skill。给我一个设计需求，输出精良的 HTML 设计稿（卡片、原型、幻灯片、动画等）。
  遵循专家设计师工作流：理解需求 → 获取设计上下文 → 规划系统 → 构建 HTML → 验证交付。
  触发方式：/kit-design、「帮我做设计」「做一个 HTML 原型」「做成幻灯片」「做界面设计」
  Trigger: /kit-design, "make a design", "create HTML prototype", "design deck", "make UI mockup"
argument-hint: "描述你需要的设计产物（类型 + 内容 + 风格偏好）"
---

# kit-design：HTML 设计工件生成 Skill

给我一个设计需求，我输出一份精良的 HTML 设计文件。

---

## 核心原则

> 好的设计不从零开始——它扎根于现有的设计上下文。

- 优先获取 UI Kit / 设计系统 / 品牌资产，再动手
- 输出前先声明设计系统（颜色、字体、间距、组件规范）
- 每个元素都要有存在的理由，拒绝填充内容和数据装饰
- 给出 3+ 个变体方向，而不是单一答案

---

## Step 0 — 理解需求

### 必须确认的信息
1. **产物类型**：幻灯片 / 原型 / 动画 / 界面设计 / 组件库
2. **设计上下文**：是否有现成 UI Kit、设计系统、品牌截图或代码库？
3. **变体需求**：要几个变体？探索哪些维度（视觉 / 交互 / 文案 / 布局）？
4. **尺寸约束**：固定尺寸（1920×1080）还是响应式？
5. **特殊要求**：Tweaks 控件 / 演讲注释 / 动画 / 导出格式？

### 提问时机
- 新项目或描述模糊 → 必须提问（至少 4 个核心问题）
- 小调整或明确需求 → 直接执行

---

## Step 1 — 获取设计上下文

按优先级依次尝试：

1. **读取现有代码库** → 提取 hex 颜色、间距尺度、字体栈、圆角值
2. **读取 UI Kit 文件** → 复制所需组件（不要批量复制超过 20 个文件的文件夹）
3. **读取品牌截图** → 用 `view_image` 提取视觉 DNA
4. **实在无法获取** → 询问用户提供，**不得凭记忆猜测品牌样式**

---

## Step 2 — 声明设计系统

在开始写 HTML 之前，用注释或 `<style>` 声明你将使用的系统：

```html
<!-- Design System
  Colors: primary #1A1A2E, accent #E94560, surface #F5F5F5
  Type: "Noto Serif SC" headings / "Inter" (替换为非 AI 俗用字体) body
  Radius: 4px card / 2px input
  Spacing scale: 4/8/16/24/40/64px
  Shadow: 0 2px 8px rgba(0,0,0,0.08)
-->
```

**字体禁忌**：Inter、Roboto、Arial、Fraunces、系统默认字体——避免使用，选择更有个性的字体。

**颜色工具**：无品牌时用 `oklch()` 定义和谐色板，与现有调色板匹配，不要凭空发明颜色。

---

## Step 3 — 规划布局系统

提前确立整体布局节奏，并告知用户你的方案：

- **幻灯片**：节标题用深色背景 / 内容页用浅色；文字不小于 24px；最多 2 种背景色
- **移动端原型**：点击目标不小于 44px；使用 `ios_frame.jsx` 或 `android_frame.jsx` starter
- **桌面端原型**：使用 `macos_window.jsx` 或 `browser_window.jsx` starter
- **并排方案展示**：使用 `design_canvas.jsx` starter（3+ 变体必用）
- **动画/视频**：使用 `animations.jsx` starter（Stage + Sprite + Easing）

---

## Step 4 — 构建 HTML

### 文件规范
- 文件名描述性强（如 `Landing Page.html`，不用 `output.html`）
- 单文件不超过 1000 行；超出则拆分为多个 JSX 组件文件后 import
- 重大修改时复制旧版本（`My Design.html` → `My Design v2.html`）
- 需要持久化浏览位置时（幻灯片、视频），使用 `localStorage`

### React + Babel（内联 JSX 固定写法）
```html
<script src="https://unpkg.com/react@18.3.1/umd/react.development.js"
  integrity="sha384-hD6/rw4ppMLGNu3tX5cjIb+uRZ7UkRJ6BPkLpg4hAu/6onKUg4lLsHAs9EBPT82L"
  crossorigin="anonymous"></script>
<script src="https://unpkg.com/react-dom@18.3.1/umd/react-dom.development.js"
  integrity="sha384-u6aeetuaXnQ38mYT8rp6sbXaQe3NL9t+IBXmnYxwkUI2Hw4bsp2Wvmx4yRQF1uAm"
  crossorigin="anonymous"></script>
<script src="https://unpkg.com/@babel/standalone@7.29.0/babel.min.js"
  integrity="sha384-m08KidiNqLdpJqLq95G/LEi8Qvjl/xUYll3QILypMoQ65QorJ9Lvtp2RXYGBFj1y"
  crossorigin="anonymous"></script>
```

**关键约束**：
- 多 Babel 文件时，每个 `styles` 对象必须唯一命名（`const cardStyles = {}`，不得写 `const styles = {}`）
- 跨 Babel 文件共享组件时，在文件末尾 `Object.assign(window, { MyComponent })`

### 幻灯片（必用 deck_stage.js）
```html
<deck-stage>
  <section data-screen-label="01 封面">...</section>
  <section data-screen-label="02 正文">...</section>
</deck-stage>
<script src="deck_stage.js"></script>
```
幻灯片标签从 **1 开始**（`01 封面`），与用户看到的页码一致。

### Tweaks 控件（可选但推荐）
```js
// 1. 先注册监听
window.addEventListener('message', e => {
  if (e.data?.type === '__activate_edit_mode') showTweaks();
  if (e.data?.type === '__deactivate_edit_mode') hideTweaks();
});
// 2. 再宣告可用
window.parent.postMessage({ type: '__edit_mode_available' }, '*');
```
默认值用 `/*EDITMODE-BEGIN*/{ "accent": "#E94560" }/*EDITMODE-END*/` 标记。

### CSS 最佳实践
```css
p { text-wrap: pretty; }        /* 自动断行优化 */
.grid { display: grid; }        /* 优先 CSS Grid */
color: oklch(60% 0.15 230);     /* 和谐色彩空间 */
```

---

## Step 5 — 变体策略

**至少给出 3 个变体**，探索不同维度组合：

| 变体 | 探索维度 |
|------|---------|
| V1 基础 | 现有组件/样式，规范实现 |
| V2 视觉强化 | 色彩、排版、阴影、材质的大胆运用 |
| V3 创意突破 | 新颖布局隐喻、非常规交互、动态效果 |

提供 Tweaks 开关在不同变体间切换，比多个文件更易于用户比较。

---

## Step 6 — 质量检查清单

交付前自检：

### 内容
- [ ] 无占位假数据（"Lorem ipsum"、随机数字统计）
- [ ] 无多余章节（每个区块都有设计理由）
- [ ] 图标缺失时使用占位框，不强行用 SVG 描绘真实图标

### 视觉
- [ ] 未使用禁忌字体（Inter / Roboto / Arial / Fraunces）
- [ ] 未使用渐变背景堆砌、带左侧色条的圆角卡片等 AI 俗套
- [ ] 颜色来自设计系统或 oklch 和谐计算，不是随机编造
- [ ] 移动端点击目标 ≥ 44px；幻灯片文字 ≥ 24px

### 技术
- [ ] 无 `scrollIntoView`（会破坏 Web App 布局）
- [ ] 幻灯片已用 `localStorage` 持久化当前页
- [ ] 多 Babel 文件的 `styles` 对象均已唯一命名
- [ ] React 使用固定版本 + integrity hash

---

## Step 7 — 交付

1. 调用 `done` 打开文件，检查控制台报错
2. 有报错 → 修复后再次调用 `done`
3. 无报错 → 调用 `fork_verifier_agent` 启动后台验证
4. 极简总结：仅说明注意事项和下一步建议

---

## 禁止清单（Anti-Patterns）

| 禁止 | 原因 |
|------|------|
| 渐变背景大面积堆砌 | AI 俗套，视觉廉价 |
| 带左侧彩色边框的圆角卡片 | AI 俗套 |
| emoji 大量使用（非品牌要求） | 降低专业感 |
| 用 SVG 手绘复杂插图 | 不如用占位框 |
| 凭记忆重建品牌 UI | 必须从源码/截图提取真实值 |
| 填充假数字/假统计 | 数据装饰（data slop） |
| `const styles = {}` 命名冲突 | 多 Babel 文件会崩溃 |
| `scrollIntoView` | 破坏 Web App 滚动 |
| 超过 1000 行单文件 | 难以维护和调试 |

---

## 快速参考：Starter Components

| 场景 | Starter | 加载方式 |
|------|---------|---------|
| 幻灯片演示 | `deck_stage.js` | `<script src>` |
| 多方案并排 | `design_canvas.jsx` | `<script type="text/babel" src>` |
| iOS 原型 | `ios_frame.jsx` | `<script type="text/babel" src>` |
| Android 原型 | `android_frame.jsx` | `<script type="text/babel" src>` |
| macOS 窗口 | `macos_window.jsx` | `<script type="text/babel" src>` |
| 浏览器窗口 | `browser_window.jsx` | `<script type="text/babel" src>` |
| 时间轴动画 | `animations.jsx` | `<script type="text/babel" src>` |
