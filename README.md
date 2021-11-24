# hugo-action-html-validate
GitHub action to validate HTML of a Hugo site

## Status

### Main

![test-build-validate](https://github.com/danielfdickinson/hugo-action-html-validate/actions/workflows/test-build-validate.yml/badge.svg)

### Pull Request

![test-build-validate PR](https://github.com/danielfdickinson/hugo-action-html-validate/actions/workflows/test-build-validate.yml/badge.svg?event=pull_request)

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
* ``.htmlvalidate.json``
* (optional) ``.htmlvalidateignore``
* ``package.json``
* (recommended) ``package-lock.json``

``package.json`` and ``package-lock.json`` are used by ``npm install`` to install ``html-validate`` which does the validation.

``html-validate`` is <https://www.npmjs.com/> with homepage <https://html-validate.org> and uses ``.htmlvalidate.json`` and ``.htmlvalidateignore``.


### Outputs

None

