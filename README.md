# frappe-suite

<!-- sf:project:start -->
[![GitHub](https://img.shields.io/badge/GitHub-mirror-181717?logo=github)](https://github.com/HomeLabHD/frappe-suite) [![GitLab](https://img.shields.io/badge/GitLab-source-FC6D26?logo=gitlab)](https://gitlab.prplanit.com/HomeLabHD/frappe-suite) [![license](https://raw.githubusercontent.com/HomeLabHD/frappe-suite/main/.stagefreight/scribe/license.svg)](https://github.com/HomeLabHD/frappe-suite/blob/main/LICENSE) [![Open Issues](https://img.shields.io/github/issues/HomeLabHD/frappe-suite)](https://github.com/HomeLabHD/frappe-suite/issues) [![Open PRs](https://img.shields.io/github/issues-pr/HomeLabHD/frappe-suite)](https://github.com/HomeLabHD/frappe-suite/pulls) [![Contributors](https://img.shields.io/github/contributors/HomeLabHD/frappe-suite)](https://github.com/HomeLabHD/frappe-suite/graphs/contributors) [![donate](https://img.shields.io/badge/donate-FF5E5B?logo=ko-fi&logoColor=white)](https://ko-fi.com/T6T41IT163) [![sponsor](https://img.shields.io/badge/sponsor-EA4AAA?logo=githubsponsors&logoColor=white)](https://github.com/sponsors/HomeLabHD)
<!-- sf:project:end -->
<!-- sf:badges:start -->
[![release](https://raw.githubusercontent.com/HomeLabHD/frappe-suite/main/.stagefreight/scribe/release.svg)](https://github.com/HomeLabHD/frappe-suite/releases) [![build](https://raw.githubusercontent.com/HomeLabHD/frappe-suite/main/.stagefreight/scribe/build.svg)](https://gitlab.prplanit.com/HomeLabHD/frappe-suite/-/pipelines) [![Last Commit](https://img.shields.io/github/last-commit/HomeLabHD/frappe-suite)](https://github.com/HomeLabHD/frappe-suite/commits) [![StageFreight](https://img.shields.io/badge/StageFreight-0.10.0--dev+e3ee67e-310937?logo=readthedocs&logoColor=white)](https://stagefreight.prplanit.com)
<!-- sf:badges:end -->
<!-- sf:image:start -->
[![GHCR](https://img.shields.io/badge/GHCR-homelabhd%2Ffrappe--suite-181717?logo=github&logoColor=white)](https://github.com/HomeLabHD/frappe-suite/pkgs/container/frappe-suite) [![Docker](https://img.shields.io/badge/Docker-hlhd%2Ffrappe--suite-2496ED?logo=docker&logoColor=white)](https://hub.docker.com/r/hlhd/frappe-suite) [![pulls](https://raw.githubusercontent.com/HomeLabHD/frappe-suite/main/.stagefreight/scribe/pulls.svg)](https://hub.docker.com/r/hlhd/frappe-suite) [![Harbor](https://img.shields.io/badge/Harbor-hlhd%2Ffrappe--suite-60b932)](https://cr.pcfae.com/harbor/projects)

[![latest](https://raw.githubusercontent.com/HomeLabHD/frappe-suite/main/.stagefreight/scribe/release-latest.svg)](https://github.com/HomeLabHD/frappe-suite/pkgs/container/frappe-suite) ![updated](https://raw.githubusercontent.com/HomeLabHD/frappe-suite/main/.stagefreight/scribe/release-updated.svg) [![size](https://raw.githubusercontent.com/HomeLabHD/frappe-suite/main/.stagefreight/scribe/release-size.svg)](https://github.com/HomeLabHD/frappe-suite/pkgs/container/frappe-suite) [![latest-dev](https://raw.githubusercontent.com/HomeLabHD/frappe-suite/main/.stagefreight/scribe/dev-latest.svg)](https://github.com/HomeLabHD/frappe-suite/pkgs/container/frappe-suite) ![updated](https://raw.githubusercontent.com/HomeLabHD/frappe-suite/main/.stagefreight/scribe/dev-updated.svg) [![size](https://raw.githubusercontent.com/HomeLabHD/frappe-suite/main/.stagefreight/scribe/dev-size.svg)](https://github.com/HomeLabHD/frappe-suite/pkgs/container/frappe-suite)
<!-- sf:image:end -->
<!-- sf:contents-base:start -->
[![python 3.14.2](https://img.shields.io/badge/python-3.14.2-0078D4?style=flat)](https://hub.docker.com/_/python)
<!-- sf:contents-base:end -->

A Frappe application **suite**: the framework plus the business apps we run on it.

Named for the platform rather than the payload — the apps in it are expected to change,
and `apps.json` is the only place that says which are present. Not `bench`, which upstream
already publishes as a development container, and not `framework`, which is the framework
alone without any of the apps below.

## Apps

| app | why |
|-----|-----|
| `frappe` | the framework itself; implied by the build, not listed |
| `erpnext` | ERP, and CRM — lead → opportunity → quotation → customer is native to it |
| `hrms` | HR. Split out of ERPNext at v14, so it is absent from the stock image |
| `helpdesk` | ticketing |
| `telephony` | Exotel/Twilio integration. Not a product choice — `helpdesk` declares it in `required_apps`, so it arrives whether or not any call ever routes through it |

`erpnext` and `hrms` are pinned to `version-15`. `helpdesk` and `telephony` cut no
version branches at all — upstream releases them from `main` and `develop` respectively,
and upstream's own install instructions name those branches.
A v15 app cannot run on a v16 framework, so the whole set moves together or not at all.

## Baked ≠ installed

An app in this image is inert until a site installs it:

```sh
bench --site <site> install-app hrms
```

So an unused app costs image size and build time, not runtime surface — no tables, no
routes, no scheduler jobs. Add one by adding a line here and rebuilding; that is cheap
enough that speculative apps are not worth their disk.

## How it is built

`frappe_docker` is a **pinned submodule**, not a vendored copy: upstream owns the build
recipe, this repo owns the app set, and neither can drift into the other. The pin is
visible in git, and moving it is a reviewable commit.

```sh
git submodule update --init                 # once, and in CI
```

Built from frappe_docker's layered `images/custom/Containerfile`. `apps.json` reaches it
as a **secret**, which is how the current Containerfile takes it — the `APPS_JSON_BASE64`
form most guides still show was removed upstream. The heavy part is the node/yarn asset
build, one pass per app with a UI.

Three build args are set deliberately:

| arg | why |
|-----|-----|
| `FRAPPE_BRANCH=version-15` | the Containerfile defaults to `version-16`, and a v15 app cannot run on a v16 framework |
| `PYTHON_VERSION`, `NODE_VERSION` | pinned exactly; a build that floats its interpreter is not reproducible whatever else it pins |

### CI must fetch the submodule

A pipeline that clones without submodules gets an empty `frappe_docker/`, and the build
fails on a missing Containerfile rather than on anything to do with this repo.
