# REPO_ESSENTIALS.md

Purpose: a language-agnostic checklist anyone can follows to turn any project into a complete, credible GitHub repository. Works for Python, Rust, JS/TS, Go, HTML/CSS, React, and anything else.

## 0. Instructions for the AI agent

1. Read this whole file once, then work through sections 1 to 13 in order.
2. Adapt, do not copy blindly. Replace every `<PLACEHOLDER>` with real project values. Never leave a placeholder in a committed file.
3. Do not invent facts: no fake badges, fake contact emails, fake benchmarks, or fake version history. If a value is unknown, ask the user or omit the item.
4. Keep files short and specific to this project. A 40-line accurate README beats a 400-line generic one.
5. No emojis in any file you generate unless the user asks.
6. Pin third-party GitHub Actions to a full commit SHA (with a version comment) where practical, and give each workflow least-privilege `permissions`.
7. When finished, run the checklist in section 14 and report what was done and what was skipped.

---

## 1. Target layout

```text
.
├── README.md
├── LICENSE
├── CHANGELOG.md
├── ROADMAP.md
├── CONTRIBUTING.md
├── CODE_OF_CONDUCT.md
├── SECURITY.md
├── SUPPORT.md                 (optional)
├── AGENTS.md                  (instructions for AI coding agents)
├── CITATION.cff               (optional, research or academic use)
├── .gitignore
├── .gitattributes
├── .editorconfig
├── .github/
│   ├── CODEOWNERS
│   ├── FUNDING.yml            (optional)
│   ├── dependabot.yml
│   ├── PULL_REQUEST_TEMPLATE.md
│   ├── ISSUE_TEMPLATE/
│   │   ├── bug_report.yml
│   │   ├── feature_request.yml
│   │   └── config.yml
│   └── workflows/
│       ├── ci.yml
│       ├── codeql.yml
│       ├── scorecard.yml
│       └── release.yml
├── docs/
│   ├── architecture.md
│   ├── adr/0001-record-architecture-decisions.md
│   └── ...
├── examples/                  (runnable usage samples)
├── scripts/                   (dev helper scripts)
└── src/ and tests/            (or the language-native equivalents)
```

GitHub looks for community health files in the repo root, `.github/`, or `docs/`. Putting defaults in a public repo named `.github` on your account applies them to every repo that lacks its own copy.
Reference: https://docs.github.com/en/communities/setting-up-your-project-for-healthy-contributions

---

## 2. Root files: what each one must contain

| File | Must contain | Resource |
|---|---|---|
| README.md | What it is, why, install, quick start, usage, links to docs, license | https://www.makeareadme.com/ |
| LICENSE | Full text of one OSI-approved license, correct year and holder | https://choosealicense.com/ |
| CHANGELOG.md | Human-written release history (section 3) | https://keepachangelog.com/en/1.1.0/ |
| ROADMAP.md | Planned direction, not promises (section 4) | see section 4 |
| CONTRIBUTING.md | Setup, test, lint, commit style, PR process | https://docs.github.com/en/communities/setting-up-your-project-for-healthy-contributions/setting-guidelines-for-repository-contributors |
| CODE_OF_CONDUCT.md | Adopt Contributor Covenant unmodified, add a real contact | https://www.contributor-covenant.org/ |
| SECURITY.md | Supported versions, private reporting channel, response expectations | https://docs.github.com/en/code-security/getting-started/adding-a-security-policy-to-your-repository |
| AGENTS.md | Build, test, lint commands and conventions for AI agents | https://agents.md/ |
| CITATION.cff | Citation metadata (optional) | https://citation-file-format.github.io/ |
| .github/CODEOWNERS | Path-to-owner mapping for review requests | https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/customizing-your-repository/about-code-owners |
| .github/FUNDING.yml | Sponsor links (optional) | https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/customizing-your-repository/displaying-a-sponsor-button-in-your-repository |

