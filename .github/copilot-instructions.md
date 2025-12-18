# Copilot / AI agent instructions for AEON

Purpose
- Short, actionable guidance to help AI agents make correct, minimal, and reviewable changes to this repository.

Big picture
- This is a minimal static Jekyll site. Content lives at the repo root (e.g., `index.html`) and the repository includes a `CNAME` indicating a custom domain.
- The repo is built by a GitHub Actions workflow (`.github/workflows/jekyll-docker.yml`) that runs a Dockerized Jekyll builder and writes output to `_site/`.

Key files
- `index.html` — site entry point and likely the main file to edit for content updates.
- `CNAME` — custom domain; do not modify without explicit approval from repo owner / maintainers.
- `.github/workflows/jekyll-docker.yml` — CI build; see build command below.

How CI builds the site (exact command)
- The workflow runs:

  docker run -v ${GITHUB_WORKSPACE}:/srv/jekyll -v ${GITHUB_WORKSPACE}/_site:/srv/jekyll/_site jekyll/builder:latest /bin/bash -c "chmod -R 777 /srv/jekyll && jekyll build --future"

  - `--future` means future-dated posts (if added) will be included.
  - `chmod -R 777` is used in the container to avoid permission errors when writing `_site/`.

Local reproduction (recommended)
- Use the same Docker image to reproduce CI locally:

  docker run --rm -it -v "$PWD":/srv/jekyll -v "$PWD"/_site:/srv/jekyll/_site -p 4000:4000 jekyll/builder:latest /bin/bash -c "jekyll serve --host 0.0.0.0 --port 4000 --future"

- Or, if you have Ruby/Jekyll locally installed: `jekyll build` or `jekyll serve --future`

Project conventions and patterns
- This repo currently has no `_config.yml` or Jekyll-specific config file; adding one is fine but ensure you test the site locally with the commands above.
- `_site/` is the generated output; avoid committing `_site/` to the repo (it is build artifact).
- Content additions should follow standard Jekyll conventions if you add collections/posts/layouts (`_posts/`, `_layouts/`, etc.).

PR / change guidance for AI agents
- Keep edits minimal and limited to content/HTML unless the issue explicitly asks for adding Jekyll structure.
- If adding or changing DNS/CNAME, require explicit human approval and note domain implications in PR description.
- If you add `_config.yml` or other Jekyll config, include a brief note in the PR describing how you tested locally and any new behavior.

Troubleshooting notes
- If CI fails with permission errors, check whether the container is able to write `_site/` (the workflow uses `chmod -R 777` to address this).
- If builds don't reflect changes, ensure files are committed to `main` or in a PR targeting `main` (workflow triggers are for `main`).

When to open an issue vs. make a change
- Minor content fixes (typos, copy updates) — make a small PR directly referencing the file changed.
- Structural changes (adding Jekyll config, posts structure, or deployment-related changes) — open an issue first and reference testing steps.

If anything here is unclear or you'd like additional coverage (e.g., deploy targets, DNS, or adding tests), tell me what to expand and I will iterate. ✅
