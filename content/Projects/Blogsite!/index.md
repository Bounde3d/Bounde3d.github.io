+++
date = '2025-12-12T19:08:43+08:00'
draft = false
title = 'Blogsite!'
description = "a simple and passionate personal site"

+++

How I set up Hugo + Blowfish on GitHub Pages along with my own customization.
<!-- , and ported my Obsidian notes over with one `Python` script. -->

<!--more-->
{{< github repo="Bounde3d/Bounde3d.github.io" showThumbnail=false >}}

## 🤔 Why I built this

Before setting up this site, I have been using a great note-taking tool 
<span class="dotted-link">[Obsidian ↗](https://obsidian.md/)</span> to document my 
journey — CTF writeups, project notes, random tech rabbit holes — for my own 
safekeeping.

But recently I decided to showcase my progress. It's still early days, but maybe 
it can inspire others to follow along. Obsidian does offer a service to publish 
your notes, but it's a paid subscription — so instead of paying for a black box, 
I decided to host it myself on GitHub Pages.

Since Obsidian is built on Markdown for note-taking, I have decided to go with <span class="dotted-link">[Hugo ↗](https://gohugo.io/) to host 
the notes, and the theme [Blowfish ↗](https://themes.gohugo.io/themes/blowfish/)</span> for it's amazing features. 

## 🛠️ Basic Setup

### Prerequisuite

You will need the following downloaded : 
- <span class="dotted-link">[Git ↗](https://github.com/git-guides/install-git)</span><br>
   <span class="text-muted">(to test, run `git --version`)</span>
- <span class="dotted-link">[Go ↗](https://go.dev/dl/)</span><br>
   <span class="text-muted">(to test, run `go version`, must be > `1.12`)</span>
- <span class="dotted-link">[Hugo ↗](https://gohugo.io/installation/)</span><br>
   <span class="text-muted">(to test, run `hugo version`, must be > `0.158.0`)</span>

You can follow the instructions in the <span class="dotted-link">[Blowfish Installation Guide ↗](https://blowfish.page/docs/installation/#install-hugo)</span> to set up the base Hugo structure with Blowfish as the theme.

Once you are able to run `hugo server` and presented with a blank Blowfish website we can begin setting up **Github Pages**! 

### Setting up GitHub Pages

#### 1. Create the repository

On GitHub, create a new repository named **exactly** `yourusername.github.io` — 
to tell GitHub to serve the site 
at the root URL. <br>
> Set it to **public** and don't initialise it with a README.

#### 2. Add a `.gitignore`

Before your first commit, make sure Hugo's generated files don't get tracked by adding a `.gitignore` file at your root folder<br><span class="text-muted">(you can add others at your own convenience <span class="dotted-link">[gitignore ↗](https://gitignores.com/gitignores/frameworks-hugo))</span></span>:

```bash
#.gitignore file

# Hugo build output
public/
resources/
.hugo_build.lock

# OS / Editor
.vscode/
Thumbs.db
.DS_Store
```
#### 3. Link your local project to the repo

at your root folder, run :

```bash
# in git bash
git init
git add .
git commit -m "initial site"
git branch -M main
git remote add origin https://github.com/yourusername/yourusername.github.io.git
git push -u origin main
```
#### 4. Set up the GitHub Actions workflow

Since Hugo needs a build step, GitHub Pages deploys it through **GitHub Actions** 
rather than serving the repo directly. Create the workflow file at root folder:

```bash
mkdir -p .github/workflows
```

Create `.github/workflows/deploy.yml`:

```yaml
name: Deploy Hugo site to GitHub Pages

# Triggers — when this workflow runs
on:
  push:
    branches: [main]      # runs automatically on every push to main
  workflow_dispatch:       # adds a manual "Run workflow" button in the Actions tab

# Permissions the workflow needs to deploy to Pages
permissions:
  contents: read    
  pages: write      
  id-token: write   

# Prevents two deploys running at the same time —
# if you push twice quickly, the second waits instead of conflicting
concurrency:
  group: pages
  cancel-in-progress: false

jobs:
  build:
    runs-on: ubuntu-latest   # spins up a fresh Ubuntu VM for this job

    steps:
      # Step 1 — pull your repo's code onto the VM
      - uses: actions/checkout@v4
        with:
          submodules: recursive   # required — pulls in the Blowfish theme
                                  
      # Step 2 — install Hugo on the VM
      - name: Setup Hugo
        uses: peaceiris/actions-hugo@v3
        with:
          hugo-version: latest
          extended: true    # "extended" version supports Sass/SCSS,
                             # which Blowfish needs for its Tailwind styles

      # Step 3 — build the site
      - name: Build
        run: hugo --minify --baseURL "https://yourusername.github.io/"
        # --minify compresses the output HTML/CSS/JS
        # --baseURL ensures all links point to the live domain,
        # overriding whatever's set locally in your config

      # Step 4 — package the built site as a deployable artifact
      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: ./public   # Hugo's build output folder

  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}   
    runs-on: ubuntu-latest
    needs: build   # only runs after the build job succeeds

    steps:
      # Step 5 — publish the artifact to GitHub Pages
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

Then in your repo, go to **Settings → Pages → Source**, and change it from <br>*Deploy from a branch* to **GitHub Actions**.

#### 5. Push and check the build

```bash
git add .
git commit -m "add deploy workflow"
git push
```

Go to the **Actions** tab in your repo — you'll see the workflow running. 

A green 
checkmark 🟢 means it deployed successfully and your site will be live at 
`https://yourusername.github.io`!


## 🎨 Customising my Theme

>[!note] 
> This is specific to **Blowfish theme**, for more details you can check out their documentation on customization
> <span class="dotted-link">[here ↗](https://blowfish.page/docs/configuration/)</span>

### Color Schemes

{{< swatches "#24273A" "#8AADF4" "#C6A0F6" >}}

Blowfish uses a CSS variable-based colour system with three primary colour families — 
**neutral** (backgrounds and text), **primary** (main accent), and **secondary** 
(supporting accent). Each family has a scale from 50 to 900, giving the theme enough 
range to handle both light and dark mode consistently.

For my theme I went with <span class="dotted-link">[Catppuccin Macchiato ↗](https://catppuccin.com/palette)</span> — 
a soothing dark palette I already use in Obsidian, so the site and my note-taking 
environment feel consistent. The three primary colours I picked are:

- `#24273A` — **Base** (neutral dark background)
- `#8AADF4` — **Blue** (primary accent)
- `#C6A0F6` — **Mauve** (secondary accent)

To generate the full Blowfish-compatible CSS scheme file from these three hex values, 
I used <span class="dotted-link">[Fugu ↗](https://github.com/nunocoracao/fugu)</span> — 
a CLI tool made by the Blowfish author specifically for this purpose.

It prompts you for your three primary hex values and outputs a ready-to-use 
`css` file. <br>

Drop it into `assets/css/schemes/` and reference it in 
`config/_default/params.toml`:

```toml
#config/_default/params.toml
colorScheme = "macchiato" # change colorScheme value to your css file name
```

### Fonts

For typography I went with these three fonts style:

| Role | Font | Example |
|---|---|---|
| Headings | <span class="dotted-link">[Inter ↗](https://fonts.google.com/specimen/Inter)</span> | <span class="example-inter">The quick brown fox jumps over the lazy dog</span> |
| Body | <span class="dotted-link">[Rubik ↗](https://fonts.google.com/specimen/Rubik)</span> | The quick brown fox jumps over the lazy dog |
| Code | <span class="dotted-link">[JetBrains Mono ↗](https://fonts.google.com/specimen/JetBrains+Mono)</span> | <span class="example-jetbrains">The quick brown fox jumps over the lazy dog</span> |

Download the font files and place them in `static/fonts/`, then load them in 
`assets/css/custom.css`:

```css
/*custom.css*/
@font-face {
  font-family: 'Inter';
  src: url('/fonts/Inter.ttf') format('truetype');
  font-weight: 100 900;
  font-style: normal;
}

@font-face {
  font-family: 'Rubik';
  src: url('/fonts/Rubik.ttf') format('truetype');
  font-weight: 100 900;
  font-style: normal;
}

@font-face {
  font-family: 'JetBrains Mono';
  src: url('/fonts/JetBrainsMono-Regular.ttf') format('truetype');
  font-weight: 400;
  font-style: normal;
}

/* Headings set to Inter */
h1, h2, h3, h4, h5, h6 {
  font-family: 'Inter', sans-serif;
}

/* Body set to Rubik */
html, body {
  font-family: 'Rubik', sans-serif;
  font-size: 15pt;
}

/* Code blocks set to Jetbrain */
code, pre {
  font-family: 'JetBrains Mono', monospace;
  font-optical-sizing: auto;
}
```

### Logo Icon

To set the icon that appears in the browser tab, generate a `favicon set` from your 
image using <span class="dotted-link">[favicon.io ↗](https://favicon.io/favicon-converter/)</span> — 
upload your image and unzip the generated files directly in `static/`:

```bash
static/
├── favicon.ico
├── favicon-16x16.png
├── favicon-32x32.png
├── apple-touch-icon.png
├── android-chrome-192x192.png
└── android-chrome-512x512.png
```

>[!note] 
> Press <kbd>CTRL</kbd>+<kbd>SHIFT</kbd>+<kbd>R</kbd> to hard reload for the changes to happen

<!-- ## Porting Obsidian Over to Hugo  -->
