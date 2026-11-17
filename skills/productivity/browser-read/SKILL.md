---
name: browser-read
description: "Read pages blocked by Cloudflare challenges or login walls using isolated Chromium. Use when a fetch returns 403 with Cf-Mitigated: challenge, the page shows Just a moment or Verify you are human (including localized equivalents), or content requires JavaScript rendering or authentication. Use the browser before archives, screenshot services, or public proxies."
---

# Read Web Pages with Chromium

When Cloudflare blocks direct HTTP fetching, the only consistently successful approach in the original test was to drive a real Chromium browser through verification, then read the DOM through the Chrome DevTools Protocol (CDP). This recipe was successfully validated on encode.su in that test.

## Hard rules

- Use only Chromium: `%LOCALAPPDATA%\Chromium\Application\chrome.exe`. **Do not use Edge**: the user explicitly prohibited it, and on the original machine Edge's `--user-data-dir` did not isolate accounts and extensions, exposing the user's identity.
- Always create a new isolated profile at `%TEMP%\chromium_iso_<ts>`. Record its exact absolute path, the browser process started for this run, and every temporary file created. Leave the user's running browser instances untouched.
- Clean up at the end: close this run's browser, delete its recorded profile and temporary files, and verify that its resources are gone. Leave all other browser processes, profiles, and files untouched. The user permits no storage left behind by this browser session.
- **Do not** try archive.today, Wayback, thum.io, mShots, or public CORS proxies before the browser. All of these failed in the original test, consuming about 30 tool calls.

## Steps

1. **Launch** detached so the browser survives the parent process exiting.

   ```
   chrome.exe --user-data-dir=%TEMP%\chromium_iso_<ts> --remote-debugging-port=9666
     --remote-allow-origins=* --no-first-run --no-default-browser-check
     --disable-sync --disable-extensions --start-maximized --new-window <url>
   ```

   Completion criterion: `http://127.0.0.1:9666/json/list` returns a page target.

2. **Connect**: poll `/json/list` and take the page target's `webSocketDebuggerUrl`. Use the built-in `WebSocket` in Node 24 or later to connect directly, without dependencies.

3. **Bring the page to the foreground** with `Page.bringToFront` and `Runtime.enable`. Turnstile does not complete in a background tab.

4. **Wait** 10-15 seconds, then use `Page.captureScreenshot`. Capturing before 8 seconds produces a loading-state image.

5. **Locate the checkbox**: scan the **viewport screenshot** using a brightness threshold or an ASCII representation to find the approximately 20px square. Convert with `CSS = device pixels / devicePixelRatio`. The original machine's DPR was 1.5.

6. **Click** using `Input.dispatchMouseEvent` in this order: mouseMoved, mousePressed, mouseReleased. Use **viewport CSS coordinates**.

   Completion criterion: `document.title` changes to the real title within 3-6 seconds, and `document.body.innerText.length` > 2000.

7. **Extract** `document.body.innerText`. For links, use `document.querySelectorAll('a[href*="github.com"]')`. For multiple pages, change `?page=N` and repeat.

8. **Clean up**: call `Browser.close` on this run's isolated instance, wait for its recorded process to exit, then delete the exact recorded profile path and temporary files. Use the ownership record instead of wildcard profile deletion or cleanup by process name.

   Completion criterion: this run's recorded browser process has exited, its exact profile directory is gone, and all temporary files created by this run are removed. Other sessions' processes and files remain untouched.

## Coordinate spaces

| Source | Coordinate system | Suitable for CDP input? |
| --- | --- | --- |
| `Page.captureScreenshot` | Viewport pixels; divide by DPR to obtain CSS coordinates | Yes |
| System screenshot (CopyFromScreen) | Screen coordinates, including toolbar and window offsets | No; the click will be offset by a toolbar height |

Mixing these spaces makes a click appear to do nothing. After a failed click, capture the **viewport** again and recalculate the coordinates instead of guessing.

## Failure modes

- Background tab: verification does not advance. Call `Page.bringToFront` first.
- Reusing the user's default profile: accounts and extensions are inherited, exceeding authorization and triggering sync prompts.
- Capturing too soon: the screenshot shows a loading state, so the checkbox cannot be found.
- Clicking with screen coordinates: the click hits the toolbar or empty space.
- Verification passes but the body is still unavailable: check for a redirect to a login page. Report the limitation honestly instead of inventing content.
