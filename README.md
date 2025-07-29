# Prograde Tech Labs - Hugo Website Implementation Guide

A modern, professional static website built with Hugo for IT consulting and custom software companies.

## 🚀 Quick Start

### Prerequisites
- [Hugo Extended](https://gohugo.io/getting-started/installing/) (v0.100.0 or later)
- [Node.js](https://nodejs.org/) (v16 or later)
- [Git](https://git-scm.com/)

### Installation

1. **Create a new Hugo site:**
```bash
hugo new site prograde-tech-labs
cd prograde-tech-labs
```

2. **Initialize Git repository:**
```bash
git init
```

3. **Create the directory structure:**
```bash
mkdir -p layouts/_default layouts/blog layouts/team layouts/contact layouts/schedule
mkdir -p assets/css static/images content/blog content/team content/services
mkdir -p data archetypes
```

4. **Copy the configuration files:**
   - Copy `hugo.yaml` to the root directory
   - Copy all layout files to their respective directories in `layouts/`
   - Copy `assets/css/main.css` to `assets/css/`

5. **Install dependencies (if using PostCSS/Tailwind):**
```bash
npm init -y
npm install -D postcss postcss-cli autoprefixer @tailwindcss/forms
```

6. **Create PostCSS config (`postcss.config.js`):**
```javascript
module.exports = {
  plugins: [
    require('autoprefixer'),
  ]
}
```

7. **Start the development server:**
```bash
hugo server -D
```

Visit `http://localhost:1313` to see your website.

## 📁 Project Structure

```
prograde-tech-labs/
├── archetypes/           # Content templates
├── assets/
│   └── css/
│       └── main.css      # Main stylesheet
├── content/
│   ├── blog/             # Blog posts
│   ├── team/             # Team member pages
│   ├── services/         # Service pages
│   └── case-studies/     # Case study pages
├── data/                 # Data files
├── layouts/
│   ├── _default/
│   │   └── baseof.html   # Base template
│   ├── blog/
│   │   └── list.html     # Blog listing page
│   ├── contact/
│   │   └── single.html   # Contact page
│   ├── schedule/
│   │   └── single.html   # Scheduling page
│   ├── team/
│   │   └── single.html   # Team page
│   └── index.html        # Homepage
├── static/
│   ├── images/           # Static images
│   ├── favicon.ico       # Favicon
│   └── robots.txt        # SEO robots file
├── hugo.yaml             # Hugo configuration
└── package.json          # Node.js dependencies
```

## 🎨 Customization

### 1. Company Information
Update `hugo.yaml` with your company details:

```yaml
params:
  company_name: "Your Company Name"
  tagline: "Your Company Tagline"
  description: "Your company description"
  phone: "+1 (555) 123-4567"
  email: "hello@yourcompany.com"
  address: "Your company address"
```

### 2. Logo Integration
1. Add your logo to `static/images/logo.png`
2. Update the logo path in `hugo.yaml`:
```yaml
params:
  logo: "/images/logo.png"
```

### 3. Color Scheme
Modify the CSS variables in `assets/css/main.css`:

```css
:root {
  --color-primary: #your-primary-color;
  --color-secondary: #your-secondary-color;
  --color-accent: #your-accent-color;
}
```

### 4. Content Creation

#### Blog Posts
Create new blog posts:
```bash
hugo new blog/your-post-title.md
```

Example front matter:
```yaml
---
title: "Your Blog Post Title"
date: 2025-07-27T10:00:00-07:00
draft: false
author: "Author Name"
description: "Post description for SEO"
image: "/images/blog/post-image.jpg"
tags: ["Technology", "Software Development"]
categories: ["Industry Insights"]
featured: false
---

Your blog content here...
```

#### Team Members
Create team member pages:
```bash
hugo new team/member-name.md
```

#### Services
Create service pages:
```bash
hugo new services/service-name.md
```

## 🎯 Key Features

### Homepage
- Hero section with compelling value proposition
- Core competencies showcase
- Recent projects/case studies
- Client testimonials
- Client logos section
- Call-to-action section

### Team Page
- Executive team profiles with photo placeholders
- Professional biographies
- Expertise tags
- Social media links
- Company culture section

### Contact Page
- Comprehensive contact form
- Multiple contact methods
- Business hours
- Map integration placeholder
- FAQ section

### Schedule/CTA Page
- Multiple meeting type options
- Interactive calendar
- Time slot selection
- Meeting details form
- Confirmation system

### Blog
- Featured article highlighting
- Category filtering
- Article grid layout
- Newsletter signup
- Author information
- Reading time estimates

### Professional Features
- Responsive design (mobile-first)
- SEO optimization
- Accessibility compliance (WCAG 2.1)
- Performance optimized
- Social media integration
- Analytics ready

## 🔧 Advanced Configuration

### SEO Optimization
1. **Google Analytics:** Add your GA4 tracking ID to `hugo.yaml`
2. **Meta Tags:** Automatically generated from front matter
3. **Structured Data:** Included for better search visibility
4. **XML Sitemap:** Auto-generated by Hugo
5. **Robots.txt:** Configure in `static/robots.txt`

### Performance
- CSS/JS minification in production
- Image optimization placeholders
- Lazy loading ready
- Critical CSS inline capability

### Forms Integration
The contact and scheduling forms are ready for integration with:
- **Netlify Forms** (recommended for Netlify hosting)
- **Formspree**
- **Custom backend APIs**

To use Netlify Forms, add `netlify` attribute to forms:
```html
<form netlify>
  <!-- form fields -->
</form>
```

### Calendar Integration
The scheduling page can be integrated with:
- **Calendly**
- **Acuity Scheduling**
- **Custom booking systems**

### Newsletter Integration
Connect the newsletter signup with:
- **Mailchimp**
- **ConvertKit**
- **Custom email service**

## 🚀 Deployment

### Netlify (Recommended)
1. Connect your Git repository to Netlify
2. Build settings:
   - Build command: `hugo --minify`
   - Publish directory: `public`
   - Hugo version: `0.111.0` (or latest)

### Vercel
1. Import your Git repository
2. Framework preset: Hugo
3. Build command: `hugo --minify`
4. Output directory: `public`

### GitHub Pages
1. Use GitHub Actions for automated deployment
2. Create `.github/workflows/hugo.yml`:

```yaml
name: Deploy Hugo site to Pages

on:
  push:
    branches: ["main"]
  workflow_dispatch:

permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: "pages"
  cancel-in-progress: false

defaults:
  run:
    shell: bash

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v3
        with:
          submodules: recursive
      - name: Setup Hugo
        uses: peaceiris/actions-hugo@v2
        with:
          hugo-version: 'latest'
          extended: true
      - name: Setup Pages
        id: pages
        uses: actions/configure-pages@v3
      - name: Build with Hugo
        env:
          HUGO_ENVIRONMENT: production
          HUGO_ENV: production
        run: |
          hugo \
            --minify \
            --baseURL "${{ steps.pages.outputs.base_url }}/"
      - name: Upload artifact
        uses: actions/upload-pages-artifact@v2
        with:
          path: ./public

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

## 📊 Analytics & Monitoring

### Google Analytics 4
Add your tracking ID to `hugo.yaml`:
```yaml
googleAnalytics: "G-XXXXXXXXXX"
```

### Additional Tracking
The template supports:
- **Google Tag Manager**
- **Facebook Pixel**
- **LinkedIn Insight Tag**
- **Custom analytics solutions**

## 🔒 Security

### Content Security Policy
Add CSP headers for enhanced security:
```
Content-Security-Policy: default-src 'self'; script-src 'self' 'unsafe-inline' https://www.google-analytics.com; style-src 'self' 'unsafe-inline' https://fonts.googleapis.com; font-src 'self' https://fonts.gstatic.com;
```

### HTTPS
Always serve over HTTPS in production. Most hosting providers offer free SSL certificates.

## 🎨 Theme Customization

### Recommended Open-Source Hugo Themes
If you prefer to start with an existing theme and customize:

1. **Ananke** - Clean, modern theme
2. **Academic/Wowchemy** - Feature-rich academic/business theme
3. **Doks** - Documentation-focused theme
4. **Clarity** - Blog-focused theme

### Custom Theme Development
The provided templates can be adapted into a standalone Hugo theme:

1. Create `themes/prograde-tech/` directory
2. Move layouts, assets, and static files
3. Create `theme.toml` configuration
4. Update `hugo.yaml` to use the theme

## 🛠️ Maintenance

### Regular Updates
- Update Hugo version monthly
- Review and update dependencies
- Monitor Core Web Vitals
- Update content regularly
- Review analytics data

### Content Management
- Create editorial calendar for blog posts
- Regular team page updates
- Service offerings review
- Case studies additions
- Client testimonials collection

### Performance Monitoring
- Use Google PageSpeed Insights
- Monitor Core Web Vitals
- Check mobile responsiveness
- Test form functionality
- Validate accessibility compliance

## 📞 Support

For technical support or customization requests:
- Review Hugo documentation: https://gohugo.io/documentation/
- Check Hugo community forum: https://discourse.gohugo.io/
- Consider hiring a Hugo developer for complex customizations

## 📄 License

This template is provided as-is for educational and commercial use. Customize freely for your business needs.

---

**Built with ❤️ using Hugo, Tailwind CSS, and modern web standards.**