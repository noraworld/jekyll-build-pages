# Jekyll Build Pages
A simple GitHub Action for producing Jekyll build artifacts with customizable environments.

## Concept of this project
You want to use the custom plugins and the latest version of Jekyll, but don't want something messy? This Action is the best for you!

### Issues about `actions/jekyll-build-pages`
GitHub has provided the easiest way to build Jekyll and deploy to GitHub Pages. None of the hassle procedures are needed. All you have to do is just enable GitHub Pages.

* [Setting up a GitHub Pages site with Jekyll - GitHub Docs](https://docs.github.com/en/pages/setting-up-a-github-pages-site-with-jekyll)
* [GitHub Pages | Jekyll • Simple, blog-aware, static sites](https://jekyllrb.com/docs/github-pages/)
* [GitHub Actions | Jekyll • Simple, blog-aware, static sites](https://jekyllrb.com/docs/continuous-integration/github-actions/)

However, the versions of its dependencies are stale! Even in 2025, it uses Jekyll 3.10.0, and in this version, some of the useful features are unavailable, such as [`render_with_liquid`](https://jekyllrb.com/docs/liquid/tags/).

[Dependency versions | GitHub Pages](https://pages.github.com/versions/)

Furthermore, it only allows you to use [kramdown](https://github.com/gettalong/kramdown) as a markdown parser, and the parser has a lot of problems as follows.

* The tables after the deadlines aren't parsed unless you put an empty line between them
* The markdown isn't available in the `<details>` tag
* `|` is interpreted as the table when it exists in a link title (e.g. [Example Domain | example.com](https://example.com))
* The link without a title isn't parsed as a link

They haven't been resolved as of [v2.5.1](https://rubygems.org/gems/kramdown/versions/2.5.1). It's difficult to use, at least for me.

### What's different?
This repository basically offers the same functions as [`actions/jekyll-build-pages`](https://github.com/actions/jekyll-build-pages), but it allows you to use the latest version of Jekyll and its dependencies. You can also use the custom plugins that aren't available in [`actions/jekyll-build-pages`](https://github.com/actions/jekyll-build-pages) by configuring `Gemfile` on your own.

`.ruby-version` and `Gemfile` are maintained on this repository, so you don't need to have them configured. You can use your own ones if you want to use the plugins that aren't listed on [`Gemfile`](Gemfile) and the custom ones.

I put an effort to keep them updated, but please keep in mind that the updating may be delayed. The pull requests to improve this project are always welcomed!

## Sample workflow
Navigate to the GitHub Pages settings (`https://github.com/<USER>/<REPO>/settings/pages`) and select the source `GitHub Actions`. And then put the following YAML file in `.github/workflows/`. Copying and pasting without any change should work fine if you use the `main` branch.

```yaml
name: Build and Deploy Jekyll

on:
  push:
    branches:
      - main
  workflow_dispatch:

permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: pages
  cancel-in-progress: false

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Build with Jekyll
        uses: noraworld/jekyll-build-pages@main
        with:
          use_ruby_version: false
          use_gemfile: false

  deploy:
    needs: build
    runs-on: ubuntu-latest
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

Consider locking the version if you don't want to change the behavior implicitly. See [the tags](https://github.com/noraworld/jekyll-build-pages/tags) for the versions list.

```diff
-        uses: noraworld/jekyll-build-pages@main
+        uses: noraworld/jekyll-build-pages@vX.X.X  # <- Replace X.X.X with a specific version
```

### Options
If you don't set these options, the configurations on this project will be used.

| Key                | Type    | Default | Description                                                                    |
| ------------------ | :-----: | :-----: | ------------------------------------------------------------------------------ |
| `use_ruby_version` | Boolean | `false` | Set your preferred Ruby version (`.ruby-version` is needed on your repository) |
| `use_gemfile`      | Boolean | `false` | Use your preferred Gems (`Gemfile` is needed on your repository)               |

### One more thing...
Are you a lazy person? Same here! This repository offers a much easier way to set up a workflow. All you need to do is copy and paste the YAML code below.

```yaml
name: Build and Deploy Jekyll

on:
  push:
    branches:
      - main
  workflow_dispatch:

jobs:
  build_and_deploy:
    uses: noraworld/jekyll-build-pages/.github/workflows/jekyll-gh-pages.yml@main
```

`use_ruby_version` and `use_gemfile` are also available here too.

Note that `jekyll-gh-pages.yml` uses the latest version of `action.yml` internally, which means the behavior might change as it's updated.

## License
All files in this project are released under the [MIT License](LICENSE).
