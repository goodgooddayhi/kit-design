# kit-design

**中文** | [English](#english)

一个用于 Claude 的 HTML 设计工件生成 Skill。给出设计需求，输出精良的 HTML 设计稿。

## 使用方式

将 `SKILL.md` 放入你的 Claude 项目 `.github/skills/kit-design/` 目录，或直接作为 Skill 导入。

触发方式：`/kit-design`、「帮我做设计」、「做成幻灯片」、「做 HTML 原型」

## 功能

- 幻灯片 / 原型 / 动画 / 界面设计 / 组件库全类型支持
- 强制先获取设计上下文（UI Kit / 品牌资产），不凭空猜测
- 自动输出 3+ 变体方向
- 内置质量检查清单，防止 AI 俗套（渐变堆砌、假数据、禁忌字体等）
- 集成 React + Babel 固定版本、Tweaks 控件协议、Starter Components

---

<a name="english"></a>

# kit-design

[中文](#top) | **English**

A Claude skill for generating high-quality HTML design artifacts. Describe what you need, get a polished HTML output.

## Usage

Place `SKILL.md` in your Claude project under `.github/skills/kit-design/`, or import it directly as a skill.

Trigger with: `/kit-design`, "make a design", "create HTML prototype", "design deck", "make UI mockup"

## Features

- Supports all artifact types: decks, prototypes, animations, UI mockups, component libraries
- Enforces design context first (UI Kit / brand assets) — no guessing from memory
- Always produces 3+ design variants
- Built-in quality checklist against AI clichés (gradient abuse, fake data, banned fonts, etc.)
- Integrates pinned React + Babel versions, Tweaks protocol, and Starter Components
