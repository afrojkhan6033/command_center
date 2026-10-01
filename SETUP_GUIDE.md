# Cloud Sync Setup Guide

Your Command Center now has real cross-device sync built in, powered by
[Supabase](https://supabase.com) (a hosted Postgres database + auth service
with a generous free tier). Nothing is pre-connected — you need your **own**
free Supabase project, and only you hold the keys to it. This takes about
5 minutes.

## 1. Create a free Supabase project
1. Go to https://supabase.com and sign up (GitHub or email).
2. Click **New Project**. Pick any name/region, set a database password
   (save it somewhere — you won't need it for the app itself, only if you
   ever want to connect other tools directly to the database).
3. Wait ~1-2 minutes while the project provisions.

## 2. Run the schema script
1. In your new project, open the left sidebar → **SQL Editor** → **New query**.
2. Open `schema.sql` (included alongside this guide), copy all of it, paste
   it into the SQL editor, and click **Run**.
3. You should see "Success. No rows returned." This creates all the tables,
   security rules (so only you can ever read your own data), and turns on
   realtime sync.

## 3. Get your API credentials
1. In the sidebar go to **Project Settings → API**.
2. Copy the **Project URL** (looks like `https://xxxxxxxx.supabase.co`).
3. Copy the **anon public** key (a long string starting with `eyJ...`).
   This key is *safe to put in client-side code* — it's meant to be public.
   The Row Level Security rules from `schema.sql` are what actually keep
   your data private, not secrecy of this key.

## 4. Connect the app
1. Open the app and click **☁ Cloud Sync**.
2. Paste in the Project URL and anon key, click **Connect**.
3. Create an account (email + password) right there in the app.
4. If you had existing data saved locally in this browser, you'll see an
   **Import local data to the cloud** button — click it once to upload
   everything you already had.

That's it. From now on:
- Anything you do is saved to your own Supabase project.
- Open the same app (same HTML file, or the same page if you host it
  somewhere) on your phone, laptop, etc., sign in with the same email, and
  everything shows up — tasks, goals, Goal Ink colors, journal, roadmap,
  backlog, and your Gemini API key/model choice.
- The Drill Sergeant AI is cross-temporal: it can see and edit your schedule
  on any date, not just the one on screen. When Cloud Sync is on, opening the
  AI panel pulls your full task history so its picture of your ledger is
  complete on every device.
- Changes made on one device stream to any other open device within a few
  seconds (via Supabase Realtime), and everything also re-syncs automatically
  every ~45 seconds as a safety net.

## Notes & good-to-knows
- **Email confirmation:** Supabase requires confirming your email by default.
  Check your inbox after signing up. You can turn this off in
  **Authentication → Providers → Email → Confirm email** if you want
  instant sign-in for a personal single-user setup.
- **Forgotten password:** the app has a "Forgot password?" link that sends a
  reset email through Supabase's built-in email service.
- **Offline behavior:** if you lose connection mid-edit, the app keeps working
  against its local cache and marks anything unsynced; it retries
  automatically once you're back online, or you can hit **Sync Now**.
- **Local Only mode:** if you skip all of this, the app works exactly as
  before, saving only to this browser's local storage. You can connect
  Supabase at any later point without losing anything already saved locally.
- **Your Gemini key:** stored in your own `user_settings` table (protected by
  the same per-user security rules), so it's still only ever visible to you,
  just synced across your own devices instead of stuck on one.
- **Hosting the file somewhere you can reach from any device:** for true
  "any device" access you'll want this HTML file hosted somewhere reachable
  by all your devices (e.g. a static host, or a private GitHub Pages page) —
  otherwise you'd need to copy the file to each device, which still works
  fine (the cloud data will be identical either way), it's just less
  convenient than one URL.
