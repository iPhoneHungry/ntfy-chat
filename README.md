# ntfy-chat

Anonymous, no-signup chat that runs entirely in your browser, backed by a public [ntfy.sh](https://ntfy.sh) topic. No accounts, no backend, no data collection — just a room name and a display name you pick.

**Live app:** https://iphonehungry.github.io/ntfy-chat/

## Start a chat with a friend

1. Open the app: https://iphonehungry.github.io/ntfy-chat/
2. When prompted, pick a room name and the name you want to be shown as.
3. Once you're in, the URL updates to include both, e.g.:
   `https://iphonehungry.github.io/ntfy-chat/?room=friday-plans-8f2k&name=Craig`
4. **Bookmark that URL** — reopening it skips the prompt and drops you right back into the room.
5. Send your friend the same link, with `&name=` swapped for their name (or just let them fill in their own name when the page asks). Same room name = same conversation.

Anyone who has the URL — or who guesses the room name — can read and post in that room. It's not a private, authenticated channel.

## ⚠️ Room names are public — don't use this for anything sensitive

Every room here is a public ntfy.sh topic under the hood. There's no login and no access control — if someone guesses or stumbles onto a room name, they can read everything posted there (ntfy retains messages for a few hours) and post to it too.

Pick a room name with real entropy — a long, unguessable string, not something like `date-night` or `family-chat`. `friday-plans-x7q2m9k4` is far safer than `chat`.

Even with a hard-to-guess room name:
- **Never share anything sensitive** — passwords, financial details, addresses, health info, or anything you wouldn't want a stranger to stumble across.
- Treat it like a note left on a public corkboard: "my favorite show is on tonight," "dinner's ready in 30," "running 10 min late" — that's exactly the kind of thing this is built for.
- This is provided as-is for casual, low-stakes use. **We take no responsibility for who else may see, post to, or do anything with a room you set up.**

## Using it to watch a notification from a script or agent

Since this is really just a friendlier UI over a ntfy topic, you can also point it at a topic that your own scripts, cron jobs, or AI agents post to — for example, to know when a long-running process finishes — without installing the ntfy app.

1. Generate a long, random room name yourself so it can't be guessed, e.g. `build-done-93jf82k1lq7x`.
2. Have your script or agent post to it directly:
   ```bash
   curl -d "Build finished successfully" "https://ntfy.sh/build-done-93jf82k1lq7x"
   ```
3. Open the app pointed at that room to watch for updates:
   `https://iphonehungry.github.io/ntfy-chat/?room=build-done-93jf82k1lq7x&name=me`

With enough entropy in the room name, this behaves like a private notification channel in practice — but it's still technically a public ntfy topic, so don't post secrets through it.

## How it works

- Single static HTML file — no build step, no backend of its own.
- All messages and images post straight to `https://ntfy.sh/<room>`, a public pub/sub service.
- Loads the last 10 messages on open, then streams new ones live over Server-Sent Events.
- Images over 2MB are automatically resized/recompressed in-browser before upload.
- Room + display name live in the URL (`?room=&name=`) so the page is bookmarkable, and are mirrored to `localStorage` for convenience.

## Running it yourself

It's a static site with no dependencies — clone the repo and open `index.html`, or host the folder anywhere that serves static files. This repo is itself deployed via GitHub Pages.
