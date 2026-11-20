# UI 相关工程配置

用法：
- 新 Web 项目初始化时配置第 1～3 项。
- 已有项目使用现有配置。新增工具前先征得需求方同意。

## 1. Browserslist（新项目必配）
- 把「开工前」第 3 步确定的兼容范围写进 `package.json` 的 `browserslist` 字段，或 `.browserslistrc` 文件。
- 前缀处理、语法降级、兼容性检查等工具都读取这份配置。兼容范围只在这里声明一次。
- 示例，按项目实际调整：
  - 管理后台：`last 2 Chrome versions, last 2 Edge versions, last 2 Safari versions`
  - 面向公众：`> 0.5%, last 2 versions, not dead, iOS >= 13`

## 2. CSS 前缀（新项目必配，通常已内置）
- 禁止手写 `-webkit-` 等浏览器前缀。`base-styles.md` 中列出的非标准属性除外。
- 先确认构建工具是否已经处理前缀：
  - Next.js：内置 Autoprefixer。
  - Tailwind CSS v4：内置 Lightning CSS，会自动加前缀。
  - Vite：默认不加前缀。
- 构建工具没有处理时，添加 Autoprefixer（PostCSS 插件）。

## 3. Stylelint（按兼容范围决定）
- 面向公众、兼容范围较宽的项目：配置。
- 只支持新浏览器的管理后台：可以不配。
- 预设用 `stylelint-config-standard`。使用 SCSS 时改用 `stylelint-config-standard-scss`。Vue 单文件组件需要额外配置 HTML 语法解析。
- 加上兼容性插件 `stylelint-no-unsupported-browser-features`。它读取 Browserslist，用到不支持的 CSS 属性时报警告。
- 在 `package.json` 中提供 `lint:css` 命令。

## 4. 项目已有 ESLint 时，启用以下规则
- React：`react/button-has-type`、`react-hooks/rules-of-hooks`、`react-hooks/exhaustive-deps`。
- Vue：`vue/html-button-has-type`。
- 兼容范围较宽时，加上 `eslint-plugin-compat`。它读取 Browserslist，检查 JS API 的兼容性。
- 项目没有 ESLint 时，不为此单独安装。

## 5. 截图检查（交付前验收使用）
先启动开发服务器。按顺序选择第一个可用的方式：

1. **Agent 自带的浏览器或截图工具。**
2. **本机 Chrome 或 Edge 的无头模式**，无需安装任何依赖：

   ```bash
   chrome --headless --window-size=1440,900 --screenshot=shot-desktop.png http://localhost:5173
   chrome --headless --window-size=390,844 --screenshot=shot-mobile.png http://localhost:5173
   ```

   - Windows：Chrome 通常在 `C:\Program Files\Google\Chrome\Application\chrome.exe`，Edge 在 `C:\Program Files (x86)\Microsoft\Edge\Application\msedge.exe`。
   - macOS：`/Applications/Google Chrome.app/Contents/MacOS/Google Chrome`。
   - 这种方式只能截取首屏，无法点击和模拟触摸。适合检查布局、层级和溢出。
   - 命令会输出一些 ERROR 日志，通常和截图无关。以图片文件是否生成为准。
3. **项目已安装 Playwright 时**，用它截图并检查交互。
4. **以上都不可用时**，跳过截图，在验收清单中注明。

截图后查看图片，对照原则和组件规范检查。截图文件放在临时目录，不提交到仓库。
