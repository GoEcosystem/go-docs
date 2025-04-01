---
layout: container
title: GitHub Pages Best Practices
description: Modern approaches to GitHub Pages using GitHub Actions and jekyll
---

<div class="toc">
    <h3 class="toc-title"><i class="fas fa-list"></i> Table of Contents</h3>
    <ul class="toc-list">
        <li><a href="#introduction">Introduction</a></li>
        <li><a href="#what-is-github-pages">What is GitHub Pages</a></li>
        <li><a href="#setting-up-github-pages">Setting Up GitHub Pages</a></li>
        <li><a href="#github-actions-for-deployment">GitHub Actions for Deployment</a></li>
        <li><a href="#theme-customization">Theme Customization</a></li>
        <li><a href="#adding-a-hero-section">Adding a Hero Section</a></li>
        <li><a href="#troubleshooting">Troubleshooting</a></li>
        <li><a href="#conclusion">Conclusion</a></li>
    </ul>
</div>

# GitHub Pages Best Practices

## Introduction

GitHub Pages offers a simple way to host documentation, project pages, and personal sites directly from a GitHub repository. This guide provides modern best practices for setting up, configuring, and deploying GitHub Pages sites using GitHub Actions and Jekyll.

## What is GitHub Pages

GitHub Pages is a static site hosting service that takes HTML, CSS, and JavaScript files directly from a repository on GitHub, optionally runs the files through a build process, and publishes a website.

<div class="feature-list">
  <ul>
    <li><i class="fas fa-check-circle"></i> <strong>Free hosting</strong> for public and private repositories</li>
    <li><i class="fas fa-check-circle"></i> <strong>Custom domains</strong> with HTTPS support</li>
    <li><i class="fas fa-check-circle"></i> <strong>Jekyll integration</strong> for static site generation</li>
    <li><i class="fas fa-check-circle"></i> <strong>GitHub Actions support</strong> for custom build workflows</li>
  </ul>
</div>

## Setting Up GitHub Pages

### GitHub Actions Deployment Method (Recommended)

The recommended approach for deploying GitHub Pages sites is using GitHub Actions workflows, which offers more flexibility and control.

1. Create a new repository or use an existing one
2. Navigate to Settings > Pages
3. Under "Build and deployment", select "GitHub Actions" as the source
4. GitHub will suggest workflow templates, or you can create a custom one

### Jekyll Configuration

For Jekyll sites, create a `_config.yml` file in the root of your repository:

```yaml
# Site settings
title: Your Project Title
description: A short description of your project
show_downloads: true # Set to false to hide download buttons

# Theme settings
remote_theme: pages-themes/cayman@v0.2.0 # Or another supported theme
plugins:
  - jekyll-remote-theme
  - jekyll-relative-links # Converts relative links to markdown files
  - jekyll-seo-tag # For better SEO
```

### Project Structure

A typical structure for a GitHub Pages site with Jekyll:

```
├── .github/
│   └── workflows/
│       └── pages.yml
├── _layouts/
│   ├── default.html
│   └── container.html
├── assets/
│   └── css/
│       └── styles.scss
├── _config.yml
├── Gemfile
├── README.md
└── index.md
```

## GitHub Actions for Deployment

Using GitHub Actions for deployment is the modern approach that provides more flexibility and control over the build process. Create a `.github/workflows/pages.yml` file:

```yaml
name: GitHub Pages

on:
  push:
    branches:
      - main
  workflow_dispatch:

# Sets permissions of the GITHUB_TOKEN to allow deployment to GitHub Pages
permissions:
  contents: read
  pages: write
  id-token: write

# Allow one concurrent deployment
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
        id: pages
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

## Theme Customization

### Creating a Custom Theme

1. Create a `_layouts/default.html` file to override the theme's default layout
2. Create an `assets/css/styles.scss` file for custom styling (note that Jekyll requires empty front matter at the top):

```scss
---
---
@charset "utf-8";

// Your custom styles here
$primary: #00ADD8;  // Example Go color
$accent: #00FF41;   // Example accent color

body {
  font-family: 'Inter', sans-serif;
  // More custom styles
}
```

### Modifying Navigation

Add a navigation bar by updating your layout file:

```html
<div class="nav-links">
  <div class="container">
    {% for link in site.nav_links %}
    <a href="{{ link.url }}"><i class="fas {{ link.icon }}"></i> {{ link.title }}</a>
    {% endfor %}
  </div>
</div>
```

And in your `_config.yml`:

```yaml
# Navigation links
nav_links:
  - title: Home
    url: /
    icon: fa-home
  - title: Documentation
    url: /docs/
    icon: fa-book
```

## Adding a Hero Section

A hero section can dramatically improve the appearance of your documentation site:

```html
<header class="page-header" role="banner">
  <div class="header-content">
    <svg class="project-logo" width="120" height="120">
      <!-- Your logo SVG here -->
    </svg>
    <h1 class="project-name">{{ page.title | default: site.title }}</h1>
    <h2 class="project-tagline">{{ page.description | default: site.description }}</h2>
    
    <div class="header-buttons">
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
    </div>
  </div>
</header>
```

## Using Card Components

Cards are a great way to present features or sections:

```html
<div class="row">
  <div class="col-md-6">
    <div class="card">
      <div class="card-title">
        <i class="fas fa-code-branch"></i> Feature Title
      </div>
      <p>Feature description goes here.</p>
      <a href="#">Learn more <i class="fas fa-arrow-right"></i></a>
    </div>
  </div>
</div>
```

## Troubleshooting

### Common Issues and Solutions

- **CSS not loading**: Ensure paths in your SCSS files are correct
- **GitHub Actions failing**: Check the workflow YAML syntax and permissions
- **Jekyll build errors**: Verify your Gemfile has the right dependencies
- **Custom domain issues**: Ensure your DNS settings are correctly configured

### Debugging Tips

1. Check the Actions tab for build logs
2. Validate your YAML files with a linter
3. Test Jekyll builds locally before pushing
4. Use the GitHub Pages health check in repository settings

## Conclusion

By following these best practices, you can create modern, beautiful documentation sites for your Go projects using GitHub Pages. The combination of Jekyll for content management, GitHub Actions for deployment, and custom styling with modern UI components ensures your documentation is both functional and visually appealing.

Remember to keep your documentation up-to-date as your project evolves and consider soliciting feedback from users to continuously improve the experience.

---

<div class="next-steps">
  <h3><i class="fas fa-arrow-right"></i> Next Steps</h3>
  <p>Now that you've learned about GitHub Pages best practices, you might want to explore:</p>
  <ul>
    <li><a href="#">Advanced Jekyll techniques</a></li>
    <li><a href="#">Custom domain configuration</a></li>
    <li><a href="#">Integrating documentation into your workflow</a></li>
  </ul>
</div>
