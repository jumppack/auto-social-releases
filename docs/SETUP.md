# Auto Social: full setup guide

The app's **Setup** screen walks you through the same steps. This page is the reference, with more detail.

## What you need

| Account / thing | Why | Cost |
|---|---|---|
| A personal **Facebook** account | To create a Page and a Meta developer app | Free |
| An **Instagram** account | It becomes a public Creator account | Free |
| A **Cloudflare** account | Your private photo storage and the daily scheduler | Free tier. Turning on R2 may ask for a card, but usage stays free |
| A **Google** account | Gemini AI writes the captions | Free tier (see the privacy note in step 6) |
| **Telegram** on your phone | Approve, edit, or skip each post | Free |
| A folder of photos | JPEG, HEIC, or PNG. Subfolders are fine | |

Plan on about **45–60 minutes** for the first setup. You only do it once.

> **Everything you create is yours.** Each person uses their own accounts, keys, and bot. Nothing
> is shared with whoever gave you this app.

---

## 1. Install the app

**macOS**: pick the download for your Mac ( → About This Mac → **Chip** or **Processor**):
- **Apple M1, M2, M3…** → `AutoSocial-…-macOS-arm64.zip` (macOS 11 or newer)
- **Intel** → `AutoSocial-…-macOS-intel.zip` (macOS 10.15 or newer)

1. Double-click the zip, then drag `Auto Social.app` into **Applications**.
2. The app isn't signed by Apple, so the first open is blocked. Open it once (it's refused), then
   go to **System Settings → Privacy & Security**, scroll down, and click **Open Anyway** next to
   "Auto Social". On macOS 14 or older, **right-click → Open → Open** also works.
3. If you used the app from source before, macOS asks whether Auto Social may use the keys it
   saved. Click **Always Allow** for each one. It asks again after installing a new version.

**Windows** (10 or 11):
1. Unzip the `Auto Social` folder anywhere, for example in Documents.
2. Double-click **`Auto Social.exe`**. If Windows shows "Windows protected your PC", click
   **More info → Run anyway**.
3. The app uses Microsoft Edge WebView2, which is already built into Windows 10/11.


---

## 2. First run: choose your photo folder

The welcome wizard opens automatically.
1. **Get started → Choose folder…** and pick the folder with the photos you want to post.
   Subfolders are included.
   - The app **only reads** this folder. It never moves, renames, or changes your originals.
2. **Scan this folder.** The app reads dates, merges identical copies, groups bursts of photos into
   carousels, and flags possible problems. Nothing is uploaded.
3. **When should posts go out?** Pick a posting time (or several, separated by commas), your timezone, and leave **"Ask me to approve
   each post"** on for now.
4. **Open library.**

### Optional: look through your library
- **Library** shows every future post, newest first. Click a post to see its details.
- Filter chips: **Carousels**, **Needs a look** (possibly blurry or near-duplicate shots), **No date**,
  **Edited by you**, **Excluded**.
- In a post's detail panel you can:
  - reorder carousel photos (◀ ▶), **Remove** a photo from a carousel, or **Split** a carousel
  - **Exclude** a post so it's never posted
- **Select to merge** combines several posts into one carousel.
- **Restore automatic grouping** undoes your grouping edits.
- Warnings are only advice. Nothing is excluded unless you exclude it.

---

> **Sections 3–6 are also in the app.** Open **Setup**: each service has numbered steps, buttons that open the right page, paste fields that catch a wrong paste, and a check before it gets a ✓.

## 3. Cloudflare (storage + scheduler) · ~10 min

Cloudflare stores your prepared photos **privately** and runs the small program (a "worker") that
posts every day.

**3a. Create the account.** Sign up at https://dash.cloudflare.com/sign-up and verify your email.
Skip any "add a website" prompts.

**3b. Claim your workers.dev address.**
1. In the left sidebar, open **Compute → Workers & Pages**.
2. If it asks you to choose a **workers.dev subdomain**, pick any name.
3. Don't create anything else. The app does that.

