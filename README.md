# 7Day Chat — V2 starter

## What this build does
- Anonymous Supabase Auth sign-in
- Create a 7-day room and share a secret invitation URL
- Maximum two distinct Supabase user IDs per room (enforced by the database RPC)
- Browser-side AES-GCM encryption for message text
- Supabase Realtime INSERT subscriptions
- Messages are timestamped and expire at the earlier of 7 days after sending or room expiry

## Important security status
This is an early-stage starter, not an audited secure messenger. Do not use it for highly sensitive conversations until it has been independently reviewed and tested. Invitation links contain the decryption key in the URL fragment. Anyone with the full link can read the room. Do not paste links into public channels or analytics-enabled pages. Browser XSS or a compromised device can expose messages and keys.

The SQL setup already discussed in the ChatGPT conversation must be present. The `chat_rooms`, `room_members`, and `chat_messages` tables, RLS policies, `is_room_member`, `create_chat_room`, `join_chat_room`, and message expiry trigger are expected.

## Configure
1. Open `index.html`.
2. Replace `PASTE_YOUR_PROJECT_URL_HERE` with the Supabase Project URL.
3. Replace `PASTE_YOUR_PUBLISHABLE_KEY_HERE` with the `sb_publishable_...` key. Never put an `sb_secret_...` key in browser code.
4. Save the file.

## Run locally
Because this uses ES modules and browser cryptography, serve it over localhost or HTTPS (not `file://`).
For example, with Python installed:
`python -m http.server 8000`
Then open `http://localhost:8000`.

## Deploy
Upload the folder to a static host that serves HTTPS, such as Cloudflare Pages. Do not add server secrets to the frontend.

## Before real use — required review
- Add rate limiting to room creation, joining, and message sending.
- Configure CAPTCHA or another abuse-control mechanism if public sign-ups are exposed.
- Review RLS and SECURITY DEFINER function ownership/search_path/EXECUTE grants.
- Confirm realtime authorization and two-user access with two separate browsers.
- Add a server-side scheduled cleanup and verify cron execution.
- Decide whether old invitation links should be invalidated after the second participant joins.
- Add safe handling for refresh/reconnect and lost invitation keys.
- Avoid analytics, third-party scripts, and logging that could expose invite fragments.
- Test browser console, network requests, and unauthorized access attempts.

The app uses a public publishable key by design. Database security must be enforced by RLS and carefully reviewed database functions.
