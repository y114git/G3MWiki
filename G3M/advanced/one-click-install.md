# One-click install links

A `g3m://` link asks G3M to import a downloadable package. The application shows a confirmation before processing it. If G3M is already running, the request is forwarded to that instance.

## Download links

For a direct HTTPS download, prefix the complete URL:

```text
g3m://https://example.com/mods/interface-pack.zip
```

The target must be an HTTP or HTTPS address. A GameBanana mod page works: G3M imports it from that page. Other web pages do not work. Use the archive's own download URL for those. The parser treats a comma as the start of additional link data, so percent-encode commas belonging to a URL.

## Local files

G3M also accepts a local file URI:

```text
g3m://file:///C:/Users/Player/Downloads/interface-pack.zip
```

On Linux or macOS, the path begins with that system's absolute file path, for example `g3m://file:///home/player/Downloads/interface-pack.zip`. Encode spaces as `%20`. The file must exist on the receiving computer; directories and network shares are not accepted through this local-file route.

The [web editor](../mods/web-editor.md) uses this route after you download its ZIP and supply the saved file's full path. A browser cannot silently write a mod into G3M's folders.

## Browser handoff

The operating system must register G3M as the handler, and the browser can request permission to open an external application. If no application opens, use normal local import while checking that registration.

While a game is running, G3M ignores import links and shows **The game is already running. Close it first.** Pairing links (`g3m://gb_pair/...`) still work during a game. `deltahub://` links are also accepted. Both schemes require review of the target and use the same download/import confirmation.

## GameBanana Remote Install

Remote Install lets you send a mod to G3M from the GameBanana website, even from another device, while G3M is running.

1. On a GameBanana file, open the arrow next to the G3M 1-click button and choose **Enroll & Pair**.
2. Click **Enroll**, then **Pair**. The browser opens a `g3m://gb_pair/...` link. G3M asks once whether to link this account.
3. Choose **Remote Install** on any file. G3M checks the queue about once a minute and downloads each mod it finds, with no further prompt. The download appears in the Downloads list and installs like any other. The **Refresh** button checks the queue immediately and restarts the one-minute wait.

Only GameBanana files are accepted from the queue. The key is stored in G3M's settings file.

To switch to another GameBanana account, sign in to that account on GameBanana and use **Enroll & Pair** again. G3M asks the same question, and after you confirm, the new account replaces the previous one. The previous account's queue is no longer read.

G3M reads two queues, one for the app ID `G3M` and one for `GameMakerModManager`. If GameBanana rejects the key for one of them, G3M stops reading that queue. When both are rejected, G3M stops checking. **Refresh** tries again, and pairing again replaces the key.
