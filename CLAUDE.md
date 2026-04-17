# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository purpose

Personal site at `rstarkey.github.io`. Built as a Jekyll site served by GitHub Pages. No JavaScript build step, no test suite — content is authored in Markdown and rendered through Liquid templates at deploy time by GitHub Pages.

## Common commands

Dependencies are managed with Bundler; Jekyll is run via `bundle exec`.

```bash
bundle install                 # first-time setup / after Gemfile changes
bundle exec jekyll serve       # local dev server with live reload → http://localhost:4000
bundle exec jekyll build       # one-shot build into _site/
bundle update github-pages     # bump Jekyll + plugins to the latest GitHub Pages set
```

`_site/` and `Gemfile.lock` are gitignored — local builds are ephemeral and GitHub Pages resolves its own lockfile when deploying.

Config edits require a server restart: `_config.yml` is NOT reloaded by `jekyll serve`.

## Architecture notes

**Theme is remote, layout is local.** `_config.yml` pulls in `remote_theme: pages-themes/midnight@v0.2.0` via the `jekyll-remote-theme` plugin. The theme's layouts live inside the gem and aren't checked in. To customize the chrome (header, footer, credits, scripts), the repo shadows just the file being changed — currently only `_layouts/default.html`. When modifying site chrome, prefer overriding the specific layout or include rather than forking the whole theme. Recent commits (`fdf1ffa`, `f46bbf8`) followed this pattern to strip the GitHub fork ribbon and "Hosted on GitHub Pages" credit.

**Deployment is implicit.** There's no CI or GitHub Actions workflow. Pushing to `main` triggers GitHub Pages' built-in Jekyll build. That build environment only allows the plugin/gem versions from the `github-pages` gem — don't add plugins outside the `:jekyll_plugins` group or they won't load in production.

**Content conventions.** Posts live in `_posts/` named `YYYY-MM-DD-slug.markdown`; standalone pages (`about.markdown`, `index.markdown`, `404.html`) live at the repo root with YAML front matter selecting a layout. The site currently relies solely on the default layout.

## Known placeholders

`_config.yml` still has the `jekyll new` scaffolding values: `title: Your awesome title`, `email: your-email@example.com`, `twitter_username: jekyllrb`, `github_username: jekyll`. Treat these as TODOs — they render into `<title>`/SEO meta via `{% seo %}`. Update them when doing any user-facing work on the site.
