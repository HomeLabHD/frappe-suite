# frappe-suite

A Frappe container image with ERPNext, HR, and Helpdesk baked in — a drop-in replacement
for `frappe/erpnext` that carries the whole set.

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

> Not [frappe/suite](https://github.com/frappe/suite), Frappe's experimental productivity
> tools.

## What's inside

| app | what it gives you |
|-----|-------------------|
| `frappe` | the framework the rest runs on |
| `erpnext` | ERP and CRM — lead → opportunity → quotation → customer |
| `hrms` | HR: employees, leave, attendance, payroll |
| `helpdesk` | ticketing |
| `telephony` | Exotel/Twilio call integration; `helpdesk` requires it |

Built on the **v15** framework line. The apps move as a set — a v15 app cannot run on a
v16 framework.

## Use it

The image goes wherever `frappe/erpnext` would, for every service role — backend,
frontend, scheduler, worker. In frappe_docker's compose that is two variables:

```sh
CUSTOM_IMAGE=hlhd/frappe-suite
CUSTOM_TAG=latest-dev
```

Or pull it directly:

```sh
docker pull hlhd/frappe-suite:latest-dev              # Docker Hub
docker pull ghcr.io/homelabhd/frappe-suite:latest-dev # GHCR
```

## Installing apps on a site

Being in the image is not being installed. Each site opts in:

```sh
bench --site <site> install-app hrms
bench --site <site> install-app helpdesk
```

Until then an app adds no tables, no routes, and no scheduler jobs — it is only disk.

## Building it yourself

`apps.json` is the app set; the build recipe is upstream's, pinned as a submodule.

```sh
git submodule update --init
```

## License

GPL-3.0
