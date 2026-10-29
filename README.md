# Smart Link Router

Open links from one tab in another Chrome window. Keep a search page, inbox, or reading list in one window while the pages you open from it go to another.

Each source tab can have its own destination window, so you can organize several workflows at once.

## Install from source

1. Download or clone this repository. If you download a ZIP, extract it first.
2. In Chrome, open `chrome://extensions`.
3. Turn on **Developer mode**.
4. Click **Load unpacked** and select the folder containing `manifest.json`.
5. Refresh any already-open pages where you want to use routing.

For easy access, open Chrome’s Extensions menu and pin **Smart Link Router** to the toolbar.

No build step or additional software is required.

## Route links to another window

1. Open at least two normal Chrome windows.
2. In the tab whose links you want to route, click the extension’s toolbar icon.
3. Choose a **Destination window**. Each entry shows that window’s active tab title and tab count; the current window is excluded.
4. Click **Set route**.
5. Click a link in the source tab. It opens as a new tab in the destination window, and Chrome brings that window into focus.

A badge on the extension’s toolbar icon indicates that the current tab has a route. The popup also shows its route status.

To change the destination, reopen the popup, select another window, and click **Set route**. To stop routing, click **Clear route**, which is highlighted when a route is active.

## What to expect

- Routing applies to ordinary left-clicks on links, including links that request a new tab. Ctrl, Command, Shift, Alt, and middle-clicks are left to the browser.
- Same-page fragment links and JavaScript links are skipped. Links targeting a named frame or targets such as `_parent` and `_top` are also skipped.
- Routing follows the source tab, including when you navigate that tab to another page where the extension can run. It does not automatically apply to other tabs or to newly opened destination tabs.
- Routes are temporary: closing the source tab or destination window removes the route. Routes also reset when Chrome restarts or the extension is reloaded, updated, or disabled.
- The popup adapts to your preferred light or dark color scheme.

## Troubleshooting

**“No other windows available”**: Open another normal Chrome window, then reopen the popup.

**Links are not being routed**: Refresh the source page after installing or reloading the extension. Check that the extension has access to the site and that a destination is still selected. Chrome restricts extensions on some pages, including its internal pages and the Chrome Web Store.

**A website behaves differently**: The extension intercepts eligible link clicks, and some websites use their own navigation handlers. Clear the route or disable the extension for that site if you encounter issues. Without a route, eligible clicks navigate the current tab, including links that normally request a new tab.

## Permissions and privacy

The extension uses access to tabs and windows to list destinations, open links, focus the destination window, and maintain the route badge. It uses session storage to keep your routes during the browser session.

Access to websites lets the extension detect link clicks on pages where Chrome permits it to run. The current code has no analytics or external data collection; route settings are stored locally in the browser session. Opening a routed link contacts its destination website as normal.

## Contributing

This is a plain JavaScript, HTML, and CSS extension. See [AGENTS.md](AGENTS.md) for the repository layout, implementation guidance, validation commands, and manual testing steps.
