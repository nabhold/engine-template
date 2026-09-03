<!-- Target path: nabhold/engine-template/TEMPLATE-USAGE.md (becomes <new-repo>/TEMPLATE-USAGE.md in any repo created from this template — delete it there once followed, per its own last checklist item). -->

# Using this template

Delete this file once you've worked through the checklist below — it isn't
part of any real engine repo, only the template.

## 1. Create the repo

```bash
gh repo create nabhold/<engine-repo-name> --template nabhold/engine-template --private
```

(`--private` matches the ecosystem's default — flip it if this repo is
meant to be public from day one.)

## 2. Scaffold-only step (do this immediately, before any application code)

This is Foundation 0 — README + CODEOWNERS + branch protection only, per the
ecosystem's established scaffolding practice. Nothing below this point
requires application code to exist yet.

- [ ] Edit `README.md`: real repo name, real ADR number, real Role/Ownership/
      Contract-dependencies content. File the ADR in
      `nabhold/shared/docs/adr/` first if it doesn't exist yet.
- [ ] Edit `.github/CODEOWNERS`: replace the placeholder team with the real
      owning team, following the `@nabhold/<repo>-maintainers` convention.
      Confirm the team actually exists in the GitHub org before merging —
      CODEOWNERS must never point at a team that doesn't exist, and must
      never be left blank.
- [ ] Apply branch protection / a repository ruleset on `main`: require pull
      requests, required CODEOWNERS review, and (once Foundation CI exists —
      step 3 below) required status checks. Exact mechanism (org-level
      ruleset vs. per-repo branch protection) depends on the org's current
      GitHub plan tier — confirm with whoever administers the `nabhold` org
      if you're not sure which is available.

At this point the repo can sit scaffolded, exactly like `baobab-trade`,
`baobab-erp`, and `baobab-pulse` did before their own build-out — there's no
requirement to do the rest immediately.

## 3. Build-start step (do this when application code actually starts)

- [ ] Decide this engine's language stack and which `baobab-dev` profile
      fits it (`full`, `frontend`, `frontend-e2e`, or `infra` — see
      `nabhold/shared`'s `contracts/development-environment/schema.yaml` for
      what each profile actually includes; pick the narrowest one that
      covers this repo's real needs, not `full` by default).
- [ ] Copy `.nabhold/environment.yaml.example` to `.nabhold/environment.yaml`
      and fill in every placeholder. Confirm the current `baobab-dev`
      release tag (check its `CHANGELOG.md` / GHCR package page — don't
      assume any version number written elsewhere is still current) and use
      the same version in both `minimum_version` here and the image tag in
      `.devcontainer/devcontainer.json`.
- [ ] Fill in `.devcontainer/devcontainer.json`'s placeholders the same way,
      then delete `.nabhold/environment.yaml.example` and
      `.devcontainer/devcontainer.json`'s inline TODO comments.
- [ ] Rename `.github/workflows/foundation.yml.example` to
      `.github/workflows/foundation.yml` (drop the `.example` suffix — do
      not create a second, differently-named file). Confirm the pinned
      commit SHA it calls (`nabhold/shared/.github/workflows/
      foundation-repository-gates.yml@<sha>`) is still current — diff it
      against that file's current `main` before relying on an old pin.
- [ ] Set `dockerfile: ""` in that workflow's `with:` block if this engine
      doesn't build a container image yet (the reusable workflow's default
      assumes a Dockerfile exists at the repo root); update it once one
      does.
- [ ] Once CI is green on a real PR, go back and add "Foundation CI" as a
      required status check on `main`'s branch protection from step 2.
- [ ] Fill in `contracts/README.md` with the actual contract(s) this engine
      consumes or publishes, or delete it if none apply yet.
- [ ] Delete this file.
