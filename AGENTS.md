# AGENTS.md

## Cursor Cloud specific instructions

This repository is the **Adobe React Spectrum** monorepo: a collection of React UI
component libraries (React Spectrum v3, React Spectrum S2, React Aria, React Aria
Components, React Stately, Internationalized) plus dev tooling. There is **no backend,
database, or external service** — "running the product" means running Storybook and/or
the docs sites, and validation means running the Jest test suite.

### Runtime / environment

- **Node 24 is a hard requirement.** Building with the wrong Node version causes native
  Parcel/Make segfaults (see `CONTRIBUTING.md` Q&A). Node 24 is installed via `nvm` and is
  made the default for login/interactive shells via a one-time `~/.bashrc` edit.
- Non-login/non-interactive shells on this VM may resolve a different `node` (`/exec-daemon/node`,
  v22) that appears earlier in `PATH`. If you hit a Node-version error, prepend Node 24 explicitly:
  `export PATH="$(echo $HOME/.nvm/versions/node/v24*/bin | cut -d' ' -f1):$PATH"`.
- Package manager is **Yarn 4** (Berry, via Corepack). The update script runs `yarn install`,
  whose `postinstall` runs `patch-package` and `yarn build:icons` (generates thousands of icon
  files under `packages/@spectrum-icons/*`). This can take a bit; it is normal.

### Running / building / testing (commands live in root `package.json`)

- **Run (primary dev env):** `yarn start` → Storybook on http://localhost:9003. First start runs a
  Parcel build and takes ~30-60s before port 9003 serves a 200. There is also an auxiliary Parcel
  server on port 3000.
- Other dev servers (optional): `yarn start:s2` (S2 Storybook, :6006), `yarn start:s2-docs`
  (docs site, :1234), `yarn start:docs` (legacy docs, :1234).
- **Test:** `yarn test` runs the full Jest suite (`STRICT_MODE=1 VIRT_ON=1`). For a focused run,
  pass a file path, e.g. `yarn jest packages/react-aria-components/test/Button.test.js`.
  Note: component tests are NOT colocated with each package's `src`; the shared suites live under
  `packages/@adobe/react-spectrum/test/` and `packages/react-aria-components/test/`.
- **Lint:** `yarn lint` runs format check + type check (`tsgo`) + `oxlint` + package lint +
  constraints concurrently. `yarn lint` (full) includes a monorepo-wide type check which is slow;
  for a quick lint-only pass use `yarn oxlint packages`.
- **Build (production libraries):** `yarn build` (`make build`). Type-error warnings during the
  Parcel types build are expected per `CONTRIBUTING.md` and it still completes successfully.

### Gotchas

- If `yarn start` fails with an export-not-found error after a prior `yarn build`, run
  `make clean_all && yarn` (build artifacts shadow source). A stale `.parcel-cache` can also cause
  odd errors — delete it.
