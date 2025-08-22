# Deploy React on Apache2 with React Router Dynamic URLs

A step-by-step guide to deploying a **React** single-page app on **Apache2**, with **React Router** dynamic URLs that work on refresh and deep links using **.htaccess** rewrite rules. Works for root domains and **subdirectory** hosting.

> Keywords: React, React Router, Apache, Apache2, .htaccess, SPA, History API fallback, rewrite, deployment, caching, compression, HTTPS, CI/CD

---

## Table of Contents
- [Demo & Repo](#demo--repo)
- [What You’ll Build](#what-youll-build)
- [Prerequisites](#prerequisites)
- [Quick Start](#quick-start)
- [Build the React App](#build-the-react-app)
  - [Vite setup](#vite-setup)
  - [CRA (Create React App) setup](#cra-create-react-app-setup)
  - [React Router config](#react-router-config)
- [Apache2 Configuration](#apache2-configuration)
  - [Enable modules](#enable-modules)
  - [Root domain hosting](#root-domain-hosting)
  - [Subdirectory hosting](#subdirectory-hosting)
  - [.htaccess (History API fallback)](#htaccess-history-api-fallback)
  - [Caching & Compression (perf)](#caching--compression-perf)
  - [HTTPS (Let’s Encrypt)](#https-lets-encrypt)
- [Deploy](#deploy)
  - [Manual rsync/SSH](#manual-rsyncssh)
  - [GitHub Actions (CI/CD)](#github-actions-cicd)
- [Troubleshooting](#troubleshooting)
- [FAQ](#faq)
- [Contributing](#contributing)
- [License](#license)

---

## Demo & Repo
- **Live site:** `https://your-domain.tld/` or `https://your-domain.tld/myapp/`
- **Repo:** `https://github.com/<your-username>/<repo-name>`

> Replace placeholders like `<repo-name>` and `your-domain.tld`.

---

## What You’ll Build
- A production build of a React SPA
- Apache2 VirtualHost or subdirectory config
- **.htaccess** rules for React Router (no 404 on refresh)
- Optional: caching, Brotli/gzip, and HTTPS
- Optional: **CI/CD** with GitHub Actions (push → build → deploy)

---

## Prerequisites
- **Node.js** 18+ and **npm** or **pnpm**
- A Linux server (e.g., Ubuntu) with **Apache2**
- SSH access (or cPanel/hosting file manager)
- GitHub repository

---

## Quick Start
```bash
# clone your repo
git clone https://github.com/<your-username>/<repo-name>.git
cd <repo-name>

# install & build (Vite example)
npm install
npm run build  # outputs dist/

# copy build to server (root-hosted example)
# NOTE: use 'build/' if you're using CRA instead of Vite.
rsync -av --delete dist/ user@server:/var/www/html/
