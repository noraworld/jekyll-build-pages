---
layout: post
title: Yikes!
---

If you see this page on your repository, it implies that something went wrong.

It looks like you don't exclude the `jekyll-build-pages` directory, which was cloned on a runner on your repository to use `.ruby-version` and `Gemfile` on [Jekyll Build Pages](https://github.com/noraworld/jekyll-build-pages) Action. In order to get this Action to work properly, you'll need to exclude the `jekyll-build-pages` directory.

```yaml
exclude:
  - jekyll-build-pages
```

If you want to use your own `.ruby-version` and `Gemfile` instead of the ones on this Action, you can declare that by `with.use_ruby_version` and `with.use_gemfile` in your workflow.

```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Build with Jekyll
        uses: noraworld/jekyll-build-pages@main
        with:
          use_ruby_version: true
          use_gemfile: true
```

If you run into trouble, you can ask a question at [noraworld/jekyll-build-pages](https://github.com/noraworld/jekyll-build-pages/issues)!
