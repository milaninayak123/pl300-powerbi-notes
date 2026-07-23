# Setup Instructions

This folder is a ready-to-go Jekyll site using the free "Just the Docs" theme —
a clean documentation-style layout with a sidebar, search, and a landing page.
It's already built with all 22 of your PL-300 parts inside `notes/`.

## Step 1 — Create the GitHub repo

1. Go to github.com → **New repository**
2. Name it exactly: `pl300-powerbi-notes`
3. Set it to **Public** (GitHub Pages on the free plan requires public repos)
4. Don't initialize with a README (you already have one here) — just create it empty

## Step 2 — Upload these files

Easiest way if you're not comfortable with git commands:
1. On your new repo's page, click **"uploading an existing file"**
2. Drag in every file/folder from this package (`_config.yml`, `index.md`, `notes/` folder and everything inside it)
3. Commit directly to the `main` branch

(If you do use git locally: `git init`, `git add .`, `git commit -m "Initial site"`, `git remote add origin <your repo URL>`, `git push -u origin main`)

## Step 3 — Turn on GitHub Pages

1. In your repo, go to **Settings → Pages**
2. Under "Build and deployment" → Source, select **Deploy from a branch**
3. Branch: `main`, folder: `/ (root)`
4. Save

GitHub will build the site automatically — takes 1-3 minutes the first time. Your live URL will be:

**https://YOUR-USERNAME.github.io/pl300-powerbi-notes/**

## Step 4 — One find-and-replace before you're done

In `index.md`, replace `YOUR-USERNAME` in the "View on GitHub" button link with your actual GitHub username. Also update the LinkedIn/portfolio links at the bottom of that same file (currently placeholder `#` links).

## Optional — Editing later

To edit a note or fix a typo: open the file in `notes/`, edit the markdown below the `---` front matter block at the top, commit the change. The site rebuilds automatically within a couple minutes of any push.

To add a new part later: copy the front-matter pattern from any existing file in `notes/`
(`title`, `parent: Notes`, `nav_order`), give it the next number, write the content below it.
