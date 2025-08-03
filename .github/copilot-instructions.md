# Copilot Instructions for Prograde Tech Labs Hugo Website

## Project Overview
This is a Hugo-based static website for an IT consulting company. The project uses **Hugo without themes** - all templates are custom-built with inline CSS styling. The site is structured for showcasing IT services, team members, blog content, and client engagement.

## Architecture & Key Patterns

### Content Structure
- **Services**: Each service is a markdown file in `content/services/` with frontmatter including `title`, `description`, and `weight` for ordering
- **Content Types**: Uses Hugo's standard content organization (blog/, team/, services/) with custom layouts for each section
- **Frontmatter Pattern**: All content uses YAML frontmatter with consistent fields like `title`, `description`, `weight`, `draft`

### Layout System
- **Base Template**: `layouts/_default/baseof.html` contains the full HTML structure with embedded CSS (no external stylesheets)
- **Inline Styling**: All CSS is written inline within HTML templates - no separate CSS files are processed during build
- **Component Pattern**: Reusable elements like navigation and footers are embedded directly in `baseof.html`
- **SVG Icons**: Icons are embedded as inline SVG with conditional rendering based on content titles (see `layouts/services/list.html`)

### Hugo Configuration
- **No Theme**: Project doesn't use Hugo themes - `hugo.yaml` has no theme specified
- **Menu System**: Navigation configured in `hugo.yaml` under `menu.main` with weight-based ordering
- **Company Data**: All company information stored in `params` section of `hugo.yaml`
- **SEO Settings**: Canonical URLs, meta tags, and structured data handled in base template

## Development Workflow

### Local Development
```bash
# Start development server (from progade-tech-labs/ directory)
hugo server -D

# Build for production
hugo --minify
```

### Asset Processing
- **CSS**: Uses Tailwind CSS via PostCSS but currently most styling is inline
- **PostCSS Config**: Basic autoprefixer setup in `postcss.config.js`
- **No JavaScript Build**: No JS bundling - vanilla JS embedded in templates when needed

## Project-Specific Conventions

### Content Creation Pattern
```bash
# Create new service
hugo new services/service-name.md

# Create new blog post  
hugo new blog/post-title.md

# Create new team member
hugo new team/member-name.md
```

### Service Page Structure
Each service page follows this frontmatter pattern:
```yaml
---
title: "Service Name"
description: "Brief description for SEO"
weight: 4  # Controls display order
---
```

### Styling Approach
- **Inline CSS**: All styles written directly in HTML templates using `style=""` attributes
- **CSS Variables**: Defined in `assets/css/main.css` but not currently used in inline styles
- **Responsive**: Grid layouts with `grid-template-columns: repeat(auto-fit, minmax(300px, 1fr))`
- **Hover Effects**: JavaScript-style event handlers for interactive elements

### Icon System
Icons are conditionally rendered based on content title matching:
```html
{{ if eq .Title "Custom Software Development" }}
  <svg><!-- icon markup --></svg>
{{ else if eq .Title "Cloud Solutions" }}
  <svg><!-- different icon --></svg>
{{ end }}
```

## Key Files to Understand

- `hugo.yaml` - Complete site configuration including company info and navigation
- `layouts/_default/baseof.html` - Master template with full page structure
- `layouts/index.html` - Homepage with hero section and service highlights  
- `layouts/services/list.html` - Services grid with icon matching logic
- `content/services/_index.md` - Services section landing page
- `package.json` - PostCSS/Tailwind dependencies (minimal setup)

## Integration Points

### Contact Forms
Forms are static HTML ready for integration with:
- Netlify Forms (add `netlify` attribute)
- Formspree or similar services
- Custom backends

### Analytics
Google Analytics 4 configured in `hugo.yaml` under `services.googleAnalytics.ID`

### Deployment
Site builds to `public/` directory, ready for static hosting (Netlify, Vercel, GitHub Pages)

## Development Notes

- **No Hot Reload for CSS**: Since styles are inline, changes require full page refresh
- **Content Organization**: Follow Hugo's content organization principles - section directories match URL structure
- **Image Assets**: Place in `static/images/` - accessible at `/images/` in templates
- **Performance**: Inline styles mean no external CSS requests but larger HTML payload
