---
name: FCA-SIFU npm publishing
description: Quirks encountered publishing/using the fca-sifu FCA library via npmjs under the myb-sifat account.
---

- npm permanently blocks reusing a version number once it has ever been published+unpublished for a package name — `npm view` may show 404 "Unpublished" while `npm publish` still rejects those same version numbers with "Cannot publish over previously published version". Bump to a clearly new version (e.g. next major) rather than retrying the same/next patch version.
- To switch a project from a local `file:` path dependency to the npm-published version while keeping the local source folder in the repo (for reference/backup), update the dependency version range in `package.json`, delete the local `node_modules/<pkg>` symlink, and reinstall with `npm install <pkg>@<version> --no-save` (or normal install) so npm fetches the real tarball instead of relinking to the local folder.
- **Why:** avoids confusion/build failures from retrying blocked version numbers, and ensures runtime actually loads from the registry copy (verify via stack trace paths pointing into `node_modules/<pkg>/...` rather than a symlink target) instead of silently still using the local folder.
