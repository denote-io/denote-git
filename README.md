# Git vault versioning

Tracks a vault or project with local Git commits in a host-rendered source control view.

This is an optional [Denote](https://github.com/denote-io/denote) plugin. Its
user guide is [guide.md](guide.md); Denote shows the same guide in
**Settings → Plugins**.

The trusted native Git transport, credential handling, and process
boundaries stay in the Denote host (`src-tauri/src/plugins/git/`). This
repository contains the downloadable plugin that drives them through the
plugin API.

## Install

Enable **Git vault versioning** from **Settings → Plugins** in Denote. Denote
downloads the release archive pinned in its built-in catalog, verifies its size
and SHA-256 digest, and only then loads the plugin in an isolated worker.

## Development

Use the Node.js version in `.node-version`.

```bash
npm ci
npm run typecheck
npm test
npm run check     # manifest, guide, and import boundaries
npm run package   # build, verify, and write .plugin-artifacts/denote.git-<version>.tgz
```

`npm run dev` rebuilds `.plugin-dev/denote.git.tgz` on every change. Load it
in Denote Development from **Settings → Plugins → Load local plugin archive**.
Disable the local plugin before loading rebuilt bytes with the same ID.

The build uses the [Denote plugin SDK](https://github.com/denote-io/denote-plugin-sdk)
and its `denote-plugin` command. It produces a single self-contained worker
bundle and a reproducible archive with fixed file modes, no timestamps, and LF
text. The pinned Vite and Rolldown versions keep archive bytes stable.

## Release

Published versions are immutable. To release a change:

1. Bump `version` in both `plugin.json` and `package.json`.
2. Merge to `main` with CI passing.
3. Run the **Release** workflow. It builds, verifies, and publishes
   `denote.git-<version>.tgz` as the `v<version>` release asset.
4. Update the plugin's entry in Denote's catalog to the new release so the
   application offers it.

## History

This plugin was developed in `plugins/git` of
[mbianchidev/denote](https://github.com/mbianchidev/denote), whose history for
this directory is preserved here. `releases.json` records every version
published from that repository, with its source commit there, original asset
URL, size, and SHA-256 digest.

[v0.10.2](https://github.com/denote-io/denote-git/releases/tag/v0.10.2) republishes the identical archive Denote
already offers (`a32d1534f024c58ebe235b06d049b103a35a2d09ff20700236d586cef0271585`, 73885 bytes),
built from source commit `75937a627d157f7698703485a5cb686cab9bb576` in
mbianchidev/denote. The source in this repository is unchanged from that commit.

CI rebuilds that version and checks it matches the published archive byte for
byte, so a source change cannot ship without a version bump.

## License

[MIT](LICENSE)
