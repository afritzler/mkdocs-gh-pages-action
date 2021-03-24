# mkdocs-gh-pages-action

Github Action to build and release mkdocs as Github pages without using Docker.

## Usage

Here is an example on how to use this action to publish your project:

```yaml
name: Publish docs via GitHub Pages

on:
  push:
    branches: [ main ]

jobs:
  build:
    name: Deploy docs
    runs-on: ubuntu-latest
    steps:
      - name: Checkout main
        uses: actions/checkout@v2

      - uses: actions/setup-python@v2
        with:
          python-version: 'pypy-3.6'

      - name: Deploy docs
        uses: afritzler/mkdocs-gh-pages-action@main
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```
