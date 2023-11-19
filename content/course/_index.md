---
title: "Course Summary"
description: "A summary of the course on Hugo and GitHub Pages"
cascade:
  featured_image: '/images/book.jpg'
type: page
menu: main
---

## Introduction

This course is designed to help you get started with Hugo and GitHub Pages.  
It is not a comprehensive course on Hugo or GitHub Pages, but rather a quick start guide to get you up and running.

### What is Hugo?

[Hugo](https://gohugo.io/) is a static site generator used for building websites.  
It is written in Go and is open source.

### What is GitHub Pages?

[GitHub Pages](https://pages.github.com/) is a free service offered by GitHub that allows you to host static websites on
GitHub.  
This site is hosted on GitHub Pages for example.

---

## Tutorial

### Step 0: Prerequisites

Install [Hugo](https://gohugo.io/getting-started/installing/) on your machine.

### Step 1: Create a new site

```bash
hugo new site mysite
```

### Step 2: Add a theme

```bash
git submodule add https://github.com/theNewDynamic/gohugo-theme-ananke.git
```

### Step 3: Add some content

```bash
hugo new example/my-first-page.md
```

### Step 4: Run the server

```bash
hugo server -D
```

### Step 5: Changes the settings of your repository

Go to your repository settings and change the following:

- Under **Pages**, set the **Source** to `GitHub Actions`

![GitHub Pages settings](https://gohugo.io/hosting-and-deployment/hosting-on-github/gh-pages-2.png)

### Step 6: Create a workflow to deploy to GitHub pages

```yaml
# .github/workflows/deploy.yml
# Sample workflow for building and deploying a Hugo site to GitHub Pages
name: Deploy Hugo site to Pages

on:
  # Runs on pushes targeting the default branch
  push:
    branches:
      - master

  # Allows you to run this workflow manually from the Actions tab
  workflow_dispatch:

# Sets permissions of the GITHUB_TOKEN to allow deployment to GitHub Pages
permissions:
  contents: read
  pages: write
  id-token: write

# Allow only one concurrent deployment, skipping runs queued between the run in-progress and latest queued.
# However, do NOT cancel in-progress runs as we want to allow these production deployments to complete.
concurrency:
  group: "pages"
  cancel-in-progress: false

# Default to bash
defaults:
  run:
    shell: bash

jobs:
  # Build job
  build:
    runs-on: ubuntu-latest
    env:
      HUGO_VERSION: 0.120.2
    steps:
      - name: Install Hugo CLI
        run: |
          wget -O ${{ runner.temp }}/hugo.deb https://github.com/gohugoio/hugo/releases/download/v${HUGO_VERSION}/hugo_extended_${HUGO_VERSION}_linux-amd64.deb \
          && sudo dpkg -i ${{ runner.temp }}/hugo.deb
      - name: Install Dart Sass
        run: sudo snap install dart-sass
      - name: Checkout
        uses: actions/checkout@v4
        with:
          submodules: recursive
          fetch-depth: 0
      - name: Setup Pages
        id: pages
        uses: actions/configure-pages@v3
      - name: Install Node.js dependencies
        run: "[[ -f package-lock.json || -f npm-shrinkwrap.json ]] && npm ci || true"
      - name: Build with Hugo
        env:
          # For maximum backward compatibility with Hugo modules
          HUGO_ENVIRONMENT: production
          HUGO_ENV: production
        run: |
          hugo \
            --gc \
            --minify \
            --baseURL "${{ steps.pages.outputs.base_url }}/"
      - name: Upload artifact
        uses: actions/upload-pages-artifact@v2
        with:
          path: ./public

  # Deployment job
  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    needs: build
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v2


```

### Step 7: Push to GitHub

```bash
git add .
git commit -m "Initial commit"
git push
```

### Step 8: Enable Actions

Go to the **Actions** tab of your repository and enable Actions.

![Enable Actions](https://gohugo.io/hosting-and-deployment/hosting-on-github/gh-pages-3.png)

### Step 9: View your site

Your site should now be available at `https://<username>.github.io/<repository>/`  
Congrats! 👍

---

## Conclusion

This concludes the course on Hugo and GitHub Pages.  
Hugo is a great tool for building static websites and GitHub Pages is a great way to host them for free.  
Combined, they make a great pair for building and hosting static websites.
