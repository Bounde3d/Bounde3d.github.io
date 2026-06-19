+++
date = '2025-12-12T19:08:43+08:00'
draft = false
title = 'Blogsite!'
description = "a simple and passionate personal site"

+++

How I set up Hugo + Blowfish on GitHub Pages, and ported my Obsidian notes over with one `Python` script.

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

### Setting Github Pages




## 🎨 Customizing my Theme

{{< swatches "#24273A" "#8AADF4" "#C6A0F6" >}}

For this blog, I have decided the 3 primary colors 


<!--
- considered Jekyll first (GitHub Pages native)
- moved to Hugo for speed / theme ecosystem
- why Blowfish specifically (Tailwind based, customisable, active)
-->



## Structuring content

<!--
- _index.md vs index.md (list pages vs page bundles)
- blog/ vs projects/ separation
- writeups/ nested under blog for CTF stuff
-->

## Porting my Obsidian notes

<!--
- what needed to change: frontmatter block added to top
- folder naming convention
- images: page bundles vs static folder
- anything that broke / had to fix
-->

## Customising the theme

<!--
- Catppuccin Macchiato palette to match Obsidian
- custom about page layout
- fonts: Inter / Rubik / JetBrains Mono
- terminal-style touches (kbd, prompt nav)
-->

## Deploying to GitHub Pages

<!--
- GitHub Actions workflow
- gotchas: baseURL, public/ folder, gitignore
-->

## What I'd do differently

<!--
- optional reflection section
- e.g. "should've picked Hugo from the start" or similar
-->

## Wrapping up

<!--
- link to GitHub repo of the site itself
- invite people to check out the blog/writeups
-->