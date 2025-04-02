---
layout: default
title: GoEcosystem Documentation
description: Organization-wide documentation and guidelines for GoEcosystem projects
---

<!-- Features Section with Cards -->
<div class="features-section">
  <h2 class="features-heading">Features</h2>
  <div class="features-list">
    <div class="feature-item">
      <div class="feature-icon">
        <i class="fas fa-book"></i>
      </div>
      <div class="feature-content">
        <h3 class="feature-title">Comprehensive Documentation</h3>
        <p class="feature-desc">Complete guides, tutorials, and reference materials for Go development.</p>
      </div>
    </div>
    
    <div class="feature-item">
      <div class="feature-icon">
        <i class="fas fa-code"></i>
      </div>
      <div class="feature-content">
        <h3 class="feature-title">Coding Standards</h3>
        <p class="feature-desc">Best practices and conventions for writing clean, maintainable Go code.</p>
      </div>
    </div>
    
    <div class="feature-item">
      <div class="feature-icon">
        <i class="fas fa-project-diagram"></i>
      </div>
      <div class="feature-content">
        <h3 class="feature-title">Project Architecture</h3>
        <p class="feature-desc">Recommended structures and patterns for organizing Go projects.</p>
      </div>
    </div>
    
    <div class="feature-item">
      <div class="feature-icon">
        <i class="fas fa-tools"></i>
      </div>
      <div class="feature-content">
        <h3 class="feature-title">Developer Tools</h3>
        <p class="feature-desc">Utilities and applications to boost productivity in Go development.</p>
      </div>
    </div>
  </div>
</div>

<!-- Documentation Cards Section -->
<div class="documentation-section">
  <h2 class="section-title">Documentation</h2>
  <div class="documentation-grid">
    <div class="documentation-card">
      <div class="documentation-icon">
        <i class="fas fa-file-code"></i>
      </div>
      <h3 class="documentation-title">Core Documentation</h3>
      <p class="documentation-desc">Essential documentation covering contribution guidelines, code of conduct, and style guide.</p>
      <ul class="documentation-links">
        <li><a href="CONTRIBUTING.html">Contributing Guidelines</a></li>
        <li><a href="CODE_OF_CONDUCT.html">Code of Conduct</a></li>
        <li><a href="STYLE_GUIDE.html">Style Guide</a></li>
        <li><a href="ARCHITECTURE.html">Architecture</a></li>
      </ul>
      <a href="#core-docs" class="view-documentation">View Documentation</a>
    </div>
    
    <div class="documentation-card">
      <div class="documentation-icon">
        <i class="fas fa-code-branch"></i>
      </div>
      <h3 class="documentation-title">Development Guides</h3>
      <p class="documentation-desc">Comprehensive guides for setting up and deploying Go projects with modern workflows.</p>
      <ul class="documentation-links">
        <li><a href="pages/github_pages_best_practices.html">GitHub Pages Best Practices</a></li>
      </ul>
      <a href="#development-guides" class="view-documentation">View Guides</a>
    </div>
  </div>
</div>

<!-- Cheatsheets Section -->
<div class="cheatsheets">
  <div class="cheatsheets-header">
    <h2 class="cheatsheets-title">Go Cheatsheets</h2>
    <a href="/cheatsheets/" class="view-all">View All <i class="fas fa-arrow-right"></i></a>
  </div>
  
  <div class="cheatsheets-grid">
    <div class="cheatsheet-card">
      <div class="cheatsheet-header">
        <h3 class="cheatsheet-title">Go Syntax Basics</h3>
      </div>
      <div class="cheatsheet-body">
        <p class="cheatsheet-desc">Essential Go syntax and language constructs.</p>
        <ul class="topic-list">
          <li><i class="fas fa-check"></i> Variables & Types</li>
          <li><i class="fas fa-check"></i> Functions & Methods</li>
          <li><i class="fas fa-check"></i> Control Structures</li>
        </ul>
      </div>
      <div class="cheatsheet-footer">
        <span class="language-label"><span class="language-dot"></span> Go 1.24.1</span>
        <a href="/cheatsheets/syntax-basics.html" class="download-link">View <i class="fas fa-arrow-right"></i></a>
      </div>
    </div>
    
    <div class="cheatsheet-card">
      <div class="cheatsheet-header">
        <h3 class="cheatsheet-title">Concurrency Patterns</h3>
      </div>
      <div class="cheatsheet-body">
        <p class="cheatsheet-desc">Goroutines, channels, and sync primitives.</p>
        <ul class="topic-list">
          <li><i class="fas fa-check"></i> Goroutines & Channels</li>
          <li><i class="fas fa-check"></i> Mutex & WaitGroups</li>
          <li><i class="fas fa-check"></i> Select Statements</li>
        </ul>
      </div>
      <div class="cheatsheet-footer">
        <span class="language-label"><span class="language-dot"></span> Go 1.24.1</span>
        <a href="/cheatsheets/concurrency.html" class="download-link">View <i class="fas fa-arrow-right"></i></a>
      </div>
    </div>
    
    <div class="cheatsheet-card">
      <div class="cheatsheet-header">
        <h3 class="cheatsheet-title">Standard Library</h3>
      </div>
      <div class="cheatsheet-body">
        <p class="cheatsheet-desc">Most commonly used packages from std lib.</p>
        <ul class="topic-list">
          <li><i class="fas fa-check"></i> io & bufio</li>
          <li><i class="fas fa-check"></i> net/http</li>
          <li><i class="fas fa-check"></i> encoding/json</li>
        </ul>
      </div>
      <div class="cheatsheet-footer">
        <span class="language-label"><span class="language-dot"></span> Go 1.24.1</span>
        <a href="/cheatsheets/stdlib.html" class="download-link">View <i class="fas fa-arrow-right"></i></a>
      </div>
    </div>
  </div>
