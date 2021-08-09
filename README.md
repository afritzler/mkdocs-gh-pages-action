# mkdocs-gh-pages-action

![license](https://img.shields.io/github/license/afritzler/mkdocs-gh-pages-action)

Github Action to build and release mkdocs as Github pages without using Docker.
At the core this action is using the [Mkdocs Material](https://squidfunk.github.io/mkdocs-material/) project
to build your documentation.

## Why?

There are already a couple of great Github actions out there building your mkdocs project
and publishing it as Github pages such as

* <https://github.com/marketplace/actions/deploy-mkdocs>

This project aims to provide a Github action without using a Docker container in the build
and publishing step. It comes handy for people running in a Github Enterprise environment using
`self-hosted` runners which don't have Docker-in-Docker support enabled.

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
          #GITHUB_DOMAIN: github.myenterprise.com
```

## Configuration

This action can be configured by setting the following `env` varibles

| **ENV VAR**        | **Default Value**     | **Notes**                                                                                         |
| ------------------ | --------------------- | ------------------------------------------------------------------------------------------------- |
| **CUSTOM_DOMAIN**  | `none`                | Custom domain name which is being written into the CNAME file of your gh-pages branch.            |
| **CONFIG_FILE**    | `mkdocs.yml`          | Custom Mkdocs configuration file relative in your project folder structure.                       |
| **GITHUB_TOKEN**   | `none`                | Personal Github token which should be used during execution.                                      |
| **PERSONAL_TOKEN** | `none`                | Personal access token which should be used during execution.                                      |
| **GITHUB_DOMAIN**  | `github.com`          | Specify a custom Github domain in case Github Enterprise is used: e.g. `github.myenterprise.com`. |
| **ACTOR**          | `author of PR/commit` | Overwrite the author of the PR/commit.                                                            |
| **REQUIREMENTS**   | `requirements.txt`    | Specify a custom `requirements.txt` file to use custom Python modules.                            |
| **FORCE**          | `"true"`              | Force defines if the `mkdocs` build artefacts should be force pushed into the `gh-pages` |

## Using custom modules

By default this action is looking for the presents of a `requirements.txt` in your project to install your custom modules.

Example: Lets say you want to install the following modules as they represent Mkdocs plugins used by your project:

```shell
mkdocs-same-dir==v0.1.0
mdx_truly_sane_lists==1.2
```

Create a corresponding file containing a versioned list of those modules (`pip freeze`). You can override the default filename `requirements.txt` by
setting the `REQUIREMENTS` environment variable.

```yaml
- name: Deploy docs
  uses: afritzler/mkdocs-gh-pages-action@main
  env:
    GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
    REQUIREMENTS: myawesome-requirements.txt
```
