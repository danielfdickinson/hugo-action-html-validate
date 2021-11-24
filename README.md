# hugo-action-html-validate
GitHub action to validate HTML of a Hugo site

## Status

### Main

![test-build-validate](https://github.com/danielfdickinson/hugo-action-html-validate/actions/workflows/test-html-validate.yml/badge.svg)

### Pull Request

![test-build-validate PR](https://github.com/danielfdickinson/hugo-action-html-validate/actions/workflows/test-html-validate.yml/badge.svg?event=pull_request)

## Details

**NB**: Currently experimental and not for public consumption. Use at your own risk.

### Purpose

Pass/Fail Validation HTML of a site stored in an tarball (artifact) from a previous step. Intended to fail a pull request or push with invalidate HTML.

### Inputs


| Input | Req'd | Default | Meaning |
|-------|-------|---------|---------|
| download-site-as | yes | unminified-site | GitHub Artifact containing a tarball with the site and ``html-validate`` configuration | 
| output-directory | yes | public | subdirectory (in tarball) containing the site to validate |

The tarball in the artifact pointed to by ``download-site-as`` must be named ``hugo-site.tar`` and contain the following:

* A subdirectory tree containing the site (default: _public_, optionally defined by ``output-directory``).

Needed configuration must exist in the workspace. The best way to do this is to do a shallow checkout of the repo.

* ``package.json`` and ``package-lock.json`` are used by ``npm install`` to install ``html-validate`` which does the validation.
* ``html-validate`` is <https://www.npmjs.com/package/html-validate/> with homepage <https://html-validate.org> and uses ``.htmlvalidate.json`` and ``.htmlvalidateignore``.

### Outputs

None

### Sample usage

```yaml
name: test-html-validate
on:
  pull_request:
    types:
      - assigned
      - opened
      - synchronize
      - reopened
  push:
    branches:
      - 'feature**'
      - main
      - 'staging**'
    paths-ignore:
      - 'README.md'
      - 'LICENSE'
      - '.gitignore'
      - '.vscode'
      - 'scripts'
jobs:
  build-unminified-site:
    runs-on: ubuntu-20.04
    steps:
      - name: "Build Site with Hugo and Audit"
        uses: danielfdickinson/hugo-action-build-audit@v0.1.1
        with:
          source-directory: src
          upload-site-as: unminified-site
          use-lfs: false
  validate-unminified-html:
    needs: build-unminified-site
    runs-on: ubuntu-20.04
    steps:
      - uses: actions/checkout@v2
      - name: Run hugo-action-html-validate
        uses: danielfdickinson/hugo-action-html-validate
```