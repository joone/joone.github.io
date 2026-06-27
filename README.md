# Joone Blog

This blog is a static site generated with [fossbook](https://github.com/joone/fossbook),
a lightweight Node.js static blog generator for GitHub Pages.

## Prerequisites

- Node.js 18+ (20 recommended)

## Install

```bash
npm install
```

## Local development

Build and start a local preview server at http://localhost:3000:

```bash
npm start          # fossbook serve
```

Build the static site into `public/`:

```bash
npm run build      # fossbook build
```

## Writing a post

Each post is a folder under `content/posts/<slug>/` containing an `index.md`.
The folder name becomes the URL slug.

```bash
npx fossbook new "My New Post"
```

Front matter is YAML:

```markdown
---
title: "My Post Title"
date: 2026-02-17
description: "A brief summary of the post"
image: "feature.png"      # optional, a file inside the post's images/ folder
tags: "JavaScript, Node.js"
---

Your Markdown content here...
```

Post images go in `content/posts/<slug>/images/` and are referenced relatively,
e.g. `![alt](images/feature.png)`. Site-wide images live in `static/images/`
and are served from `/images/`.

## Configuration

Site settings live in [`fossbook.config.js`](fossbook.config.js).

## Deployment

Pushing to `main` triggers [`.github/workflows/deploy.yml`](.github/workflows/deploy.yml),
which builds the site and publishes it to GitHub Pages via GitHub Actions.

> In the repository settings under **Settings → Pages**, set the build and
> deployment **Source** to **GitHub Actions**.
