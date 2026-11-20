# 基线样式与浏览器差异

用法：
- 新项目初始化时逐项落实。
- 使用 Tailwind Preflight 等自带 reset 的框架时，只补齐框架没有覆盖的项。禁止再叠加一套 reset。
- 已有项目：只在本次修改涉及的地方落实。其余缺失项在回复中列出，经需求方同意后再补。
- 每条只给处理方向。具体代码按项目技术栈编写。

## 通用
- **表单控件字体**：`button`、`input`、`select`、`textarea` 默认不继承页面字体。统一设置 `font: inherit; color: inherit`。
- **按钮误提交**：表单内的 `<button>` 默认是提交按钮。非提交按钮写 `type="button"`。表单提交时阻止默认行为，避免页面刷新。
- **焦点样式**：禁止全局 `outline: none`。用 `:focus-visible` 提供清晰可见的焦点样式。
- **中文字体栈**：英文字体之后写中文系统字体，如 `-apple-system, BlinkMacSystemFont, "Segoe UI", "PingFang SC", "Hiragino Sans GB", "Microsoft YaHei", sans-serif`。缺少中文字体时会回退到宋体。
- **图片和媒体**：`img`、`svg`、`video` 设为 `display: block; max-width: 100%`，避免底部空隙和溢出。
- **长文本**：长链接、长英文单词设置 `overflow-wrap: anywhere`，避免撑破布局。
- **自动填充**：Chrome 自动填充后输入框会变成黄色或蓝色。覆盖 `:-webkit-autofill` 样式，与输入框背景保持一致。
- **数字输入框**：隐藏上下箭头，禁止滚轮改变数值。或改用文本框加 `inputmode="numeric"`。
- **搜索框**：隐藏 `type="search"` 的原生清除按钮，使用自定义的清除按钮。
- **滚动条宽度**：Windows 上滚动条会占用宽度。给 `html` 设置 `scrollbar-gutter: stable`，弹窗锁定滚动时页面不横向跳动。Safari 18.2 起支持，旧版本忽略即可。

## 移动端与 iOS

### 禁止页面缩放（必须落实）
输入框聚焦、双击、双指都不能让页面缩放。以下 4 项全部落实，缺一项都会在某些设备上漏掉：

1. **viewport**：`width=device-width, initial-scale=1, maximum-scale=1, user-scalable=no, viewport-fit=cover`。Android 上的浏览器据此禁止全部缩放。iOS Safari 据此阻止输入框聚焦放大，双指缩放的限制会被它忽略。
2. **控件字号**：输入框、下拉、文本域的字号至少 16px。部分浏览器忽略 viewport 设置时，这一项防止聚焦放大。
3. **CSS**：`html` 设置 `touch-action: pan-x pan-y`。禁止双击和双指缩放，保留上下左右滚动。
4. **iOS Safari 兜底**：前三项落实后，iOS 上仍能双指缩放时，只加一段 JS：在 `document` 上监听 `gesturestart`，调用 `preventDefault()`。禁止再写其他拦截缩放的代码。

需要缩放的特定区域（图片预览、地图、文档查看器）：
- 使用对应的组件，在区域内实现缩放。页面本身保持不可缩放。
- 给该区域设置 `touch-action: none`，手势交给组件处理。

落实后在 iOS 和 Android 真机上分别验证。无法验证时，在验收清单的「未验证」中列出。

### 其他
- **横屏字号**：设置 `-webkit-text-size-adjust: 100%`，防止 iOS 横屏后文字变大。
- **点击高亮**：设置 `-webkit-tap-highlight-color: transparent`，同时提供自己的按下反馈。
- **控件外观**：输入框、下拉、按钮设置 `appearance: none` 后自定义样式，去掉 iOS 默认的圆角和内阴影。
- **视口高度**：先写 `100vh` 作为回退，再写 `100dvh`。
- **安全区**：底部固定的元素加上 `env(safe-area-inset-bottom)` 的内边距。
- **滚动穿透**：弹窗和抽屉的滚动锁定按 `references/components/modal.md` 的「实现方式」处理，禁止手写触摸事件拦截。

## 常见错误
- 框架已有 reset，又引入 normalize.css，样式互相覆盖。
- 为了去掉点击后的蓝框，全局写了 `outline: none`。
- 只在 viewport 里写了 `user-scalable=no`，iOS 上仍然可以双指缩放。
- 只写了英文字体，Windows 上中文显示为宋体。
