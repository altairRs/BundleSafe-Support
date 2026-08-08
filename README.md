# BundleSafe

BundleSafe edits one Java `.properties` key across its locale files and shows the
exact per-file change before anything is written.

It preserves comments, ordering, separators, escapes, byte-order marks, encodings,
and line endings outside the selected values. Multi-file changes are applied as
one transaction and rolled back if a write fails.

![Edit one key across its locale files](assets/bundle-grid.png)

![Review every byte-level change before applying](assets/exact-preview.png)

## Getting help

Search the [existing issues](https://github.com/altairRs/BundleSafe-Support/issues)
first. If the problem has not been reported, open a bug report and include the
BundleSafe version, IDE build, operating system, exact error, and minimal steps to
reproduce it.

Do not post proprietary translations, credentials, customer data, or an entire
private project. Use invented values in a reduced reproduction.

- [Report a bug](https://github.com/altairRs/BundleSafe-Support/issues/new?template=bug.yml)
- [Request a feature](https://github.com/altairRs/BundleSafe-Support/issues/new?template=feature.yml)
- [Privacy policy](PRIVACY.md)
- [Security policy](SECURITY.md)

The BundleSafe source repository is private. This repository contains public
documentation and issue tracking only.

