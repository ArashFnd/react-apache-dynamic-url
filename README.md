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
```
In your site’s vhost (e.g., /etc/apache2/sites-available/clinics.kidiar.com.conf), make sure the build directory allows overrides:
```bash
<VirtualHost *:80>
  ServerAdmin webmaster@localhost
  ServerName clinics.kidiar.com
  ServerAlias www.clinics.kidiar.com
  DocumentRoot /var/www/clinics.kidiar.com
  <Directory /var/www/clinics.kidiar.com>
    Options FollowSymLinks
    AllowOverride All
    Require all granted
  </Directory>
  ErrorLog ${APACHE_LOG_DIR}/error.log
  CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```
Reload the apache:
```bash
sudo systemctl reload apache2
```
Add this `.htaccess` file to your parent directory with respect to your dynamic url:
```bash
RewriteEngine On

# IMPORTANT if your app is under a sub-path:
RewriteBase /fa/blog/

# Serve existing files/directories as-is
RewriteCond %{REQUEST_FILENAME} -f [OR]
RewriteCond %{REQUEST_FILENAME} -d
RewriteRule ^ - [L]

# Everything else goes to React
RewriteRule ^ index.html [L]
```
## Troubleshooting
- **404 on refresh / deep link**  
  Do not add `.htaccess` file to the built directory of your dynamic URL.
  For example, if your dynamic URL is: clinics.kidiar.com/fa/blog/[subject], which [subject] is the name of your title as a directory, you should add this `.htaccess file` to: clinics.kidiar.com/fa/blog
