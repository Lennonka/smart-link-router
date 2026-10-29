# Repository guide

## Project

Smart Link Router is a Chrome Manifest V3 extension that routes links from a source tab into another browser window. It uses plain JavaScript, HTML, and CSS with no build step, package manager configuration, or automated test suite.

## File map

- `manifest.json`: extension metadata, permissions, service worker, content script, popup, and icon declarations.
- `background.js`: session route storage, runtime message handlers, destination tab creation, toolbar badges, and cleanup when tabs or windows close.
- `content-script.js`: captures eligible link clicks and asks the service worker to route them.
- `popup.html`, `popup.css`, `popup.js`: destination selection, current route status, and set/clear actions.
- `icons/`: existing extension icons at 16, 48, and 128 pixels.

## Implementation conventions

- Match the existing three-space indentation, double-quoted JavaScript strings, and semicolons.
- Keep changes focused and use the existing browser APIs and native controls. Avoid adding dependencies or a build pipeline for routine changes.
- Keep message names and payloads consistent between the popup, content script, and service worker. The content script's route lookup uses `sender.tab`; the popup passes an explicit `tabId`.
- Routes are stored under `tabRoutes` in `chrome.storage.session`, mapping source tab IDs to destination window IDs. Do not change their lifetime or storage format incidentally.
- Keep asynchronous message channels open where responses are sent after awaiting work. Account for the Manifest V3 service worker lifecycle; do not rely on persistent in-memory state.
- Consider closed destination windows, missing active tabs, and unavailable destinations when changing routing behavior.
- When editing click interception, check normal clicks, modifier clicks, link targets, fragment links, and JavaScript links. Avoid unintended changes to navigation behavior.
- Do not expand permissions, host access, or bump the extension version unless the task requires it.

## Popup design

- Keep the layout compact: currently 320 pixels wide, with the extension's actual icon in the header.
- Use the CSS custom properties and `prefers-color-scheme` rules for light and dark colors. This follows the preferred color scheme, rather than reading custom Chrome theme colors.
- When a route exists, “Clear route” is the primary action and “Set route” is secondary. Without a route, “Set route” is primary and “Clear route” is disabled.
- Keep “Set route” text-only, without an arrow or other icon.
- Preserve explicit input labels, live status announcements, visible keyboard focus, and disabled states during loading or when actions are unavailable.

## Validation

Run checks appropriate to the files changed:

```sh
node --check background.js
node --check content-script.js
node --check popup.js
python3 -m json.tool manifest.json > /dev/null
git diff --check
```

There is no build or test command. For behavior changes, use focused checks with mocked Chrome APIs where useful.

## Manual testing

Validate in Chrome:

1. Open `chrome://extensions`, enable Developer mode, and load this directory as an unpacked extension, or reload the existing installation.
2. Refresh source pages after content-script changes so they receive the updated script.
3. With two normal browser windows, set a route and confirm an eligible link opens in the destination window and focuses it.
4. Reopen the popup, confirm the active route and action prominence, then clear the route and check navigation and badge state.
5. Check the no-destination state and cleanup after closing a source tab or destination window when relevant.
6. For UI changes, inspect both light and dark modes, keyboard navigation, and long window titles.

Report which checks actually ran and any browser verification that could not be completed. Syntax checks alone do not verify browser behavior.
