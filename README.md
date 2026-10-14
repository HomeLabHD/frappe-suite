# frappe-suite

A Frappe container image with ERPNext, HR, and Helpdesk baked in — a drop-in replacement
for `frappe/erpnext` that carries the whole set.

<!-- sf:project:start -->
[![GitHub](https://img.shields.io/badge/GitHub-mirror-181717?logo=github)](https://github.com/HomeLabHD/frappe-suite) [![GitLab](https://img.shields.io/badge/GitLab-source-FC6D26?logo=gitlab)](https://gitlab.prplanit.com/HomeLabHD/frappe-suite) [![license](https://raw.githubusercontent.com/HomeLabHD/frappe-suite/main/.stagefreight/scribe/license.svg)](https://github.com/HomeLabHD/frappe-suite/blob/main/LICENSE) [![Open Issues](https://img.shields.io/github/issues/HomeLabHD/frappe-suite)](https://github.com/HomeLabHD/frappe-suite/issues) [![Open PRs](https://img.shields.io/github/issues-pr/HomeLabHD/frappe-suite)](https://github.com/HomeLabHD/frappe-suite/pulls) [![Contributors](https://img.shields.io/github/contributors/HomeLabHD/frappe-suite)](https://github.com/HomeLabHD/frappe-suite/graphs/contributors) [![donate](https://img.shields.io/badge/donate-FF5E5B?logo=ko-fi&logoColor=white)](https://ko-fi.com/T6T41IT163) [![sponsor](https://img.shields.io/badge/sponsor-EA4AAA?logo=githubsponsors&logoColor=white)](https://github.com/sponsors/HomeLabHD)
<!-- sf:project:end -->
<!-- sf:badges:start -->
[![release](https://raw.githubusercontent.com/HomeLabHD/frappe-suite/main/.stagefreight/scribe/release.svg)](https://github.com/HomeLabHD/frappe-suite/releases) [![build](https://raw.githubusercontent.com/HomeLabHD/frappe-suite/main/.stagefreight/scribe/build.svg)](https://gitlab.prplanit.com/HomeLabHD/frappe-suite/-/pipelines) [![Last Commit](https://img.shields.io/github/last-commit/HomeLabHD/frappe-suite)](https://github.com/HomeLabHD/frappe-suite/commits) [![StageFreight](https://img.shields.io/badge/StageFreight-0.14.0--dev+f5a3826-310937?logo=readthedocs&logoColor=white)](https://stagefreight.prplanit.com)
<!-- sf:badges:end -->
<!-- sf:image:start -->
[![GHCR](https://img.shields.io/badge/GHCR-homelabhd%2Ffrappe--suite-181717?logo=github&logoColor=white)](https://github.com/HomeLabHD/frappe-suite/pkgs/container/frappe-suite) [![Docker](https://img.shields.io/badge/Docker-hlhd%2Ffrappe--suite-2496ED?logo=docker&logoColor=white)](https://hub.docker.com/r/hlhd/frappe-suite) [![pulls](https://img.shields.io/docker/pulls/hlhd/frappe-suite?color=1d63ed&logo=docker&logoColor=white)](https://hub.docker.com/r/hlhd/frappe-suite) [![Harbor](https://img.shields.io/badge/Harbor-hlhd%2Ffrappe--suite-60b932)](https://cr.pcfae.com/harbor/projects)

[![latest](https://raw.githubusercontent.com/HomeLabHD/frappe-suite/main/.stagefreight/scribe/release-latest.svg)](https://github.com/HomeLabHD/frappe-suite/pkgs/container/frappe-suite) ![updated](https://raw.githubusercontent.com/HomeLabHD/frappe-suite/main/.stagefreight/scribe/release-updated.svg) [![size](https://raw.githubusercontent.com/HomeLabHD/frappe-suite/main/.stagefreight/scribe/release-size.svg)](https://github.com/HomeLabHD/frappe-suite/pkgs/container/frappe-suite) [![latest-dev](https://raw.githubusercontent.com/HomeLabHD/frappe-suite/main/.stagefreight/scribe/dev-latest.svg)](https://github.com/HomeLabHD/frappe-suite/pkgs/container/frappe-suite) ![updated](https://raw.githubusercontent.com/HomeLabHD/frappe-suite/main/.stagefreight/scribe/dev-updated.svg) [![size](https://raw.githubusercontent.com/HomeLabHD/frappe-suite/main/.stagefreight/scribe/dev-size.svg)](https://github.com/HomeLabHD/frappe-suite/pkgs/container/frappe-suite)
<!-- sf:image:end -->

> Not [frappe/suite](https://github.com/frappe/suite), Frappe's experimental productivity tools.

## What's inside

<!-- sf:contents-base:start -->
[![python 3.14.2](https://img.shields.io/badge/python-3.14.2-0078D4?style=flat)](https://hub.docker.com/_/python)
<!-- sf:contents-base:end -->
<!-- sf:contents-apps:start -->
![frappe 16.51.0](https://img.shields.io/badge/frappe-16.51.0-2ea043?style=flat) ![erpnext 16.50.0](https://img.shields.io/badge/erpnext-16.50.0-2ea043?style=flat) ![hrms 16.50.0](https://img.shields.io/badge/hrms-16.50.0-2ea043?style=flat) ![crm 1.87.0](https://img.shields.io/badge/crm-1.87.0-2ea043?style=flat) ![helpdesk 1.30.2](https://img.shields.io/badge/helpdesk-1.30.2-2ea043?style=flat) ![telephony 0.0.1](https://img.shields.io/badge/telephony-0.0.1-2ea043?style=flat)
<!-- sf:contents-apps:end -->

| app | what it gives you |
|-----|-------------------|
| `frappe` | the framework the rest runs on |
| `erpnext` | ERP: accounting, stock, buying, selling, projects |
| `hrms` | HR: employees, leave, attendance, payroll |
| `helpdesk` | ticketing |
| `crm` | the standalone CRM — pipeline, deals, contacts, its own UI. Distinct from ERPNext's built-in CRM module, which is lead → quotation → customer inside the desk |
| `telephony` | Exotel/Twilio call integration; `helpdesk` requires it |

Built on the **v16** line. Everything that publishes a `version-16` branch tracks it, so
framework and app patches arrive without an edit here. `helpdesk` and `telephony` publish
no version branches at all — `helpdesk` and `crm` track `main`, `telephony` tracks `develop`, which is what upstream's own
install instructions name.

## Use it

The image goes wherever `frappe/erpnext` would, for every service role — backend, frontend, scheduler, worker. In frappe_docker's compose that is two variables:

```sh
CUSTOM_IMAGE=hlhd/frappe-suite
CUSTOM_TAG=latest-v16
```

`latest-v16` follows the v16 line. For a build that never moves under you, pin the
immutable `v<version>` tag of a [release](https://github.com/HomeLabHD/frappe-suite/releases)
instead — that version names this image, not ERPNext's.

Or pull it directly:

```sh
docker pull hlhd/frappe-suite:latest-v16              # Docker Hub
docker pull ghcr.io/homelabhd/frappe-suite:latest-v16 # GHCR
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
