---
title: Editing Java properties without rewriting the file
description: Why a one-value edit can produce a large properties-file diff, and how to avoid it.
---

# Editing one value without rewriting a `.properties` file

A Java properties file is not just a map written one entry per line. These two
entries load to the same key and value:

```properties
app.title = Coffee House
app.title:Coffee House
```

They are not the same file. The separator and surrounding whitespace differ.
Comments, blank lines, entry order, escape spelling, continuations, and line
endings can differ too.

That distinction matters when a tool reads the whole file into a `Properties`
object and writes it back. The resulting values may be correct while the diff
also contains reordered entries, normalized separators, rewritten escapes, or
new line endings. `Properties.store` is useful for producing a valid properties
file; it is not an API for preserving the original layout.

## A safer check for a one-value edit

Before applying an edit, compare the old and new bytes and ask:

1. Does the diff touch only the selected value?
2. Are comments, blank lines, and entry order unchanged?
3. Is the original separator (`=`, `:`, or whitespace) still present?
4. Are unrelated escape sequences spelled exactly as before?
5. Did the line-ending style and final-newline state stay unchanged?

Continuation lines need extra care. A logical value can span several physical
lines, and an odd run of backslashes before a line ending continues the value.
Leading whitespace on the following physical line is then ignored while Java
parses it. Rebuilding that entry without accounting for the raw layout can
change either its formatting or its meaning.

The format rules are specified in
[`java.util.Properties`](https://docs.oracle.com/en/java/javase/26/docs/api/java.base/java/util/Properties.html).

## How BundleSafe handles it

[BundleSafe](https://plugins.jetbrains.com/plugin/33509-bundlesafe) shows the
exact affected region before writing and replaces the selected value rather
than serializing the complete file. Unrelated bytes remain untouched. For a
bundle-family edit, all affected files are applied as one transaction.

[Back to BundleSafe](index.html)
