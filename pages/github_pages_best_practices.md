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

3. **User Site** (`username.github.io`)
   - Similar to organization sites but for individual users
   - Repository must be named exactly `username.github.io`
   - Published at `https://username.github.io/`

## GitHub Pages Deployment Options

GitHub Pages offers two primary publishing methods:

### 1. Deploy from a Branch

```yaml
# In repository settings:
# Source: Deploy from a branch
# Branch: main (or gh-pages)
# Folder: / (root) or /docs
```

- **Pros**: Simple setup, automatic deployment when pushing to the branch
- **Cons**: Limited control over the build process
- **Best for**: Simple sites, static HTML, basic Jekyll sites
- **Important**: If using Jekyll, GitHub will automatically build the site

### 2. Deploy with GitHub Actions

```yaml
# In repository settings:
# Source: GitHub Actions
```

- **Pros**: Complete control over the build process, supports complex builds
- **Cons**: Requires more configuration
- **Best for**: Complex sites, custom build requirements, non-Jekyll static site generators

## Optimal GitHub Actions Workflow for GitHub Pages

Based on our experience, this is the recommended GitHub Actions workflow for GitHub Pages (as of 2025):

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

# Allow only one concurrent deployment
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
      
      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: '.'  # For project root, or './docs' for docs directory
  
  # Deployment job
  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    needs: build  # Important: ensure this runs after the build job
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

**Key points**:
- Use `v4` of the actions (latest stable as of 2025)
- Separate build and deploy into distinct jobs
- Specify `needs: build` to ensure proper job order
- Set `cancel-in-progress: true` to avoid wasting resources
- Include proper permissions for GitHub Pages deployment
- Set up a proper deployment environment with URL output

## Common Issues and Solutions

### 1. 404 Error: "There isn't a GitHub Pages site here"

**Causes**:
- GitHub Pages not enabled in repository settings
- Incorrect branch configuration
- GitHub Actions workflow hasn't completed successfully

**Solutions**:
- Manually enable GitHub Pages in repository settings first
- For Actions workflow, select "GitHub Actions" as the source
- For branch deployment, select the appropriate branch (main, gh-pages, or main/docs)
- Create a gh-pages branch if using that approach: `git checkout -b gh-pages && git push -u origin gh-pages`

### 2. GitHub Actions Workflow Failure: "Missing download info"

**Causes**:
- Outdated or deprecated GitHub Actions versions
- Dependency conflicts between actions

**Solutions**:
- Use stable versions of GitHub Actions (v4 is recommended as of 2025)
- Include necessary setup steps for dependencies
- Use specific release tags rather than branch references

### 3. Jekyll Liquid Template Errors

**Causes**:
- Code examples in markdown files using double curly braces `{{...}}` which Jekyll interprets as Liquid syntax
- Special characters that Jekyll tries to process

**Solutions**:
- Wrap code blocks in Jekyll raw tags:
  ```
  {% raw %}
  // Your code with {{ curly braces }}
  {% endraw %}
  ```
- Add `.nojekyll` file to bypass Jekyll processing entirely
- Use proper Jekyll configuration for code blocks

### 4. Jekyll Build Configuration Issues

**Causes**:
- Missing or incorrect `_config.yml`
- Theme compatibility issues
- Plugin dependencies

**Solutions**:
- Use a simple theme configuration:
  ```yaml
  theme: jekyll-theme-cayman
  title: Your Site Title
  description: Your site description
  ```
- For custom themes, explicitly include required plugins
- Keep Jekyll configuration minimal for GitHub Pages compatibility

### 5. Failed to Create Deployment Error

**Causes**:
- GitHub Pages not properly enabled for GitHub Actions
- Missing permissions in workflow file
- Missing environment configuration

**Solutions**:
- Ensure GitHub Pages is set to use GitHub Actions in repository settings
- Include all required permissions: `contents: read`, `pages: write`, `id-token: write`
- Add proper environment configuration with `name: github-pages`
- Make sure build and deploy are separate jobs with `needs:` parameter

## Recommended Workflow for Setting Up GitHub Pages

Based on our experience with GoEcosystem, here's the recommended workflow:

### For Organization Sites:

1. Create a repository named `organization-name.github.io`
2. Initialize with a simple `index.html` or Jekyll site
3. Enable GitHub Pages in repository settings
4. Set up GitHub Actions workflow for deployment
5. Test and verify the site is accessible

### For Project Sites:

1. Add a `.github/workflows/pages.yml` file with the workflow configuration
2. Add proper Jekyll configuration if using Jekyll
3. Manually enable GitHub Pages in repository settings, selecting "GitHub Actions" as source
4. If Jekyll build fails, troubleshoot by examining workflow logs
5. For markdown-heavy sites, consider using raw tags for code blocks

### For Sites with Complex Code Examples:

1. Use Jekyll's raw tags to prevent template processing:
   ```
   {% raw %}
   // Code with {{curly}} braces
   {% endraw %}
   ```
2. Consider adding a `.nojekyll` file if not using Jekyll features
3. Use direct links to GitHub repository files for complex code examples

## Lessons Learned from GoEcosystem Setup

1. **Always Check Repository Settings First**: GitHub Pages needs to be explicitly enabled in settings
2. **GitHub Actions Version Stability**: Use stable, well-tested GitHub Actions versions (v4 as of 2025)
3. **Separate Build and Deploy Jobs**: Always use a two-job approach with proper needs parameter
4. **Jekyll Configuration**: Keep it simple and avoid complex theme configurations
5. **Code Blocks in Markdown**: Always use raw tags for code with template-like syntax
6. **Branch Strategy**: For simplicity, a `gh-pages` branch is often the most reliable approach
7. **Permissions**: Ensure proper permissions are set in GitHub Actions workflows
8. **Concurrency**: Set up proper concurrency to avoid wasting Actions minutes

By following these best practices, you can set up reliable GitHub Pages for both organization and project sites, avoiding common pitfalls and ensuring smooth deployment.
