# Dilshan — Portfolio

A single-page portfolio site built on a Webflow export, with custom GSAP additions:

- **Scroll-scrubbed hero video** — the hero section pins in place while the
  background video plays forward on scroll-down and reverses on scroll-up.
- **Bento gallery** — a tiled image grid that flips into fullscreen rows on scroll
  (GSAP Flip + ScrollTrigger).
- **Horizontal-scroll gallery** — a pinned section where more project shots scroll
  sideways as the page scrolls down.
- **Floating "island" nav** — an expanding pill menu with orchestrated open/close
  easing (GSAP timelines with per-tween `easeReverse`).
- **Hero name reveal** — a cursor-tracked image-reveal hover effect on the hero
  heading.

Everything custom is marked in `index.html` with `<!-- ADDED: ... -->` comments,
so it's easy to find and adjust independently of the original Webflow markup.

## File structure

```
.
├── index.html                  # the whole site (single page)
├── assets/
│   └── video/
│       └── hero-video.mp4      # hero background video
└── README.md
```

> This is a static site — no build step, no dependencies to install. Open
> `index.html` directly in a browser, or serve it with any static host.

## Running locally

Because the page loads a video and fetches fonts/scripts from CDNs, some
browsers restrict `file://` pages from playing local video correctly. Serve it
over a tiny local server instead of double-clicking the file:

```bash
# Python (already on most systems)
python3 -m http.server 8000

# or, if you have Node
npx serve .
```

Then open `http://localhost:8000` in your browser.

## Hosting on GitHub Pages

### 1. Create the repository

```bash
cd portfolio-repo          # this folder
git init
git add .
git commit -m "Initial commit: portfolio site"
```

Create a new **empty** repository on GitHub (no README/license — you already
have one), then connect and push:

```bash
git branch -M main
git remote add origin https://github.com/<your-username>/<your-repo>.git
git push -u origin main
```

### 2. Turn on GitHub Pages

1. On GitHub, open your repo → **Settings** → **Pages** (left sidebar).
2. Under **Build and deployment** → **Source**, choose **Deploy from a branch**.
3. Under **Branch**, choose **main** and folder **/ (root)** → **Save**.
4. Wait 1–2 minutes. GitHub will show a green banner with your live URL:
   ```
   https://<your-username>.github.io/<your-repo>/
   ```

That's it — no build step needed since this is plain HTML/CSS/JS.

### 3. (Optional) Use a custom domain

1. In the same **Pages** settings screen, enter your domain under **Custom domain**
   (e.g. `dilshan.com`) and save. GitHub creates a `CNAME` file in your repo for you.
2. At your domain registrar, add a **CNAME record** pointing your domain (or
   subdomain, e.g. `www`) to `<your-username>.github.io`.
   - For an **apex domain** (`dilshan.com` with no `www`), instead add **A records**
     pointing to GitHub's IPs:
     ```
     185.199.108.153
     185.199.109.153
     185.199.110.153
     185.199.111.153
     ```
3. DNS changes can take anywhere from a few minutes to 24 hours to propagate.
4. Once it resolves, tick **Enforce HTTPS** back in the Pages settings.

### 4. Making updates later

Any time you change `index.html` or swap the video:

```bash
git add .
git commit -m "Describe what changed"
git push
```

GitHub Pages redeploys automatically within a minute or two of every push to
`main` — no extra steps.

## Notes

- **Video size:** `hero-video.mp4` is ~3.3MB, well under GitHub's 100MB hard
  limit (and the 50MB warning threshold), so no Git LFS needed. If you swap in a
  longer/higher-res video later and it creeps past ~50MB, consider compressing
  it (e.g. `ffmpeg -i in.mp4 -vcodec libx264 -crf 28 out.mp4`) or hosting it on
  a CDN/video host instead of in the repo.
- **Relative paths:** the video is referenced as `assets/video/hero-video.mp4`
  in `index.html`. Keep that folder structure intact wherever you host this —
  if you move the video, update the `src` on the `<video id="heroScrollVideo">`
  tag to match.
- **CDN scripts:** GSAP, ScrollTrigger, Flip, and the Webflow runtime are all
  loaded from CDNs referenced directly in `index.html` — nothing to install.
