---
layout: default
title: GitHub Pages Best Practices
---

# GitHub Pages Best Practices

This document outlines best practices, common issues, and optimal workflows for setting up GitHub Pages based on our experience with the GoEcosystem organization.

## Types of GitHub Pages Sites

GitHub Pages supports three types of sites:

1. **Organization Site** (`organization-name.github.io`)
   - Repository must be named exactly `organization-name.github.io`
   - Published at `https://organization-name.github.io/`
   - Represents the entire organization

2. **Project Site** (`organization-name.github.io/repository-name`)
   - Any repository can have a GitHub Pages site
   - Published at `https://organization-name.github.io/repository-name/`
   - Useful for project documentation or demos

## Modern GitHub Pages Setup (Recommended)

We recommend using GitHub Actions for all GitHub Pages deployments. This approach provides several advantages:

- More reliable builds with dependency caching
- Better error handling and build logs
- Proper Jekyll environment setup
- No need for a separate gh-pages branch

### Step 1: Configure GitHub Actions Workflow

Create a `.github/workflows/pages.yml` file in your repository:

```yaml
name: GitHub Pages

on:
  push:
    branches:
      - main
  workflow_dispatch:

permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: "pages"
  cancel-in-progress: true

jobs:
  # Build job
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4
      
      - name: Setup Pages
        uses: actions/configure-pages@v4
      
      - name: Setup Ruby
        uses: ruby/setup-ruby@v1
        with:
          ruby-version: '3.2'
          bundler-cache: true
      
      - name: Install dependencies
        run: |
          gem install bundler
          bundle install
      
      - name: Build with Jekyll
        run: bundle exec jekyll build
        env:
          JEKYLL_ENV: production
      
      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: '_site'

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
        uses: actions/deploy-pages@v4
```

### Step 2: Create Jekyll Configuration

Create or update `_config.yml` file in your repository:

```yaml
remote_theme: pages-themes/cayman@v0.2.0
plugins:
  - jekyll-remote-theme
  - jekyll-relative-links
  - jekyll-seo-tag

title: Your Project Name
description: A concise description of your project
show_downloads: false
go_logo_enabled: true
show_hero: true

# GitHub repository info
repository: YourOrganization/your-repo

# Navigation links
nav_links:
  - title: Home
    url: https://yourorganization.github.io/
    icon: fa-home
  - title: Documentation
    url: https://yourorganization.github.io/docs/
    icon: fa-book
  - title: GitHub
    url: https://github.com/YourOrganization
    icon: fa-github

# Enable GitHub Pages to render Markdown files
relative_links:
  enabled: true
  collections: true

include:
  - README.md
  - CONTRIBUTING.md
  - LICENSE.md
```

### Step 3: Create the Gemfile

Create a `Gemfile` in your repository:

```ruby
source "https://rubygems.org"

gem "jekyll", "~> 3.9.3"
gem "github-pages", "~> 228"
gem "webrick", "~> 1.8"
gem "jekyll-remote-theme"
gem "jekyll-relative-links"
gem "jekyll-seo-tag"
```

### Step 4: Configure Repository Settings

1. Go to your repository on GitHub
2. Navigate to Settings > Pages
3. Under "Build and deployment":
   - Source: Select "GitHub Actions"
4. Save the changes

## Customizing Your Theme

### Creating a Custom Layout

Create a `_layouts/default.html` file to customize the default layout:

