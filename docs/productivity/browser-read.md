## What it does

`browser-read` reads a web page in a real Chromium browser when a plain HTTP fetch cannot reach its content. It uses a fresh, isolated profile and reads the rendered page through the Chrome DevTools Protocol (CDP).

The session does not reuse your normal browser profile, accounts, or extensions. Isolation also means that an existing logged-in browser does not automatically make the new session authenticated.

## When to reach for it

Type `/browser-read`, or the [agent](https://www.aihero.dev/ai-coding-dictionary/agent) reaches for it automatically when a task fits.

| Situation | Use |
| --- | --- |
| HTTP fetching returns a Cloudflare challenge, or the page says “Just a moment” or “Verify you are human” | `browser-read` |
| The text only appears after JavaScript rendering | `browser-read` |
| A page requires login | An isolated browser can reveal the login requirement, but does not guarantee access to the protected content |
| You want a cited investigation across several sources | [research](https://aihero.dev/skills-research), which addresses the research question rather than just opening a page |

## Prerequisites

This is a Windows-specific recipe. It expects Chromium at `%LOCALAPPDATA%\Chromium\Application\chrome.exe` and a Node.js runtime with built-in WebSocket support; the supplied recipe uses Node 24 or later. Its local CDP port must be available.

The page must be accessible to you. The skill does not supply credentials or reuse another browser's login state. Temporary browser data and any helper files need a writable location and must be cleaned up after the session. The agent records the exact profile path, browser process, and temporary files created for this read, and removes only those resources.

## Isolation and rendered content

The useful result is the rendered text, not a screenshot of a loading page. The recipe brings the page to the foreground, checks whether the real content appeared, and uses viewport coordinates rather than whole-screen coordinates when interaction is needed. Pixel coordinates must use the current device's pixel ratio, not assume the original machine's ratio.

## Common questions

**Can it use the browser where I am already signed in?**

No. The recipe starts a separate Chromium profile. It leaves the regular browser alone, so its accounts and extensions are not inherited.

**What if the page still asks me to log in?**

The agent reports that access is still blocked. A passed browser challenge does not imply that an account-protected page has become readable, and it must not invent the missing content.

## It's working if

- The returned text is the requested page content rather than the challenge or login screen.
- Your existing browser session and profile are untouched.
- The isolated browser session and its temporary data are cleaned up when the read finishes.
- If access is still blocked, the response says so rather than presenting guessed text as a successful read.

## Where it fits

This is a reach-for-it-anytime standalone page-reading tool. It belongs in Productivity because browser access is useful outside code work. [research](https://aihero.dev/skills-research) is the neighbour for a wider investigation and a cited write-up. [ask-matt](https://aihero.dev/skills-ask-matt) maps it alongside the rest of the skills.