License picker notes: MIT or Apache-2.0 for permissive, GPL-3.0 or AGPL-3.0 for copyleft, MPL-2.0 for file-level copyleft. Use SPDX identifiers (https://spdx.org/licenses/) in package manifests. For per-file license compliance across a repo see https://reuse.software/.

### README skeleton

Sections in order: title and one-line description, badges (max 5 to 6), screenshot or demo (if visual), features (short list), install, quick start (copy-pasteable), usage and configuration, documentation link, roadmap link, contributing link, license, acknowledgements.

Rules: every command in the README must actually run; show the expected output for the quick start; link to `CHANGELOG.md` and `ROADMAP.md`; add alt text to every image.

Good references: https://github.com/matiassingers/awesome-readme and https://github.com/othneildrew/Best-README-Template

### AGENTS.md skeleton

Sections: project overview (2 to 3 lines), setup command, test command, lint and format command, code style rules, directory map, things never to do (for example, never commit secrets, never edit generated files), commit and PR conventions. Keep under about 100 lines. Symlink or duplicate as `CLAUDE.md` only if a tool requires it.

---

## 3. CHANGELOG (high priority)

Format: Keep a Changelog 1.1.0. Versioning: Semantic Versioning 2.0.0.
- https://keepachangelog.com/en/1.1.0/
- https://semver.org/spec/v2.0.0.html

Rules:
1. The file is `CHANGELOG.md` in the repo root.
2. Newest release first. Always keep an `## [Unreleased]` section at the top.
3. Every version has an ISO date (YYYY-MM-DD).
4. Group entries under only these headings: Added, Changed, Deprecated, Removed, Fixed, Security.
5. Write for humans: describe user-visible impact, not commit hashes. Link to the PR or issue.
6. Mark breaking changes explicitly with the word "BREAKING" at the start of the entry.
7. Add comparison links at the bottom so each version heading links to a GitHub compare view.
8. Do not paste raw `git log` output.

Template:

```markdown
# Changelog

All notable changes to this project are documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

### Added
- <new feature> (#<PR>)

## [1.0.0] - YYYY-MM-DD

### Added
- Initial public release.

[Unreleased]: https://github.com/<OWNER>/<REPO>/compare/v1.0.0...HEAD
[1.0.0]: https://github.com/<OWNER>/<REPO>/releases/tag/v1.0.0
```

Starting out: use `0.1.0` for the first tagged release. Move to `1.0.0` when the public API is stable.

Automation options (pick one, do not stack them):

| Tool | Best for | Link |
|---|---|---|
| release-please | Any language. Opens a Release PR that bumps version, updates CHANGELOG.md, and tags on merge | https://github.com/googleapis/release-please |
| release-please GitHub Action | Easiest way to run release-please | https://github.com/googleapis/release-please-action |
| semantic-release | Fully automated publish flow, JS-centric | https://github.com/semantic-release/semantic-release |
| git-cliff | Highly configurable changelog generator, Rust binary, any language | https://github.com/orhun/git-cliff |
| conventional-changelog | One-shot generation from commits | https://github.com/conventional-changelog/conventional-changelog |
| Changesets | JS and monorepos, contributor-written change files | https://github.com/changesets/changesets |
| towncrier | Python news fragments | https://github.com/twisted/towncrier |

Note: release-please finalizes a fresh section per release rather than accumulating under `[Unreleased]`. If you want the strict Keep a Changelog style with a manual `[Unreleased]`, maintain it by hand or use git-cliff.

---

## 4. ROADMAP (high priority)

Two layers, both small:

1. `ROADMAP.md` in the repo: the public, durable direction.
2. GitHub Projects board (Roadmap layout) or milestones: the live task tracking. https://docs.github.com/en/issues/planning-and-tracking-with-projects

Rules:
1. Use horizon buckets (Now, Next, Later) instead of calendar dates. Dates create false promises.
2. State the project goal in two sentences at the top.
3. Every item links to an issue or discussion when one exists.
4. Include a Non-goals section. It prevents scope creep and unwanted PRs.
5. Add a line saying the roadmap is a statement of intent, not a commitment.
6. Review at every minor release and move shipped items into `CHANGELOG.md`.

Template:

```markdown
# Roadmap

Goal: <what this project is for, in two sentences>.

This roadmap is a statement of intent, not a commitment. Priorities change.
Shipped work is recorded in [CHANGELOG.md](CHANGELOG.md).

## Now (in progress)
- [ ] <item> (#<issue>)

## Next (planned, not started)
- [ ] <item> (#<issue>)

## Later (ideas under consideration)
- <item>

## Non-goals
- <thing this project will not do, and why>

## How to influence the roadmap
Open a feature request using the issue template or start a Discussion.
```

Related planning artifacts worth adding when the project grows:
- Architecture Decision Records in `docs/adr/` (https://adr.github.io/ and the MADR template at https://adr.github.io/madr/)
- GitHub Discussions for open-ended proposals
- Milestones named after the target version, such as `v0.2.0`

---

## 5. Versioning, commits, releases

Commit messages: Conventional Commits 1.0.0 (https://www.conventionalcommits.org/en/v1.0.0/)

```text
<type>(<optional scope>): <short imperative summary>

<optional body>

<optional footer, for example: BREAKING CHANGE: ... or Closes #123>
```

Types: feat (minor bump), fix (patch bump), docs, style, refactor, perf, test, build, ci, chore. A `!` after the type or a `BREAKING CHANGE:` footer means a major bump.

Enforce on PR titles (squash-merge workflow) with https://github.com/amannn/action-semantic-pull-request. Enforce locally with https://github.com/conventional-changelog/commitlint if the project uses Node tooling.

Branching: trunk-based with short-lived feature branches. Default branch is `main`. Protect it (section 12).

Tags and releases:
- Tags are `vMAJOR.MINOR.PATCH`.
- Every tag gets a GitHub Release. Release notes come from the matching CHANGELOG section.
- Attach built artifacts and checksums to the release.
- Generated release notes config: https://docs.github.com/en/repositories/releasing-projects-on-github/automatically-generated-release-notes

---

## 6. GitHub templates

### Issue forms (`.github/ISSUE_TEMPLATE/bug_report.yml`)

Syntax reference: https://docs.github.com/en/communities/using-templates-to-encourage-useful-issues-and-pull-requests/syntax-for-issue-forms

```yaml
name: Bug report
description: Report something that is not working
labels: ["bug", "triage"]
body:
  - type: textarea
    id: what-happened
    attributes:
      label: What happened?
      description: What did you expect, and what happened instead?
    validations:
      required: true
  - type: textarea
    id: repro
    attributes:
      label: Steps to reproduce
    validations:
      required: true
  - type: input
    id: version
    attributes:
      label: Version
      placeholder: "1.2.3 or commit SHA"
    validations:
      required: true
  - type: input
    id: environment
    attributes:
      label: OS and runtime
```

Also add `feature_request.yml` (problem, proposed solution, alternatives) and `config.yml` to disable blank issues and link to Discussions and SECURITY.md:

```yaml
blank_issues_enabled: false
contact_links:
  - name: Questions and ideas
    url: https://github.com/<OWNER>/<REPO>/discussions
    about: Ask questions here instead of opening an issue
```

### Pull request template (`.github/PULL_REQUEST_TEMPLATE.md`)

```markdown
## Summary
<what and why>

## Changes
-

## Testing
<how this was verified>

## Checklist
- [ ] Tests added or updated
- [ ] Docs updated
- [ ] CHANGELOG.md updated under [Unreleased] (if user-visible)
- [ ] No secrets or large binaries committed
```

### SECURITY.md essentials

Include: supported versions table, how to report (prefer GitHub Private Vulnerability Reporting, plus an email), what to include in a report, expected acknowledgement and fix timelines, disclosure policy. Do not ask reporters to open public issues.
Example of a well-formed real policy: https://github.com/electron/electron/blob/main/SECURITY.md

---

## 7. .gitignore, .gitattributes, .editorconfig

.gitignore sources (use these instead of writing from memory):
- Official templates: https://github.com/github/gitignore
- Generator (combine many stacks, for example OS + editor + language): https://www.toptal.com/developers/gitignore
- API for scripts: `https://www.toptal.com/developers/gitignore/api/python,node,rust,go,macos,windows,linux,visualstudiocode`
- GitHub API list of templates: https://api.github.com/gitignore/templates

Always ignore: dependency folders, build output, virtual environments, `.env` and other secret files, OS junk (`.DS_Store`, `Thumbs.db`), editor folders, logs, coverage output, local databases. Commit an `.env.example` with fake values and keys only.

Language quick reference:

| Language | Ignore |
|---|---|
| Python | `__pycache__/`, `.venv/`, `*.egg-info/`, `dist/`, `.pytest_cache/`, `.ruff_cache/` |
| Rust | `target/` (commit `Cargo.lock` for binaries and apps) |
| JS/TS | `node_modules/`, `dist/`, `.next/`, `coverage/` (commit the lockfile) |
| Go | binaries, `vendor/` only if not vendoring, `*.test`, `*.out` |
| HTML/CSS | `node_modules/`, `dist/`, `.cache/` |

.gitattributes: normalize line endings and mark generated files.

```text
* text=auto eol=lf
*.png binary
*.jpg binary
package-lock.json linguist-generated
```

.editorconfig (https://editorconfig.org/):

```ini
root = true

[*]
charset = utf-8
end_of_line = lf
insert_final_newline = true
trim_trailing_whitespace = true
indent_style = space
indent_size = 2

[*.{py,rs}]
indent_size = 4

[Makefile]
indent_style = tab

[*.go]
indent_style = tab
```

---

## 8. Workflows (GitHub Actions)

Docs: https://docs.github.com/en/actions
Starter workflows by language: https://github.com/actions/starter-workflows
Lint your workflows: https://github.com/rhysd/actionlint (syntax) and https://github.com/woodruffw/zizmor (security)

### ci.yml (adapt the steps per language)

```yaml
name: CI
on:
  push:
    branches: [main]
  pull_request:

permissions:
  contents: read

concurrency:
  group: ci-${{ github.ref }}
  cancel-in-progress: true

jobs:
  test:
    runs-on: ${{ matrix.os }}
    strategy:
      fail-fast: false
      matrix:
        os: [ubuntu-latest, windows-latest, macos-latest]
    steps:
      - uses: actions/checkout@<PIN_SHA> # vX
      # Language setup step goes here (setup-python, setup-node, rust-toolchain, setup-go)
      # Install dependencies with a cache
      # Lint
      # Test
      # Build
```

Language setup actions: actions/setup-python, actions/setup-node, actions/setup-go, dtolnay/rust-toolchain, astral-sh/setup-uv, Swatinem/rust-cache.

### dependabot.yml

Reference: https://docs.github.com/en/code-security/dependabot/working-with-dependabot/dependabot-options-reference

```yaml
version: 2
updates:
  - package-ecosystem: "github-actions"
    directory: "/"
    schedule:
      interval: "weekly"
  # Add the ecosystems this repo uses: npm, pip, cargo, gomod, docker, etc.
  - package-ecosystem: "<ECOSYSTEM>"
    directory: "/"
    schedule:
      interval: "weekly"
    groups:
      minor-and-patch:
        update-types: ["minor", "patch"]
```

Alternative: Renovate (https://github.com/renovatebot/renovate) with a `renovate.json`.

### codeql.yml

Code scanning docs: https://docs.github.com/en/code-security/code-scanning
Simplest path: Settings, Code security, enable CodeQL default setup. Use a custom `codeql.yml` only if you need extra query suites, and disable default setup first or the two conflict. CodeQL covers JS/TS, Python, Go, Java, C/C++, C#, Ruby, Swift, and Rust support is evolving, so check the current language list in the docs.

### scorecard.yml

OpenSSF Scorecard workflow: https://github.com/ossf/scorecard-action
Dashboard and checks: https://scorecard.dev/

### release.yml

Choose one:
1. release-please (section 3): runs on push to `main`, opens and merges Release PRs.
2. Tag-triggered: on push of `v*` tag, build, test, publish, create a GitHub Release.

Language-specific release tooling:
- Go: GoReleaser https://goreleaser.com/
- Rust: cargo-dist https://github.com/axodotdev/cargo-dist
- Python: PyPI trusted publishing https://docs.pypi.org/trusted-publishers/ and https://github.com/pypa/gh-action-pypi-publish
- JS: npm provenance https://docs.npmjs.com/generating-provenance-statements and Changesets
- Any: build provenance attestations https://github.com/actions/attest-build-provenance

Other useful workflows: stale issue handling (actions/stale), link checking for docs (https://github.com/lycheeverse/lychee-action), PR labeler (actions/labeler), dependency review on PRs (actions/dependency-review-action), docs deploy to GitHub Pages.

---

## 9. Language toolchains

Pick the row for the project. Each needs: formatter, linter, test runner, type or static check, and a one-command dev setup.

| Stack | Format and lint | Test | Other |
|---|---|---|---|
| Python | ruff (https://docs.astral.sh/ruff/) | pytest | uv for envs and locking (https://docs.astral.sh/uv/), mypy or pyright, `pyproject.toml` |
| Rust | rustfmt, clippy | `cargo test`, nextest | cargo-deny (https://github.com/EmbarkStudios/cargo-deny), cargo-audit, cargo-dist |
| JS/TS | Biome (https://biomejs.dev/) or ESLint + Prettier | Vitest or Jest | `tsconfig.json` strict mode, Changesets |
| React | ESLint with react-hooks plugin, Prettier or Biome | Vitest + Testing Library | Storybook for components (https://storybook.js.org/), Playwright for e2e (https://playwright.dev/) |
| Go | gofmt, golangci-lint (https://golangci-lint.run/) | `go test ./...` | govulncheck, GoReleaser |
| HTML/CSS | Prettier, Stylelint (https://stylelint.io/), html-validate | Playwright | Lighthouse CI (https://github.com/GoogleChrome/lighthouse-ci), axe for accessibility |

Cross-language developer experience:
- Task runner entry point so contributors run one command: Makefile, `justfile` (https://github.com/casey/just), or package scripts.
- Pre-commit hooks: https://pre-commit.com/ (any language) or https://github.com/evilmartians/lefthook (fast, single binary).
- Dev containers: `.devcontainer/devcontainer.json` (https://containers.dev/) for one-click Codespaces setup.
- Secret scanning in hooks: https://github.com/gitleaks/gitleaks

Testing and coverage: aim for tests on core logic and a CI-enforced coverage floor. Report with Codecov (https://about.codecov.io/) or Coveralls (https://coveralls.io/).

---

## 10. Documentation

Structure docs with the Diataxis framework: tutorials, how-to guides, reference, explanation. https://diataxis.fr/

Minimum docs for a serious repo:
- `docs/architecture.md`: components, data flow, one diagram.
- `docs/adr/`: one short file per significant decision.
- Reference docs generated from code (rustdoc, pydoc and Sphinx, TypeDoc, godoc).
- A configuration reference listing every option, default, and environment variable.

Diagrams in Markdown render natively on GitHub with Mermaid: https://docs.github.com/en/get-started/writing-on-github/working-with-advanced-formatting/creating-diagrams

Doc site generators (pick one only if the project needs a site):

| Tool | Good for | Link |
|---|---|---|
| MkDocs Material | Python and general docs | https://squidfunk.github.io/mkdocs-material/ |
| Docusaurus | React-based docs and blogs | https://docusaurus.io/ |
| VitePress | Vue and JS projects, fast | https://vitepress.dev/ |
| mdBook | Rust projects and books | https://rust-lang.github.io/mdBook/ |
| Sphinx | Python API docs | https://www.sphinx-doc.org/ |
| GitHub Pages | Free hosting for any static output | https://pages.github.com/ |

For HTTP APIs, publish an OpenAPI description (https://spec.openapis.org/oas/latest.html) and render it with Swagger UI (https://swagger.io/tools/swagger-ui/) or Redoc (https://github.com/Redocly/redoc).

---

## 11. Badges and repo presentation

Badge service: https://shields.io/ (static, dynamic, and endpoint badges).

Use only badges that are true and useful, placed on one line under the title. Recommended set:
- CI status (GitHub Actions workflow badge from the Actions tab)
- Latest release or package version
- License
- Coverage (only if reporting is set up)
- OpenSSF Scorecard (https://scorecard.dev/) once the workflow has run

Examples (replace placeholders):

```text
https://img.shields.io/github/v/release/<OWNER>/<REPO>
https://img.shields.io/github/license/<OWNER>/<REPO>
https://img.shields.io/github/actions/workflow/status/<OWNER>/<REPO>/ci.yml?branch=main
https://api.securityscorecards.dev/projects/github.com/<OWNER>/<REPO>/badge
```

Repo settings that affect presentation:
- Description (one line) and homepage URL
- 5 to 10 Topics for discoverability
- Social preview image, 1280 x 640 px, under Settings, General
- Pin the repo on your profile if it is a flagship project
- Releases page populated with notes and artifacts
- Contributor recognition via https://allcontributors.org/ (optional)

---

## 12. Security and supply chain

Turn these on (Settings, Code security):
1. Dependency graph and Dependabot alerts
2. Dependabot security updates
3. Secret scanning and push protection
4. Private vulnerability reporting (so the SECURITY.md link works)
5. Code scanning with CodeQL

Branch protection or Rulesets on `main`:
- Require pull requests and at least one approval (if more than one maintainer)
- Require status checks to pass (CI, CodeQL)
- Block force pushes and branch deletion
- Require linear history or squash merge, matching the commit convention
- Docs: https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-rulesets/about-rulesets

Hardening checklist:
- Workflow `permissions:` set to read-only by default, elevated per job
- Actions pinned to commit SHAs, kept current by Dependabot
- Lockfiles committed
- No secrets in code or history; rotate immediately if one leaks
- Signed or attested release artifacts: Sigstore https://www.sigstore.dev/, SLSA https://slsa.dev/
- SBOM generation for released artifacts (CycloneDX https://cyclonedx.org/ or SPDX)

Measure progress: OpenSSF Scorecard (https://scorecard.dev/) and the OpenSSF Best Practices badge (https://www.bestpractices.dev/). Quick wins on the Scorecard: SECURITY.md, a license, Dependabot, CI tests, restricted token permissions, pinned dependencies, and SAST.

---

## 13. APIs and machine-readable sources

Use these to automate repo setup or to power the README and docs:

| API | Use | URL |
|---|---|---|
| GitHub REST API | Create repos, set topics, labels, releases | https://docs.github.com/en/rest |
| GitHub GraphQL API | Projects, bulk queries | https://docs.github.com/en/graphql |
| GitHub CLI (`gh`) | Script repo creation, settings, releases from the terminal | https://cli.github.com/ |
| GitHub Licenses API | Fetch license text by SPDX key | https://api.github.com/licenses |
| GitHub gitignore API | Fetch official .gitignore templates | https://api.github.com/gitignore/templates |
| Toptal gitignore API | Combined .gitignore for many stacks | https://www.toptal.com/developers/gitignore/api/python,node |
| Shields.io | Badge images and endpoint badges | https://shields.io/ |
| OpenSSF Scorecard API | Score for a repo | https://api.securityscorecards.dev/ |
| deps.dev | Dependency and advisory data across ecosystems | https://deps.dev/ |
| OSV | Open vulnerability database API | https://osv.dev/ |
| Libraries for demo data | Free public APIs for examples and tests | https://github.com/public-apis/public-apis |

Useful `gh` one-liners:

```text
gh repo create <NAME> --public --source=. --remote=origin --push
gh repo edit --description "<DESC>" --add-topic <TOPIC> --homepage <URL>
gh release create v0.1.0 --notes-file RELEASE_NOTES.md
gh label create "good first issue" --color 7057ff
```

---

## 14. Final checklist (definition of done)

Files
- [ ] README with working install and quick start
- [ ] LICENSE matching manifest metadata
- [ ] CHANGELOG.md with `[Unreleased]` and compare links
- [ ] ROADMAP.md with Now, Next, Later, and Non-goals
- [ ] CONTRIBUTING.md, CODE_OF_CONDUCT.md, SECURITY.md
- [ ] AGENTS.md with accurate commands
- [ ] .gitignore, .gitattributes, .editorconfig
- [ ] Issue forms, PR template, CODEOWNERS

Automation
- [ ] CI runs lint, test, build on PRs
- [ ] Dependabot or Renovate configured for every ecosystem in use
- [ ] CodeQL and Scorecard running
- [ ] Release flow produces a tag, a GitHub Release, and a CHANGELOG entry
- [ ] Pre-commit hooks or a single task-runner command for local checks

Settings
- [ ] Description, topics, homepage, social preview
- [ ] Branch protection or ruleset on `main`
- [ ] Secret scanning, push protection, private vulnerability reporting enabled
- [ ] Discussions enabled (optional), issue templates link to them

Quality
- [ ] No placeholders left in any file
- [ ] All links resolve
- [ ] All badges are real and passing
- [ ] First release tagged (`v0.1.0` or later)

---

## 15. Source index

Official GitHub docs
- Community health files: https://docs.github.com/en/communities/setting-up-your-project-for-healthy-contributions
- Security features: https://docs.github.com/en/code-security
- Actions: https://docs.github.com/en/actions
- Projects: https://docs.github.com/en/issues/planning-and-tracking-with-projects
- Starter workflows: https://github.com/actions/starter-workflows
- Official gitignore templates: https://github.com/github/gitignore

Standards
- Keep a Changelog: https://keepachangelog.com/en/1.1.0/
- Semantic Versioning: https://semver.org/spec/v2.0.0.html
- Conventional Commits: https://www.conventionalcommits.org/en/v1.0.0/
- Contributor Covenant: https://www.contributor-covenant.org/
- SPDX licenses: https://spdx.org/licenses/
- REUSE: https://reuse.software/
- Diataxis: https://diataxis.fr/
- ADRs: https://adr.github.io/
- AGENTS.md: https://agents.md/
- EditorConfig: https://editorconfig.org/
- OpenAPI: https://spec.openapis.org/oas/latest.html

Release and changelog tooling
- release-please: https://github.com/googleapis/release-please
- semantic-release: https://github.com/semantic-release/semantic-release
- git-cliff: https://github.com/orhun/git-cliff
- Changesets: https://github.com/changesets/changesets
- GoReleaser: https://goreleaser.com/
- cargo-dist: https://github.com/axodotdev/cargo-dist

Security
- OpenSSF Scorecard: https://scorecard.dev/
- OpenSSF Best Practices badge: https://www.bestpractices.dev/
- Sigstore: https://www.sigstore.dev/
- SLSA: https://slsa.dev/
- gitleaks: https://github.com/gitleaks/gitleaks
- zizmor: https://github.com/woodruffw/zizmor

README and presentation
- Make a README: https://www.makeareadme.com/
- Awesome README: https://github.com/matiassingers/awesome-readme
- Best-README-Template: https://github.com/othneildrew/Best-README-Template
- Shields.io: https://shields.io/
- Choose a License: https://choosealicense.com/
- All Contributors: https://allcontributors.org/

Template repositories worth studying (read their file layout, do not copy their branding)
- A Go example pre-wired with Dependabot, CodeQL, branch protection ruleset, and Scorecard: https://github.com/smlx/go-cli-github
- Electron's SECURITY.md as a policy model: https://github.com/electron/electron/blob/main/SECURITY.md
