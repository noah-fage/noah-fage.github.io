# noah-fage.github.io

Personal site, built with Jekyll, hosted on GitHub Pages.

## Add a blog post

Create a file in `_posts/` named `YYYY-MM-DD-some-title.md`:

```
---
layout: post
title: "Your title"
date: 2026-09-15
tags: [tag-one, tag-two]
---

Your post in Markdown.
```

Commit and push. GitHub Pages rebuilds automatically in about a minute. The post
shows up on the home page and on `/writing/`.

## Edit a page

`index.md` (home), `projects.md`, `about.md`, `writing.md` are plain Markdown
with a small front-matter block at the top. Edit and push.

## Run locally (optional)

```
gem install bundler jekyll
jekyll serve
```

Then open http://localhost:4000. Not required. You can edit files on GitHub
directly and see the result live in a minute or two.
