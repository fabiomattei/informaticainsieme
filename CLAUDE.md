# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

**Informatica Insieme** is a Jekyll-based educational website (https://www.informaticainsieme.it) that provides personal notes and tutorials on computer science topics in Italian. The site does not use cookies or tracking.

## Build & Development Commands

### Prerequisites
- Ruby with Bundler
- Node.js (for simple-jekyll-search)

### Common Commands

```bash
# Install dependencies
bundle install
npm install

# Run the development server (live at http://localhost:4000)
bundle exec jekyll serve

# Build for production
bundle exec jekyll build

# Clean build artifacts
bundle exec jekyll clean
```

## Architecture & Structure

### Jekyll Static Site Generator
The site uses Jekyll 4.3.3 with the Minima theme. It generates static HTML from Markdown content and Liquid templates.

### Content Organization: Collections
All educational content is organized into 17 Jekyll collections (defined in `_config.yml`), each with its own URL prefix:

- **Programming Languages**: `_python` (22 files), `_ruby` (6 files), `_cpp` (2 files)
- **Algorithms & Problem Solving**: `_algoritmi` (15 files), `_problemsolving` (12 files)
- **Data & Databases**: `_database` (5 files), `_pandas` (1 file), `_codifica` (5 files)
- **Web Development**: `_web` (10 files), `_rete` (8 files)
- **Machine Learning & AI**: `_ia` (19 files), `_pythonmath` (2 files)
- **Specialized Topics**: `_flask` (8 files), `_videogames` (11 files), `_arduino` (1 file), `_olimpiadiinformatica` (3 files), `_storia` (1 file)

Total: ~152 educational pages across all collections.

### Directory Layout

```
informaticainsieme/
├── _config.yml              # Jekyll configuration, collection definitions, site metadata
├── _includes/               # Reusable HTML components (header, footer, analytics, social)
├── _layouts/                # Template files (base, home, page, post)
├── _plugins/                # Custom Ruby filters (italian_dates.rb for Italian date formatting)
├── _sass/                   # SCSS stylesheets
├── _posts/                  # Blog posts (organized in subdirectories by year/month)
├── _[collection]/           # Content directories for each collection (e.g., _python, _ruby, _ia)
├── paginemenu/              # Menu pages (index/landing pages for collections)
├── script/                  # Utility scripts (dateloader.js, getdate.php)
├── dati/                    # Data files
├── images/                  # Static image assets
├── Gemfile                  # Ruby dependencies (Jekyll, minima, jekyll-feed)
├── package.json             # Node dependencies (simple-jekyll-search)
└── search.json              # Search index data
```

### Content Front Matter Conventions

All content files use YAML front matter with these standard fields:

```yaml
---
title: "Page or Section Title"
date: 'YYYY-MM-DDTHH:MM:SS+TZ'
author: "Author Name"
layout: page  # or 'post' for blog entries
---
```

Optional fields:
- `excerpt`: Summary text for listings
- `tags`: Categorization tags
- `modified_date`: Last updated timestamp

### Custom Jekyll Filters

**File**: `_plugins/italian_dates.rb`

Registers two Liquid filters for date formatting:
- `italianDate`: Outputs dates in Italian format (e.g., "17 maggio 2026")
- `html5date`: Outputs dates in HTML5 format (YYYY-MM-DD)

Usage in templates: `{{ page.date | italianDate }}`

### Template Structure

- **`_layouts/base.html`**: Root wrapper for all pages
- **`_layouts/home.html`**: Homepage with post listings
- **`_layouts/page.html`**: Standard collection/tutorial pages
- **`_layouts/post.html`**: Blog post layout with author/date metadata
- **`_includes/header.html`**: Navigation and site header
- **`_includes/footer.html`**: Footer with social links (GitHub, Mastodon)
- **`_includes/google-analytics.html`**: Analytics integration
- **`_includes/disqus_comments.html`**: Comment system (conditionally included)

### Content Rendering

The site uses syntax highlighting via Jekyll's built-in Liquid `highlight` tags:

```liquid
{% highlight python %}
print("Hello World")
{% endhighlight %}
```

All content is rendered as static HTML at build time. No server-side processing occurs.

### Search Functionality

`simple-jekyll-search` (npm package) provides client-side search. The `search.json` file is auto-generated during build to index all content for fast JavaScript-based search in the browser.

### Site Configuration

Key settings in `_config.yml`:
- **Title**: "Informatica insieme"
- **Language**: Italian (locale: it_IT)
- **URL**: https://www.informaticainsieme.it
- **Theme**: Minima (~2.5)
- **Owner**: Fabio Mattei (burattino@gmail.com, GitHub: @fabiomattei)
- **Social**: Mastodon profile linked (mastodon.uno/@fabiomattei)

## Development Patterns & Conventions

### Styling

Custom SCSS lives in `_sass/minima.scss` on top of the Minima theme defaults.

### Build Artifacts

`_site/` is generated at build time and is excluded from Git.

