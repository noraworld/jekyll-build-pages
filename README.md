# Jekyll Build Pages
A simple GitHub Action for producing Jekyll build artifacts with customizable environments.

## Sample workflow
```yaml
name: Build and Deploy Jekyll with GitHub Actions

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

Are you a lazy person? Same here! This repository offers a easier way to set up a workflow. All you need to do is copy and paste the YAML code below.

```yaml
name: Build and Deploy Jekyll with GitHub Actions

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
