# Epistle · 尺素（Tang 改本）

> 一个把博客当成「一叠私人信札」的 Hugo 主题。
> 这是 [yibie/hugo-theme-epistle](https://github.com/yibie/hugo-theme-epistle) 的二次开发分支，改动见下文「与上游的差别」。

---

## 设计哲学

博客不是信息流，是**书桌上的一叠信**：

- 首页是摊开的最新一封，点右侧「信匣」抽出归档
- 每篇都有称谓、正文、祝颂语、手写签名、落款日期
- 纸会旧，墨会淡——按写信的年份自动推，不用手选皮肤

---

## 与上游的差别

上游给了五款手选信纸（白纸蓝墨 / 暖笺 / 晴空 / 夜信 / 春笺）。这一版把它们换掉了：

| 改动 | 做法 |
| --- | --- |
| **信龄自动老化** | 由 `date` 算出 `--age: 0→1`，纸色、墨色、小字、朱墨一起插值。三年前的信自然泛黄、墨迹变淡，六年以上到顶 |
| **手写签名** | 内置一条单线笔迹的 SVG 签名（`layouts/partials/signature.html`），带 `feDisplacementMap` 墨迹毛边，颜色继承 `--ink`，会跟着信龄一起淡 |
| **三折折痕** | 超过 600 字的长信自动叠三折，两条折痕按 1/3、2/3 落在纸上；读到哪一折，哪一折的压痕就松开，等于一条不用画的进度条 |
| **夜读 = 台灯** | 暗色不是换底色：一层固定的径向遮罩把中心留给信纸，四周真的暗下去，再叠一小圈暖光。不是全黑底 |
| **素纸模式** | 一键关掉纸纹、树影、楷体、灯罩，回到纯白底黑字的干净正文（对比度按 WCAG AA 走） |
| **减动效** | `prefers-reduced-motion: reduce` 时直接不生成树影，不是暂停动画 |
| **打印 / 导出 PDF** | `@media print` 是一张真信纸：A4、白底黑字、页眉有站名与日期、落款与签名不跨页、代码块自动换行不断页 |
| **Heti 本地化** | 赫蹏的 CSS/JS 打包进主题，不再依赖 unpkg CDN（国内访问更稳） |

---

## 安装

```bash
cd your-hugo-site
git submodule add https://github.com/cyjx007/hugo-theme-epistle.git themes/epistle
```

或直接把 `layouts/`、`static/`、`theme.toml` 拷进 `themes/epistle/`。

## 配置

```toml
theme = 'epistle'
summaryLength = 0

[params]
  author = '你的名字'
  tagline = 'Linux · 网络 · 折腾'
  description = '一句话介绍'
  valediction = '此致'          # 祝颂语
  showSignature = true          # 是否显示签名
  signatureName = 'Tang'        # 签名的无障碍名称
  dappledLight = true           # 树影开关
  ageSpanYears = 6              # 多少年到达"最旧"
  ageCurve = 0.45               # 老化曲线（越小越早泛黄）
```

## 文章 Front Matter

```yaml
---
title: "标题"
date: 2026-09-04T11:15:25+08:00
draft: false
slug: "我的-文章"        # 想保留旧 URL 就写死
salutation: "见字如面，"  # 可选
postscript: "又及：……"    # 可选
tags: ["Linux"]
categories: ["折腾"]
---
```

## 阅读设置

页眉右上角三个开关，状态存在 `localStorage`：

- **夜读 / 日读**：昼夜切换（首次跟随系统 `prefers-color-scheme`）
- **素纸**：关掉所有装饰
- **打印**：直接调起打印对话框，另存为 PDF 即可

也支持 URL 参数临时覆盖：`?theme=dark`、`?plain=1`。

---

## 许可

GPL-3.0-or-later，与上游一致。
