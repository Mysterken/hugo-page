---
title: "Homepage"
---

# About

This is a Hugo site deployed to a github page.  
It use the theme [ananke](https://github.com/theNewDynamic/gohugo-theme-ananke)

## Course Summary

1. Install [Hugo](https://gohugo.io/getting-started/installing/)  

2. Create a new site: {{< highlight bash >}} hugo new site mysite {{< /highlight >}}  

3. Add a theme: {{< highlight bash >}} git submodule add https://github.com/theNewDynamic/gohugo-theme-ananke.git {{< /highlight >}}  

4. Add some content: {{< highlight bash >}} hugo new example/my-first-page.md {{< /highlight >}}   

5. Run the server: {{< highlight bash >}} hugo server -D {{< /highlight >}}  

6. Create a workflow to deploy to github pages: 
{{< highlight yaml >}}
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
{{< /highlight >}}

7. Push to github and watch the magic happen

## Super cool random image below

{{< figure src="https://picsum.photos/600/300" >}}

## Send me your money NOW

{{< tweet user="Lw_Goombang" id="1724089120040104164" >}}

{{< youtube HviBn_L6quA >}}

## I am a professional coder

{{< highlight go >}} sudo rm -rf / {{< /highlight >}}
{{< gist getify 2b53198906d320abe650 >}}