**3c. Turn on R2 storage.**
1. Sidebar: **Storage & databases → R2 object storage**.
2. Click the button to start: *Purchase R2 plan / Get started / Enable R2*.
3. Add a card if asked. The free allowance is 10 GB and a photo library uses about 1 GB.
4. **Don't create a bucket.** The app creates a private one.

**3d. Create one API token.**
1. Go to https://dash.cloudflare.com/profile/api-tokens and click **Create Token**.
2. At the bottom, choose **Custom token → Get started**.
3. **Token name:** `Auto Social`.
4. **Permissions:** add three rows, each **Account** → *permission* → **Edit**:
   - `Workers Scripts`
   - `Workers R2 Storage`
   - `D1`
5. **Account Resources:** Include → *your account*. Leave the rest empty.
6. **Continue to summary → Create Token**, then **copy the token now** (it's shown only once).

**3e. Find your Account ID.** It's the 32-character code in the dashboard address
(`dash.cloudflare.com/<account-id>/…`), and it's also shown on the Workers & Pages overview.

**3f. Connect it.**
1. In the app, open **Setup → Cloudflare**.
2. Paste the **Account ID** and the **API token**, then click **Save**.
3. Click **Set up Cloudflare**. In about a minute the app creates a private bucket, a database, and
   your worker, and switches on the schedule.
4. Click **Check connection**. Every line should show ✓, including *Bucket has no public address*
   and the upload, signed-link, and cleanup test.

> It only touches things named `autosocial-…`. Other workers you already have are left alone. It
> uses 1 of your account's 5 scheduled triggers.

---

## 4. Facebook Page + Instagram · ~15 min

**4a. Create a Facebook Page.**
1. While logged in to your personal account, go to https://www.facebook.com/pages/create.
2. Pick a **name** (what followers see) and a category, such as *Digital creator*, *Photographer*,
   or *Personal blog*.
3. Click **Create Page**. Inviting friends is optional.

> **Who sees Page posts?** Anyone can, because Pages are public. They don't show on your personal
> timeline, and your friends only see them if they follow the Page.

**4b. Make Instagram a Creator account** (in the Instagram app).
1. **Profile → ☰ → Account type and tools → Switch to professional account → Creator**.
2. Pick a category and finish. The account becomes **public**.

**4c. Link Instagram to the Page** (on facebook.com).
1. Switch to your Page: click your profile picture, then choose the Page.
2. **Settings & privacy → Settings → Linked accounts → Instagram → Connect account**.
3. Log in to Instagram when asked.

**4d. Create your Meta developer app.**
1. Go to https://developers.facebook.com and click **Get started** if you've never used it. You may
   need to verify a phone number.
2. **My Apps → Create app**.
   - **Name:** anything, for example `Auto Social`, plus your email.
   - **Use case:** **Other** → app type **Business**.
   - If asked about a business portfolio: **"I don't want to connect a business portfolio yet"**.
3. Open **App settings → Basic** and copy the **App ID** and the **App secret** (click *Show*).
4. In the app: **Setup → Facebook + Instagram**. Paste both and click **Save**.

> The developer app stays in "development mode". That's fine, because you're the admin of your Page
> and Instagram, so no Meta review is needed.

**4e. Allow in-app login** (one time, in your developer app).
1. In the app's dashboard sidebar, open **Facebook Login for Business → Settings**. If it isn't
   there: **Add product → Facebook Login for Business → Set up**.
2. Under **Client OAuth settings**, turn on **Client OAuth login** and **Embedded browser OAuth
   login**, then **Save changes**.
   - Leave **Valid OAuth Redirect URIs** empty.
   - A banner saying Facebook Login for Business "requires advanced access" is fine to ignore. It
     doesn't apply to you as the app's admin.

**4f. Log in and connect.**
1. In the app: **Setup → Facebook + Instagram → Log in with Facebook**.
2. A Facebook window opens. Sign in, **select your Page and your Instagram account** (click
   **Edit access** if needed), and allow everything. The window closes by itself.
   - The app turns the login into a Page token that never expires and finds your Instagram account.
   - Your Facebook password is never seen by the app.
3. Click **Check connection**. You should see 5 ✓: *never expires, all 6 permissions, your Page,
   your Instagram, publishing quota*.

<details><summary>If Log in with Facebook doesn't work: use Graph API Explorer instead</summary>

1. Open **Graph API Explorer**: https://developers.facebook.com/tools/explorer
2. On the right, set **Meta App** to *your app* and **User or Page** to **User Token**.
3. Under **Permissions**, add all six: `pages_show_list`, `pages_read_engagement`,
   `pages_manage_posts`, `instagram_basic`, `instagram_content_publish`, `instagram_manage_insights`.
4. Click **Generate Access Token**. In the Facebook window, select your Page and your Instagram
   account, and allow everything.
5. Copy the token from the **Access Token** box, paste it into **Setup → Facebook + Instagram**, and
   click **Connect with pasted token**. It isn't stored.

</details>

---

## 5. Captions: Gemini · ~3 min

1. Go to https://aistudio.google.com/apikey and click **Create API key**, then copy it.
2. In the app: **Setup → Gemini captions**. Paste it and click **Save and test**.

> **Free tier or paid?**
> - **Free tier:** captioning a whole library is free, but there's a daily limit, so a big library
>   takes a day or two. Google may use free-tier inputs, including your photos, to improve its
>   products.
> - **Paid tier:** click **Set up billing** next to the key. It costs about $0.0005–0.001 per photo,
>   and your data isn't used for training.

---

## 6. Approvals: Telegram · ~3 min

1. In Telegram, open **@BotFather** and send `/newbot`.
   - Give it a display name.
   - Give it a username ending in `bot`, for example `myname_autosocial_bot`.
2. BotFather replies with a **token** (`123456789:AA…`). Copy it.
3. In the app: **Setup → Telegram → Bot token**. Paste it and click **Save**.
4. Open your bot in Telegram (`t.me/<your bot username>`), tap **Start**, and send it "hi".
5. In the app, click **Connect Telegram**. You'll get "✅ Auto Social is connected" on your phone.
6. Under **Setup → Cloudflare**, click **Update worker**. This switches on the bot's buttons.

> The bot only answers **your** chat. Messages from anyone else are ignored.

---

## 7. Caption, review, upload

On the **Dashboard**, under **Keep it running**:
1. **Caption new photos.** It writes a caption, 1–3 hashtags, and alt text for every post, plus a
   📍 location line when the photo has GPS.
   - If the free Gemini limit is reached, it stops with "try again tomorrow". Click it again the next
     day; posts that already have captions are skipped.
2. **Optional: Review.** Edit any caption, hashtags, or place, **Regenerate** a caption, or
   **Approve** posts ahead of time. Approved posts skip the Telegram question.
3. **Upload.** It sends the prepared photos to your private bucket. It's safe to stop and run again.
4. **Sync.** It sends your settings to the worker and pulls back what's been posted.

> Captions never name or guess who's in a photo, and never invent dates, places, or events.
> GPS is removed from the uploaded copies; the location only appears as the 📍 line.

---

## 8. Choose your schedule and go live

**Settings → Posting:**
- **Posting times** and **Timezone**, for example `12:00`, `Asia/Kolkata`. For more than one post a day, list several times: `12:00, 18:00`. Each time gets its own pick and its own Telegram request.
- **Ask me to approve each post on Telegram**: recommended for the first week.
- **Dry run**: when on, everything runs except the actual publishing. Leave it on for a test day if
  you like, then turn it off to go live.

**Settings → Advanced:**
- *Ask for approval this many hours before posting*. Default 3, so 09:00 for a 12:00 post.
- *Auto-approve unanswered posts this many minutes before posting*. Default 30. Set it to 0 to hold
  unanswered posts instead.
- *Share of picks that are the newest photo*. Default 0.8.

Click **Save**, then **Dashboard → Sync**.

> **In v1, settings only reach the worker when you click Sync (or Upload).** Always Sync after
> changing Settings.

---

## Daily life

| Time (your timezone) | What happens |
|---|---|
| Posting time − 3 h (e.g. 09:00) | Telegram sends today's pick: **✅ Approve**, **✏️ Edit** (then send the new caption as a message), **⏭️ Skip** (a new pick comes right away) |
| Posting time − 30 min (e.g. 11:30) | Still unanswered? It's **auto-approved**, and you're told. Send `/pause` to stop it |
| Posting time (e.g. 12:00) | Posted to Instagram and Facebook. The photo is deleted from the cloud bucket |
| Mondays from 10:00 | A weekly stats summary (likes and comments) |

With several posting times (Settings → Posting times, e.g. `12:00, 18:00`), each time follows this
schedule on its own, and its Telegram request says which time it's for.

**How posts are picked:** about 80% of the time it's your newest unposted photo, otherwise a random
older one. Photos without a real date are only picked in the random share. **Nothing is ever posted
twice.** Each photo is tracked by a fingerprint of its contents, on your computer and in the cloud.

**Telegram commands:**

| Command | Does |
|---|---|
| `/status` | Posts and days left, posted, failed, posting times, and the current mode |
| `/pause` · `/resume` | Stop or restart posting |
| `/auto on` | Fully automatic: no more daily questions |
| `/auto off` | Back to approval mode |
| `/help` | The command list |

After a week or so, send **`/auto on`** if you're happy with the captions.

---

## Adding photos later

1. Drop new photos into your library folder.
2. **Dashboard → Scan for new photos → Caption new photos → Upload → Sync.**

New photos slot into the order by when they were taken.

---

## Troubleshooting

| Problem | Fix |
|---|---|
| No message at approval time | Send `/status`. Is it paused? Are posts left? On the Dashboard, click **Sync** |
| "❌ A post failed to publish" | It isn't retried automatically. Check **Setup → Facebook + Instagram → Check connection**. If the token is the problem, redo step 4f, then **Update worker** |
| "No Facebook Pages were shared with the app" | Redo 4f. In the Facebook window, click **Edit access** and tick your Page and Instagram. If it still fails, also add the `business_management` permission |
| "That doesn't look like a … token" when saving | The wrong thing was on your clipboard. Copy the value again |
| Caption run stopped ("try again tomorrow") | The free Gemini daily limit was reached. Click **Caption new photos** again tomorrow |
| Scan says the folder isn't available | The drive isn't connected. Nothing is marked missing until it's back |
| Save says "Not sent to the scheduler yet" | The worker couldn't be reached. Click **Dashboard → Sync** later; the change is kept |
| Changed your Facebook password | Redo step 4f, then **Update worker** |

---

## FAQ

**Does my computer need to be on?** Only for setup, captioning, and uploading. Daily posting runs in
your Cloudflare account.

**What does it cost?** Nothing on free tiers. The paid Gemini tier is optional and costs about $0.50
for a 500-photo library.

**Who can see my photos before they're posted?**
- Nobody. The bucket is private.
- Instagram and Facebook fetch each photo through a link that expires after an hour.
- Only you get the Telegram previews.

**How do I stop everything?** Send `/pause`. To remove it completely, delete `autosocial-worker`,
the `autosocial-media` bucket, and the `autosocial` D1 database in your Cloudflare dashboard.

**Can I edit a caption after uploading?** Yes. Edit it in **Review** (it updates in the cloud at
once), or use ✏️ on Telegram.

---

## Known limitations

- **A slot more than an hour late is skipped for that day.** If the worker is first set up, or a time is added, after that time has passed, it starts the next day. This stops a burst of posts or late-night requests.
- **Unsigned builds** show a warning on first launch (see [Install](#1-install-the-app)).
- **The Schedule and Analytics screens aren't built yet.** Use `/status` and the weekly Telegram summary.
- **No video,** and only Facebook Pages and Instagram.
