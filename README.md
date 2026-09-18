# ci-example

Phase 4 acceptance project for the portfolio's monitoring: a one-file static
site created from [`ulisseas/project-template`](https://github.com/ulisseas/project-template)
that proves CI telemetry, releases and uptime monitoring work end to end
with zero per-repo configuration.

- Site: https://ci-example.ulisseas.com (GitHub Pages, custom domain; served
  at https://ulisseas.github.io/ci-example until the DNS record exists)
- Source: `site/index.html`, validated by `html-validate` in `ci-build`.

## What runs

| Workflow | When | What |
|---|---|---|
| `CI` (`ci-build`, `commitlint`) | every push to `main` and every PR | checks that `site/index.html` exists and is valid HTML; lints the would-be squash commit message |
| `Release` (`release`, `deploy`) | every push to `main`; `workflow_dispatch` with `deploy=true` | semantic-release tags `vX.Y.Z`; `deploy` publishes `site/` to GitHub Pages and uploads the `deployed-version` artifact |
| `telemetry` | when `CI` or `Release` completes | calls the shared `ulisseas/.github` ci-telemetry workflow: traces to Honeycomb and Grafana Cloud, CI metrics to Grafana Cloud Metrics |

The synthetic uptime check for the site is provisioned by the private
`portfolio-infra` repository.

## Dashboards

- [Status](https://grafana.ulisseas.com/public-dashboards/dc3e15c21b8a4b2487cdf1dbab08dd64)
- [CI](https://grafana.ulisseas.com/public-dashboards/9c7fd71e4e3d497f8e3be636275dc4a2)
- [Releases](https://grafana.ulisseas.com/public-dashboards/6d289ba972de4c94ad4b29ebbd0f924a)

## Working in the repo

Feature branch, PR with a conventional-commit title, squash-merge. `main` is
protected by the `protect-main` ruleset (`scripts/harden-repo.sh`): PR-only,
`ci-build` and `commitlint` required, linear history.

    npm install      # installs the commit-msg hook
    npm run check    # html-validate site/

## License

MIT, see [LICENSE](LICENSE).
