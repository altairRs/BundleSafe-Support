# BundleSafe

BundleSafe is an editor for Java `.properties` resource bundles. It shows one key
across its locale files and previews each file before applying the edit.

![BundleSafe locale editor](assets/bundle-grid.png)

## What it preserves

- comments, ordering, separators, and untouched escape spelling;
- UTF-8 and ISO-8859-1 files, including BOM and native-to-ASCII forms;
- CRLF, LF, mixed line endings, and missing trailing newlines;
- duplicate-key semantics used by Java.

![BundleSafe exact preview](assets/exact-preview.png)

BundleSafe works locally and does not collect usage data or send file contents
over the network.

[Report a problem](https://github.com/altairRs/BundleSafe-Support/issues) ·
[Privacy policy](PRIVACY.html) · [Security](SECURITY.html)
