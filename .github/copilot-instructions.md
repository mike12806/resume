# GitHub Copilot Instructions

## Project Overview
This repository is a **personal resume website** for Michael E. Faherty. It is a static HTML/CSS site built with Bootstrap and deployed to **Cloudflare Pages** at `resume.mfaherty.net`. All changes are version-controlled in Git and deployed automatically via GitHub Actions.

## Repository Structure

```
.github/
  workflows/
    deploy.yml              # Deploy to Cloudflare Pages on push to main
    renovate-validate.yml   # Validate Renovate config on pull requests
  copilot-instructions.md   # This file
resume/
  index.html                # Resume content
  style.css                 # Custom styles
  bootstrap.min.css         # Bootswatch "lux" theme (Bootstrap)
renovate.json               # Renovate dependency update configuration
```

## Technology Stack

- **Frontend**: Static HTML5 and CSS3 with Bootstrap (Bootswatch "lux" theme)
- **Hosting**: Cloudflare Pages, served at `resume.mfaherty.net`
- **Deployment**: GitHub Actions using `cloudflare/wrangler-action`
- **Dependency updates**: Renovate (tracking Wrangler and GitHub Actions versions)

## HTML & CSS Guidelines

- Use **semantic HTML5 elements** (`<header>`, `<section>`, `<footer>`, etc.)
- Use **Bootstrap utility classes** wherever possible before writing custom CSS
- Keep custom styles in `style.css`; avoid inline styles
- Use **2 spaces** for indentation in HTML and CSS files
- Keep the resume content clean, readable, and accessible

## Deployment

### How It Works
Pushing to the `main` branch triggers the `deploy.yml` workflow, which:
1. Deploys the `resume/` directory to Cloudflare Pages using Wrangler
2. Registers and verifies the custom domain `resume.mfaherty.net`
3. Configures a DNS CNAME record in the `mfaherty.net` Cloudflare zone
4. Purges the Cloudflare cache so visitors see the latest version

### Required Secrets
- `CF_API_TOKEN` — Cloudflare API token with Pages and DNS permissions
- `CF_ACCOUNT_ID` — Cloudflare account ID

### Wrangler Version
The Wrangler CLI version is pinned in `deploy.yml` via the `WRANGLER_VERSION` env variable and tracked by Renovate.

## Renovate Configuration

This project uses **Renovate** for automated dependency updates:

- **Automerge is disabled** — all updates require manual review before merging
- **GitHub Actions** workflow dependencies are tracked and updated automatically
- **Wrangler** npm package version (used in the deploy workflow) is tracked via a custom regex manager

## CI/CD Workflow

1. **Make changes** in a feature branch
2. **Open a pull request** — Renovate config is validated automatically
3. **Review** changes for correctness
4. **Merge** to `main` — triggers automatic deployment to Cloudflare Pages

## Best Practices

- **Keep secrets out of code** — use GitHub Actions secrets for API tokens
- **Pin dependency versions** — use specific version tags for actions and Wrangler
- **Use meaningful commit messages** — describe what changed and why
- **Test changes locally** before opening a pull request by previewing the HTML in a browser
