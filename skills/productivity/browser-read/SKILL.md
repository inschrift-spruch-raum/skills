---
name: browser-read
description: "用隔离的 Chromium 实例读取被 Cloudflare 人机验证或登录墙挡住的网页。Use when 抓取返回 403 且响应头带 Cf-Mitigated: challenge，页面出现 Just a moment…／正在进行安全验证／Verify you are human，或页面必须靠 JS 渲染、登录态才能读到正文。不要先试 archive、Wayback、截图服务或公共代理。"
---

# 浏览器读取网页

直接 HTTP 抓取被 Cloudflare 拦住时，唯一稳定可行的是**驱动一个真实 Chromium** 完成验证，再从 DevTools 协议（CDP）读 DOM。本配方已在 encode.su 上跑通并通过。

## 硬规则

- 只用 Chromium：`%LOCALAPPDATA%\Chromium\Application\chrome.exe`。**不要用 Edge**：用户明确禁止；且 Edge 在本机的 `--user-data-dir` 不隔离账号与扩展，会带上用户身份。
- 永远新建隔离 profile（`%TEMP%\chromium_iso_<ts>`），不碰用户正在运行的浏览器实例。
- 结束必须清理：关浏览器、删 profile、删临时文件、校验无残留。用户对"浏览器带来的存储"零容忍。
- **不要**在浏览器之前先试 archive.today / Wayback / thum.io / mShots / 公共 CORS 代理。这些路径在原始验证中均失败，消耗了约 30 次工具调用。

## 步骤

1. **启动**（detached，父进程退出后浏览器仍存活）

   ```
   chrome.exe --user-data-dir=%TEMP%\chromium_iso_<ts> --remote-debugging-port=9666
     --remote-allow-origins=* --no-first-run --no-default-browser-check
     --disable-sync --disable-extensions --start-maximized --new-window <url>
   ```

   完成判据：`http://127.0.0.1:9666/json/list` 能返回 page target。

2. **连接**：轮询 `/json/list`，取 page target 的 `webSocketDebuggerUrl`；Node 24+ 用内置 `WebSocket` 直连，无需依赖。

3. **置前**：`Page.bringToFront` + `Runtime.enable`。后台标签页里 Turnstile 永远不完成。

4. **等待** 10–15 秒，再 `Page.captureScreenshot`。早于 8 秒会截到加载态。

5. **定位复选框**：对**视口截图**做像素扫描（亮度阈值或 ASCII 图）找那个约 20px 的方块，按 `CSS = 设备像素 / devicePixelRatio` 换算（本机 DPR 1.5）。

6. **点击**：`Input.dispatchMouseEvent` 依次 mouseMoved → mousePressed → mouseReleased，坐标用**视口 CSS 坐标**。

   完成判据：`document.title` 在 3–6 秒内变成真实标题，且 `document.body.innerText.length` > 2000。

7. **抓取**：`document.body.innerText`；链接用 `document.querySelectorAll('a[href*="github.com"]')`；多页改 `?page=N` 重跑。

8. **清理**：`Browser.close` → 删 `%TEMP%\chromium_iso_*` → 删工作区里的脚本与截图。

   完成判据：无残留 chrome 进程、无 `chromium_iso_*` 目录、无本次会话新增文件。

## 坐标空间（最容易错的地方）

| 来源 | 坐标系 | 能否喂给 CDP |
|---|---|---|
| `Page.captureScreenshot` | 视口 CSS 像素（先除以 DPR） | ✅ 正确 |
| 系统屏幕截图（CopyFromScreen） | 屏幕坐标，含工具栏与窗口偏移 | ❌ 会差一个工具栏高度 |

混用表现为"点了没反应"；点击失败时重新截**视口**图算坐标，不要盲试。

## 失败模式

- 标签页在后台 → 验证不推进；先 `Page.bringToFront`。
- 复用用户默认 profile → 带上其账号/扩展，越权且触发同步弹窗。
- 等待不足就截图 → 截到加载态，找不到复选框。
- 用屏幕坐标点击 → 命中工具栏或空白处。
- 验证通过后仍读不到正文 → 检查是否被重定向到登录页；此时如实告知用户，不要伪造内容。
