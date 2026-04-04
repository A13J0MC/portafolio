# Alejandro Mendoza — Data Engineer Portfolio

Personal portfolio built with [Jekyll](https://jekyllrb.com/) and hosted on GitHub Pages.

**Live site:** https://a13j0mc.github.io/portafolio/

---

## Local Development

### Prerequisites

- Ruby 3.x
- Bundler (`gem install bundler`)

### Run locally

```bash
bundle install
bundle exec jekyll serve
```

Open http://localhost:4000/portafolio/ in your browser.
Changes to files are picked up automatically (except `_config.yml`, which requires a server restart).

---

## How to Update the Portfolio

### Personal info, skills, and social links

Edit [_config.yml](_config.yml). This file controls:

- `author` — name, email, GitHub handle, LinkedIn, location
- `skills` — lists rendered in the skills grid on the home page
- `tagline` — the phrase shown in the footer

```yaml
author:
  name: "Alejandro Mendoza"
  email: "your.email@example.com"
  github: "A13J0MC"
  linkedin: "alejandro-mendoza"

skills:
  languages:
    - Python
    - SQL
    - ...
```

### Bio, experience, and education

Edit [about.md](about.md) — the full About page with bio paragraphs, experience details, education, and certifications.

### Experience timeline on the home page

Edit the `<!-- EXPERIENCE PREVIEW -->` section in [index.html](index.html). Each job is a `timeline-item` block:

```html
<div class="timeline-item">
  <div class="timeline-header">
    <div>
      <div class="timeline-title">Job Title</div>
      <div class="timeline-company">Company Name</div>
    </div>
    <div class="timeline-period">2023 — Present</div>
  </div>
  <div class="timeline-location">📍 City (Remote)</div>
  <div class="timeline-body">
    <ul>
      <li>What you did.</li>
    </ul>
    <div class="timeline-tags">
      <span class="tag">Python</span>
    </div>
  </div>
</div>
```

### Adding a new project

Create a new `.md` file inside `_projects/`. The filename becomes the URL slug.

```bash
# Example
touch _projects/my-pipeline-project.md
```

Paste this frontmatter template and fill it in:

```yaml
---
title: "Project Title"
description: "One-sentence description shown on the project card."
icon: "🔧"              # emoji shown on the card
category: "Batch Processing"   # label shown on the detail page
status: completed       # completed | wip
featured: true          # true = shown on home page (keep to 3 max)
tech_stack:
  - Python
  - Apache Spark
  - Airflow
github: "https://github.com/A13J0MC/your-repo"
demo: ""                # optional live demo URL
---

## Overview

Describe the project here in Markdown...
```

The body of the file (below the `---`) supports full Markdown including code blocks.

### Removing a project

Delete the corresponding file from `_projects/`. The card disappears automatically on the next deploy.

---

## Redeployment

Every `git push` to the `main` branch triggers an automatic rebuild and deploy via GitHub Actions. No manual steps needed.

```bash
# Typical update workflow
git add .
git commit -m "Add project: My New Pipeline"
git push
```

The deploy usually completes in **under 2 minutes**. You can watch it live at:

https://github.com/A13J0MC/portafolio/actions

### Trigger a manual deploy (without a code change)

```bash
gh workflow run deploy.yml --repo A13J0MC/portafolio
```

---

## Project Structure

```
portafolio/
├── _config.yml              # Site settings, author info, skills
├── index.html               # Home page (hero, skills, experience, featured projects)
├── about.md                 # Full about/resume page
├── projects.md              # All projects listing page
├── _projects/               # One .md file per project
│   ├── etl-pipeline-spark.md
│   ├── streaming-kafka-flink.md
│   └── dbt-data-warehouse.md
├── _layouts/
│   ├── default.html         # Base HTML shell (head, header, footer)
│   ├── home.html
│   └── project.html         # Individual project detail page
├── _includes/
│   ├── header.html          # Sticky nav
│   └── footer.html          # Social links
├── assets/css/main.scss     # All styles (dark theme)
└── .github/workflows/
    └── deploy.yml           # GitHub Actions build & deploy pipeline
```

---

## Where to See It

| Environment | URL |
|-------------|-----|
| Production  | https://a13j0mc.github.io/portafolio/ |
| Local dev   | http://localhost:4000/portafolio/ |
| GitHub repo | https://github.com/A13J0MC/portafolio |
| Actions log | https://github.com/A13J0MC/portafolio/actions |
