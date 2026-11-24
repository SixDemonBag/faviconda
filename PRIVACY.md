# Faviconda on Your Mac

This page lists everything Faviconda does on your Mac, and why. It explains the permissions it asks for, the files it reads, changes and creates, and how to remove it.

**In short:** Faviconda works only with files on your Mac. It has no accounts, analytics or crash reporting. Its only network request is a daily check for a newer version, which you can turn off. It reads Safari's icon cache and bookmarks, and changes only the icon cache.

## Permissions

| Permission | Why |
| --- | --- |
| **Full Disk Access** | Safari keeps its icons in `~/Library/Safari`, which macOS protects. Without this permission Faviconda can't read or write the icons. |
| **The Safari extension, allowed on all websites** | Safari downloads a site's own icon when you visit it. The extension needs to see each page's address to keep your icon in place on the sites you've customised. |
| **Open at login** | The menu bar app re-applies your icons each time Safari quits, so it has to be running. It is turned on the first time you open Faviconda, and you can turn it off from its menu bar icon. |

Faviconda doesn't ask to control other apps, and doesn't use the camera, microphone, location, contacts, calendars or photos.

## What Runs, and When

| What | When | Why |
| --- | --- | --- |
| **Apply to Safari** | When you click it | Writes your icons into Safari's cache. It asks Safari to quit first, the same as choosing Quit Safari, because Safari overwrites its cache while it is open. Safari reopens afterwards only if it was open before. |
| **Re-apply on quit** | Each time Safari quits, and when Faviconda opens with Safari closed | Puts back any icon Safari replaced while it was open. |
| **Icon guard** | While Faviconda is running | Watches Safari's icon folder for changes, and puts your icon file back if Safari overwrites it. You can turn it off in Settings. |
| **Status check** | Every minute while the window is open | Reads Safari's cache to show whether each site is showing your icon. |
| **Lost-icon log** | Every 30 seconds while Safari is open | Notes the moment a bookmark stops showing your icon, to help find the cause. You can turn it off in Settings. |
| **Update check** | About once a day, if turned on (it is by default) | Asks GitHub for the version number of the newest Faviconda release, to show a banner when there is one. See below. |
| **The extension** | On every page you open in Safari | Checks the page's address against your sites. On a site you've customised, it points the page's icon links at your icon and blocks the site's own icon files. On any other site it does nothing more. |

Faviconda stays running in the menu bar when its window is closed. macOS's App Nap is turned off for it, so it doesn't miss Safari quitting.

## The Update Check

Once a day, Faviconda sends one request to GitHub (`api.github.com`) for the newest release of Faviconda. The request holds nothing about you or your sites: no account, no cookies, no identifier. GitHub sees your IP address, as any website would. If a newer version is out, the window shows a banner with a link to it. Nothing is downloaded until you click it, and the new version replaces the old one only when you open it.

Turn it off in Settings > Updates and Faviconda never connects to the internet. **Check for Updates…** in the menus still checks once, when you choose it.

## Files It Reads

- **Safari's icon cache** (`~/Library/Safari/Favicon Cache`). This includes a list of page addresses that have icons. Faviconda reads it to find the icons for your sites. For example, `https://domain.com/` and its icon.
- **Your bookmarks** (`~/Library/Safari/Bookmarks.plist`), to find the bookmarks your sites cover. It reads titles and addresses, and doesn't change or copy the file.
- **Safari's list of extensions** for each profile, to show a notice if the Faviconda extension is off or not allowed on all websites.

It never reads your History, Reading List, passwords, cookies, open tabs or page contents.

## Files It Changes

Faviconda changes only Safari's icon cache, and only while Safari is closed.

- It saves your images there and points your sites' bookmarks at them.
- When you remove a site, it hands that site back to Safari at the next Apply, and deletes the image it had added.
- Earlier versions locked icon files to stop Safari replacing them. Faviconda now removes any of those locks at each Apply.
- Earlier test versions changed dates in Safari's link preview cache (`~/Library/Safari/Metadata Cache`). Each Apply checks for any left over and resets them. This changes nothing on a Mac that never ran those versions.

## Files It Creates

In `~/Library/Application Support/Favicons`:

- **Your sites and images** (`rules.json`).
- **A record of the icon files it added** (`owned-slots.json`), so it can remove them when you remove a site.
- **Backups**, if turned on. Before each Apply, a full copy of Safari's icon cache. A backup includes the list of page addresses that have icons, with no titles or visit dates. For example, `https://domain.com/blog/post-1`. You choose how many to keep, or turn them off, in Settings. The first backup, Install State Backup, is kept until you delete it in Finder.
- **Logs**, each kept short: Apply, the icon guard, the quit watcher, and lost icons. They list site names only, never full addresses. For example, `domain.com: showing our icon -> missing`.
- **Diagnostic reports**, only if you turn on the Debug menu and ask for them. They hold samples of your bookmarks and cached pages, such as a bookmark's title and address. They are deleted the next time Faviconda opens.

The extension keeps its own files in its sandbox container (`~/Library/Containers/fyi.cole.safari-favicons.SafariExtension`), and its own storage inside Safari:

- a copy of your sites and images;
- the icon file addresses it blocks, for example `https://domain.com/favicon.ico`;
- a short log by site name;
- whether it is allowed on all websites.

**Something to know:** Safari's own folder is protected by Full Disk Access, but `~/Library/Application Support` is not. Any app you run can read the backups there. If you'd rather not keep a copy of the cache's page list, turn backups off in Settings and delete them. Time Machine and Spotlight can also copy or index these files, like anything else in your user folder.

## What It Never Does

- Send anything about you, your sites or your browsing anywhere. The block rules work inside Safari, and the update check is the only request it makes.
- Read or change anything on the pages you visit, other than the icon links of sites you've customised.
- Change your bookmarks, History or any Safari setting.
- Read files outside Safari's folder and its own.

## Removing Faviconda

1. Remove all your sites in Faviconda, then click **Apply to Safari**. Safari gets every site back, and downloads each site's own icon on your next visit.
2. Quit Faviconda from its menu bar icon.
3. Delete Faviconda from Applications. This also removes the extension from Safari and the login item.
4. Delete `~/Library/Application Support/Favicons`, including the backups, and the extension's folder, `~/Library/Containers/fyi.cole.safari-favicons.SafariExtension`.
5. Remove Faviconda from Full Disk Access in System Settings > Privacy & Security.
