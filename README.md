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
- **Live site:** `[https://clinics.kidiar.com//](https://clinics.kidiar.com/fa/blog/%D8%AA%D8%B4%DA%A9%DB%8C%D9%84-%D8%AD%D8%A7%D9%81%D8%B8%D9%87-%D9%86%D9%88%D8%B2%D8%A7%D8%AF-%D8%B2%D9%88%D8%AF%D8%AA%D8%B1-%D8%A7%D8%B2-%D8%A2%D9%86-%DA%86%D9%87-%D8%AA%D8%B5%D9%88%D8%B1-%D9%85%DB%8C-%D8%B4%D8%AF-%D8%B4%D8%B1%D9%88%D8%B9-%D9%85%DB%8C-%D8%B4%D9%88%D8%AF)`
- **Repo:** `https://github.com/ArashFnd/react-apache-dynamic-url`

> The live site is my own website built based on this tutorial.

---

## What You’ll Build
- A production build of a React SPA
- Apache2 VirtualHost or subdirectory config
- **.htaccess** rules for React Router (no 404 on refresh)

---

## Prerequisites
- **Node.js** 18+ and **npm**
- A Linux server (e.g., Ubuntu) with **Apache2**
- SSH access

---

## Quick Start
With apache installed and enabled on your ubuntu server, run below command:
```bash
sudo a2enmod rewrite
bash```
In your site’s vhost (e.g., /etc/apache2/sites-available/000-default.conf), make sure the build directory allows overrides:
```bash
<VirtualHost *:80>
  DocumentRoot /var/www/your-app/build

  <Directory /var/www/your-app/build>
    Options FollowSymLinks
    AllowOverride All
    Require all granted
  </Directory>
</VirtualHost>
