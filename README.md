# frappe-suite

<!-- sf:project:start -->
<!-- sf:project:end -->
<!-- sf:badges:start -->
<!-- sf:badges:end -->
<!-- sf:image:start -->
<!-- sf:image:end -->
<!-- sf:contents-base:start -->
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