```html
<!DOCTYPE html>
<html lang="en-US">
  <head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&family=JetBrains+Mono:wght@400;500&display=swap" rel="stylesheet">
    <link rel="shortcut icon" href="https://go.dev/favicon.ico" type="image/x-icon">
    {% seo %}
    <link rel="stylesheet" href="{{ '/assets/css/style.css?v=' | append: site.github.build_revision | relative_url }}">
  </head>
  <body>
    <!-- Hero Section -->
    <header class="page-header" role="banner">
      <div class="header-content">
        {% if site.go_logo_enabled %}
        <svg class="go-logo" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 254.5 225" width="120" fill="#ffffff">
          <path d="M40.2 101.1c-.4 0-.5-.2-.3-.5l2.1-2.7c.2-.3.7-.5 1.1-.5h35.7c.4 0 .5.3.3.6l-1.7 2.6c-.2.3-.7.6-1 .6l-36.2-.1zM25.1 110.3c-.4 0-.5-.2-.3-.5l2.1-2.7c.2-.3.7-.5 1.1-.5h45.6c.4 0 .6.3.5.6l-.8 2.4c-.1.4-.5.6-.9.6l-47.3.1zM49.3 119.5c-.4 0-.5-.3-.3-.6l1.4-2.5c.2-.3.6-.6 1-.6h20c.4 0 .6.3.6.7l-.2 2.4c0 .4-.4.7-.7.7l-21.8-.1z"/>
          <g>
            <path d="M153.1 99.3c-6.3 1.6-10.6 2.8-16.8 4.4-1.5.4-1.6.5-2.9-1-1.5-1.7-2.6-2.8-4.7-3.8-6.3-3.1-12.4-2.2-18.1 1.5-6.8 4.4-10.3 10.9-10.2 19 .1 8 5.6 14.6 13.5 15.7 6.8.9 12.5-1.5 17-6.6.9-1.1 1.7-2.3 2.7-3.7h-19.3c-2.1 0-2.6-1.3-1.9-3 1.3-3.1 3.7-8.3 5.1-10.9.3-.6 1-1.6 2.5-1.6h36.4c-.2 2.7-.2 5.4-.6 8.1-1.1 7.2-3.8 13.8-8.2 19.6-7.2 9.5-16.6 15.4-28.5 17-9.8 1.3-18.9-.6-26.9-6.6-7.4-5.6-11.6-13-12.7-22.2-1.3-10.9 1.9-20.7 8.5-29.3 7.1-9.3 16.5-15.2 28-17.3 9.4-1.7 18.4-.6 26.5 4.9 5.3 3.5 9.1 8.3 11.6 14.1.6.9.2 1.4-1 1.7z"/>
            <path d="M186.2 154.6c-9.1-.2-17.4-2.8-24.4-8.8-5.9-5.1-9.6-11.6-10.8-19.3-1.8-11.3 1.3-21.3 8.1-30.2 7.3-9.6 16.1-14.6 28-16.7 10.2-1.8 19.8-.8 28.5 5.1 7.9 5.4 12.8 12.7 14.1 22.3 1.7 13.5-2.2 24.5-11.5 33.9-6.6 6.7-14.7 10.9-24 12.8-2.7.5-5.4.6-8 .9zm23.8-40.4c-.1-1.3-.1-2.3-.3-3.3-1.8-9.9-10.9-15.5-20.4-13.3-9.3 2.1-15.3 8-17.5 17.4-1.8 7.8 2 15.7 9.2 18.9 5.5 2.4 11 2.1 16.3-.6 7.9-4.1 12.2-10.5 12.7-19.1z"/>
          </g>
        </svg>
        {% endif %}
        <h1 class="project-name">{{ page.title | default: site.title }}</h1>
        <h2 class="project-tagline">{{ page.description | default: site.description }}</h2>
        
        <!-- Navigation buttons with icons -->
        {% if site.github.is_project_page %}
        <a href="{{ site.github.repository_url }}" class="btn">
          <i class="fab fa-github"></i> View on GitHub
        </a>
        {% endif %}
        
        {% if site.show_downloads %}
        <a href="{{ site.github.zip_url }}" class="btn">
          <i class="fas fa-download"></i> Download .zip
        </a>
        <a href="{{ site.github.tar_url }}" class="btn">
          <i class="fas fa-download"></i> Download .tar.gz
        </a>
        {% endif %}
        
        {% for link in site.nav_links %}
        <a href="{{ link.url }}" class="btn">
          <i class="fas {{ link.icon }}"></i> {{ link.title }}
        </a>
        {% endfor %}
      </div>
    </header>

    <main id="content" class="main-content" role="main">
      {{ content }}

      <footer class="site-footer">
        {% if site.github.is_project_page %}
        <span class="site-footer-owner">
          <a href="{{ site.github.repository_url }}">{{ site.github.repository_name }}</a> is maintained by 
          <a href="{{ site.github.owner_url }}">{{ site.github.owner_name }}</a>.
        </span>
        {% endif %}
        
        <div class="footer-links">
          {% for link in site.nav_links %}
          <a href="{{ link.url }}"><i class="fas {{ link.icon }}"></i> {{ link.title }}</a>
          {% if forloop.last == false %} | {% endif %}
          {% endfor %}
        </div>
        
        <p>Copyright &copy; 2025 Your Organization. All rights reserved.</p>
      </footer>
    </main>
  </body>
</html>
```

### Adding Custom CSS

Create an `assets/css/style.scss` file:

