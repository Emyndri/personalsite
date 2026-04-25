# Hosting jacobcavallo.com on GitHub Pages

A step-by-step guide to getting this site live at `https://jacobcavallo.com`.

There are two parts: (1) get the site on GitHub Pages, (2) point your Porkbun domain at it.

---

## Part 1 — Deploy to GitHub Pages

### 1. Create a GitHub account
If you don't have one yet, sign up at <https://github.com>. Pick a username you're happy with — I'll refer to it below as `YOURUSERNAME`. (`jacobcavallo` would be an obvious fit if it's available.)

### 2. Create a new repository
On GitHub, click the **+** icon (top right) → **New repository**.

- **Repository name:** `YOURUSERNAME.github.io` (this exact pattern triggers GitHub Pages' "user site" mode automatically)
- **Visibility:** Public *(required — private Pages needs GitHub Pro)*
- **Initialize with README:** leave **unchecked**
- Click **Create repository**

### 3. Upload your files
The easiest way for a non-developer workflow is **GitHub Desktop**:

1. Download from <https://desktop.github.com> and sign in with your GitHub account.
2. **File → Add local repository** → choose this `Personal Website` folder.
3. GitHub Desktop will say it's not a Git repo yet — click **create a repository**.
4. In the dialog, just hit **Create Repository** (no need to add a license/gitignore — `.gitignore` is already in the folder).
5. Click **Publish repository** (top bar).
   - Set **Name:** `YOURUSERNAME.github.io`
   - **Uncheck** "Keep this code private"
   - Click **Publish Repository**

Within a few seconds, your code is on GitHub.

> **Alternative — web upload:** If you'd rather not install GitHub Desktop, after creating the empty repo, click **uploading an existing file** on the empty repo page and drag every file/folder from `Personal Website` into the upload area. Then click **Commit changes**. (You'll need to do this every time you update — Desktop is much smoother long-term.)

### 4. Verify Pages is on
- Repo → **Settings** → **Pages** (left sidebar)
- Source should already be set to **Deploy from a branch** → `main` / `(root)`
- Within ~1 minute, the page will show **Your site is live at `https://YOURUSERNAME.github.io`**

Visit that URL — you should see your homepage. Click around: Videos, Photos, Jshapes, the Contact mailto link. Everything should work.

---

## Part 2 — Point jacobcavallo.com at it

### 1. Confirm the CNAME file is in your repo
A `CNAME` file (no extension) containing exactly the line `jacobcavallo.com` is already in this folder and was uploaded with everything else. After GitHub Pages sees it, the repo is "claimed" for that domain.

### 2. Set DNS records at Porkbun
1. Log in to <https://porkbun.com>.
2. **Domain Management** → click **DNS** next to `jacobcavallo.com`.
3. Delete any default A/AAAA records pointing at Porkbun's parking page.
4. Add the following records:

| Type  | Host     | Answer / Value                 | TTL |
|-------|----------|--------------------------------|-----|
| A     | (blank)  | 185.199.108.153                | 600 |
| A     | (blank)  | 185.199.109.153                | 600 |
| A     | (blank)  | 185.199.110.153                | 600 |
| A     | (blank)  | 185.199.111.153                | 600 |
| AAAA  | (blank)  | 2606:50c0:8000::153            | 600 |
| AAAA  | (blank)  | 2606:50c0:8001::153            | 600 |
| AAAA  | (blank)  | 2606:50c0:8002::153            | 600 |
| AAAA  | (blank)  | 2606:50c0:8003::153            | 600 |
| CNAME | www      | YOURUSERNAME.github.io.        | 600 |

> The four A records (and four AAAA records) are GitHub's official Pages IPs. The `CNAME` row makes `www.jacobcavallo.com` redirect to the apex domain.
> The trailing dot on `YOURUSERNAME.github.io.` is intentional but Porkbun usually adds it automatically — don't worry if it gets stripped.

### 3. Tell GitHub about the custom domain
- Repo → **Settings** → **Pages**
- Under **Custom domain**, enter `jacobcavallo.com` and click **Save**
- GitHub will run a DNS check. It can take anywhere from 5 minutes to a few hours for DNS to propagate.
- Once the green check appears, **enable** the **Enforce HTTPS** checkbox at the bottom. (If it's still grayed out, wait — GitHub is provisioning a free Let's Encrypt certificate. Usually ready within an hour.)

### 4. Test
- Visit `https://jacobcavallo.com` — you should see your site.
- Visit `https://www.jacobcavallo.com` — should redirect to the apex.
- Test on your phone using cellular (not Wi-Fi) to make sure DNS propagated everywhere.

---

## Updating the site later

Any time you change a file:

**Via GitHub Desktop:**
1. Save your edits in the local folder.
2. Open GitHub Desktop — you'll see the changes listed.
3. Type a short message in the **Summary** box (e.g. "Update photos page").
4. Click **Commit to main**, then **Push origin**.
5. Pages will redeploy in ~30 seconds.

---

## Troubleshooting

- **404 after enabling Pages:** Wait a minute and refresh — the first deploy can take a moment. If it persists, check Settings → Pages that the source is `main / (root)`.
- **Custom domain shows "Domain does not resolve":** DNS hasn't propagated yet. Wait 15–60 minutes and click Save again. You can check progress with `nslookup jacobcavallo.com` from a terminal.
- **HTTPS toggle is grayed out:** Certificate is still being issued. Usually ready within an hour of the DNS check passing.
- **Site loads but images are broken:** Make sure file paths in your HTML match exact filenames (case-sensitive on Pages — `Photo-1.jpg` ≠ `photo-1.jpg`).
- **CNAME file gets deleted from the repo after enabling custom domain:** GitHub re-creates it from the Pages settings, so this is normal — don't worry about it.

---

## Quick-reference summary

1. GitHub: create `YOURUSERNAME.github.io` repo, publish folder via GitHub Desktop.
2. Verify live at `https://YOURUSERNAME.github.io`.
3. Porkbun: add 4 A records + 4 AAAA records on apex, plus `www` CNAME.
4. GitHub Pages settings: enter `jacobcavallo.com` as custom domain, then enable Enforce HTTPS.
5. Done.
