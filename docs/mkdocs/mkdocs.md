# MkDocs

MkDocs is a static site generator written in Python. You write content as plain markdown files, run `mkdocs build`, and it produces a complete HTML/CSS/JS website. No databases, no backend.

## Project Structure

The entire project structure is essentially two things:

```plain
my-site/
├── mkdocs.yml          # config file
└── docs/               # your content
    ├── index.md
    ├── automation/
    │   ├── cucumber-junit5.md
    │   └── allure-report.md
    └── build-tools/
        ├── maven.md
        └── gradle.md
```

## `mkdocs.yml`

`mkdocs.yml` is a single YAML config file -- defines site name, theme, navigation structure, plugins.

Example minimal version:

```yaml
site_name: My Site Name
theme:
  name: material
  features:
    - navigation.tabs
    - search.suggest
nav:
  - Home: index.md
  - Automation:
      - Cucumber + JUnit 5: automation/cucumber-junit5.md
      - Allure Report: automation/allure-report.md
  - Build Tools:
      - Maven: build-tools/maven.md
      - Gradle: build-tools/gradle.md
```

- Markdown files: your actual content. Same `.md` syntax used in GitHub READMEs. No special framework, no React components, no JSX. Just write.

## Full workflow end-to-end

```bash
# 1. Install (one-time)
pip install mkdocs-material

# 2. Create project
mkdocs new my-site
cd my-site

# 3. Initialize git (one-time)
git init

# 4. Edit mkdocs.yml + write markdown files

# 5. Preview locally
mkdocs serve    # opens at http://localhost:8000, live-reloads on save

# 6. Commit + push main branch
git add .
git commit -m "your commit message"
git push -u origin main

# 7. Deploy to GitHub Pages
mkdocs gh-deploy
```

## What `mkdocs gh-deploy` actually does

It's a convenience wrapper that:

1. Runs `mkdocs build` -> generates the static site into a `site/` folder
2. Pushes the contents of `site/` to a branch called `gh-pages` on your `origin` remote
3. GitHub detects the `gh-pages` branch and serves it at `https://<username>.github.io/<repo>

So you end up with two branches:

- `main` -> your source (markdown files, `mkdocs.yml`)
- `gh-pages` -> the built HTML (auto-managed, don't edit manually)

## One-time GitHub Pages setup

After the first `gh-deploy`, go to: Repo -> Settings -> Pages and confirm:

- Source: `Deploy from a branch`
- Branch: `gh-pages`/`(root)`

Usually GitHub auto-detects this, but worth verifying.

## Better approach: GitHub Actions

`mkdocs gh-deploy` works but deploys from your local machine -- meaning the site only updates when you remember to run it. Cleaner workflow: deploy automatically on every push to `main` via GitHub Actions.

Create `.github/workflows/deploy.yml`:

```yaml
name: Deploy MkDocs

on:
    push:
        branches: [main]

permissions:
    contents: write

jobs:
    deploy:
        runs-on: ubuntu-latest
        steps:
            - uses: actions/checkout@v4
            - uses: actions/setup-python@v5
                with:
                    python-version:'3.x'
            - run: pip install mkdocs-material
            - run: mkdocs gh-deploy --force
```

Now the flow is:

```bash
# Edit markdown -> commit -> push
git add .
git commit -m "your commit message"
git push
# Site auto-deploys in -1 minute
```