```scss
---
---

@import "{{ site.theme }}";

:root {
  --primary-color: #00ADD8; /* Go blue */
  --secondary-color: #5DC9E2;
  --dark-color: #0D1117;
  --light-color: #F8F9FA;
  --code-bg: #1F2937;
  --code-color: #E5E7EB;
}

/* Override theme with our modern Go-branded styling */
body {
  font-family: 'Inter', -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif;
  color: var(--dark-color);
  line-height: 1.6;
}

.page-header {
  background: linear-gradient(135deg, #00ADD8 0%, #5DC9E2 100%);
  padding: 4rem 2rem;
  position: relative;
  overflow: hidden;
}

.page-header::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: url('data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSI1MDAiIGhlaWdodD0iNTAwIj48ZmlsdGVyIGlkPSJub2lzZSIgeD0iMCIgeT0iMCIgd2lkdGg9IjEwMCUiIGhlaWdodD0iMTAwJSI+PGZlVHVyYnVsZW5jZSB0eXBlPSJmcmFjdGFsTm9pc2UiIGJhc2VGcmVxdWVuY3k9IjAuNjUiIG51bU9jdGF2ZXM9IjMiIHN0aXRjaFRpbGVzPSJzdGl0Y2giIHJlc3VsdD0ibm9pc2UiLz48ZmVTcGVjdWxhckxpZ2h0aW5nIGluPSJub2lzZSIgc3VyZmFjZVNjYWxlPSIxMCIgc3BlY3VsYXJDb25zdGFudD0iMC4xIiBzcGVjdWxhckV4cG9uZW50PSIyMCIgbGlnaHRpbmdDb2xvcj0iI2ZmZmZmZiIgcmVzdWx0PSJzcGVjdWxhciIvPjxmZUNvbXBvc2l0ZSBpbj0ibm9pc2UiIGluMj0ic3BlY3VsYXIiIG9wZXJhdG9yPSJhcml0aG1ldGljIiBrMT0iMCIgazI9IjEiIGszPSIxIiBrND0iMCIvPjwvZmlsdGVyPjxyZWN0IHdpZHRoPSI1MDAiIGhlaWdodD0iNTAwIiBmaWxsPSJ0cmFuc3BhcmVudCIgZmlsdGVyPSJ1cmwoI25vaXNlKSIgb3BhY2l0eT0iMC4xNSIvPjwvc3ZnPg==');
  opacity: 0.3;
  z-index: 0;
}

.header-content {
  position: relative;
  z-index: 1;
  text-align: center;
}

.go-logo {
  height: 80px;
  margin-bottom: 1.5rem;
}

/* Additional CSS styling... (the rest of your CSS) */
```

## Troubleshooting Common Issues

### Build Failures

If your GitHub Actions build is failing, check:

1. **Gemfile Configuration**: Make sure your Gemfile includes all necessary dependencies:
   - `github-pages` gem for GitHub Pages compatibility
   - `jekyll-remote-theme` if using a remote theme
   - `webrick` for Ruby 3.0+ compatibility

2. **Jekyll Configuration**: Check your `_config.yml` for errors:
   - Proper YAML formatting
   - Compatible plugin declarations
   - Valid theme specification

3. **File Structure**: Ensure your repository follows Jekyll conventions:
   - Pages in the root directory or in organized folders
   - Front matter on all content pages

### Theme Not Applying

If your theme isn't applying correctly:

1. **Check CSS Loading**: Inspect your page and check the Network tab to confirm style.css is loading
2. **Verify Front Matter**: Ensure all pages have proper front matter with layout declaration
3. **Theme Configuration**: Confirm your `_config.yml` has the correct theme specified

### Custom Domain Issues

If using a custom domain:

1. **DNS Configuration**: Ensure your DNS records are set up correctly:
   - A record pointing to GitHub Pages IP addresses
   - CNAME record if using a subdomain

2. **CNAME File**: Verify your repository has a CNAME file with your domain

## Best Practices

### Content Organization

- Use a clear and consistent directory structure
- Keep related files together in logical folders
- Include a descriptive README.md at each directory level

### Markdown Guidelines

- Use proper heading hierarchy (h1 → h2 → h3)
- Include alt text for images
- Use relative links to other repository files

### Versioning Documentation

For projects with multiple versions:

1. Use branches or tags for major versions
2. Consider implementing a version switcher in your theme
3. Clearly mark the version of each documentation page

## Conclusion

By following these guidelines, you'll create a GitHub Pages site that is reliable, maintainable, and provides a great user experience. The modern GitHub Actions approach gives you more flexibility and better performance than the legacy branch-based deployment method.

Remember to periodically update your dependencies and workflows as GitHub releases new versions of the GitHub Pages and Actions features.
