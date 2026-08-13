---
title: UTF-8, BOMs, and Unicode escapes in Java properties
description: A practical guide to the encoding rules behind Java properties and resource bundles.
---

# UTF-8, BOMs, and `\uXXXX` in Java `.properties` files

There is no single encoding rule for every API that reads a `.properties` file.
The API and Java version matter.

## `Properties.load(InputStream)`

The byte-stream form of `java.util.Properties.load` treats each input byte as
ISO-8859-1. Characters outside that range are represented with Unicode escapes
such as `\u30AB`. The `Reader` overload instead uses the reader's character
encoding.

## `PropertyResourceBundle`

For Java 9 and later, a property resource bundle loaded from an input stream is
read as UTF-8 first. If the input is not valid UTF-8, the implementation falls
back to ISO-8859-1 unless configured otherwise. This is separate from the
`Properties.load(InputStream)` rule.

See the JDK documentation for
[`Properties`](https://docs.oracle.com/en/java/javase/26/docs/api/java.base/java/util/Properties.html)
and
[`PropertyResourceBundle`](https://docs.oracle.com/en/java/javase/26/docs/api/java.base/java/util/PropertyResourceBundle.html)
for the exact behavior.

## What a UTF-8 BOM does

A UTF-8 byte order mark is the three-byte prefix `EF BB BF`. It is invisible in
most editors. IntelliJ IDEA can use a BOM as an encoding signal and disables
some manual encoding choices when a file already declares its encoding this
way. IntelliJ normally creates UTF-8 files without a BOM, but its behavior is
configurable in [File Encodings](https://www.jetbrains.com/help/idea/settings-file-encodings.html).

Do not add or remove a BOM incidentally while changing a value. Whether the
runtime accepts it depends on how the file is loaded, and the byte change also
creates an unrelated diff.

## Choosing a project convention

Pick the convention used by the code that actually loads the file:

- legacy `Properties.load(InputStream)`: ISO-8859-1 plus escapes, or change the
  code to use an explicitly encoded `Reader`;
- modern `ResourceBundle`: UTF-8 is supported, subject to its documented
  fallback behavior;
- build-time conversion: keep the source encoding and conversion step explicit
  in the build.

Then configure the IDE to match and commit that configuration where practical.

[BundleSafe](https://plugins.jetbrains.com/plugin/33509-bundlesafe) detects the
file representation before editing and preserves an existing UTF-8 BOM and
untouched `\uXXXX` spelling. Its preview shows the raw region that will change.

[Back to BundleSafe](index.html)