</div>

<!-- Projects Section -->
<div class="projects-section">
  <h2 class="section-title">Projects</h2>
  <p class="section-desc">GoEcosystem includes several projects, templates, and tools.</p>
  
  <div class="projects-categories">
    <div class="project-category">
      <h3 class="category-title">Templates & Infrastructure</h3>
      <div class="project-list">
        <div class="project-item">
          <h4 class="project-title">Go Template</h4>
          <p class="project-desc">Standard Go project template with proper directory structure.</p>
        </div>
        
        <div class="project-item">
          <h4 class="project-title">Go Infra Template</h4>
          <p class="project-desc">Template for creating reusable Go libraries.</p>
        </div>
        
        <div class="project-item">
          <h4 class="project-title">Go Web Template</h4>
          <p class="project-desc">Web application template using Go, Fiber, and PostgreSQL.</p>
        </div>
      </div>
    </div>
    
    <div class="project-category">
      <h3 class="category-title">Applications & Tools</h3>
      <div class="project-list">
        <div class="project-item featured">
          <h4 class="project-title">Go Web Scraper</h4>
          <p class="project-desc">Comprehensive web scraper with database integration and web interface.</p>
          <a href="projects/web-scraper.html" class="project-link">View Documentation →</a>
        </div>
      </div>
    </div>
  </div>
</div>

<!-- Getting Started Terminal Section -->
<div class="getting-started">
  <h2>Getting Started</h2>
  <div class="getting-started-terminal">
    <div class="terminal-content">
      <div class="comment"># Clone the repository</div>
      <div class="code-line">git clone https://github.com/goecosystem/go-docs.git</div>
      <div class="code-line">cd go-docs</div>
      
      <div class="comment"># Run the example code</div>
      <div class="code-line">go run examples/hello.go</div>
      
      <div class="comment"># Build your project</div>
      <div class="code-line">go build -o myapp</div>
      
      <div class="comment"># Run your application</div>
      <div class="code-line">./myapp</div>
    </div>
  </div>
</div>

<!-- CTA Section -->
<div class="cta-section">
  <h2 class="cta-title">Ready to Start Building?</h2>
  <p class="cta-text">Explore our comprehensive documentation, examples, and resources to help you build amazing Go applications.</p>
  <div class="cta-buttons">
    <a href="/docs/getting-started/" class="btn primary-btn">
      <i class="fas fa-book"></i> Read the Docs
    </a>
    <a href="https://github.com/goecosystem" class="btn secondary-btn">
      <i class="fab fa-github"></i> View on GitHub
    </a>
  </div>
</div>

<!-- Legacy Content Below (Hidden) -->
<div style="display: none;">
# GoEcosystem Documentation

Welcome to the GoEcosystem documentation hub. This site contains comprehensive documentation and guidelines for all GoEcosystem projects.

## Core Documentation

### Contributing Guidelines
Learn how to contribute to GoEcosystem projects, including the workflow and process for submitting pull requests.
[View Guidelines →](CONTRIBUTING.html)

### Code of Conduct
Community standards and expectations for all participants in the GoEcosystem community.
[View Code of Conduct →](CODE_OF_CONDUCT.html)

### Style Guide
Coding standards and practices to maintain consistency across all GoEcosystem projects.
[View Style Guide →](STYLE_GUIDE.html)

### Architecture
Design principles and architectural patterns used in GoEcosystem projects.
[View Architecture →](ARCHITECTURE.html)

## Development Guides

### GitHub Pages Best Practices
Learn how to set up, configure, and deploy GitHub Pages for your Go projects using modern workflows, including latest GitHub Actions v4 for deployment.
[View Guide →](pages/github_pages_best_practices.html)

## Projects

GoEcosystem includes several projects:

### Templates & Infrastructure

#### Go Template
Standard Go project template with proper directory structure.

#### Go Infra Template
Template for creating reusable Go libraries.

#### Go Web Template
Web application template using Go, Fiber, and PostgreSQL.

### Applications & Tools

#### Go Web Scraper
Comprehensive web scraper with database integration and web interface.
[View Documentation →](projects/web-scraper.html)

### Documentation & Learning

#### Go Cheatsheets
Quick reference guides for Go programming.
[View Cheatsheets →](cheatsheets/)
</div>
