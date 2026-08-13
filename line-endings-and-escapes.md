---
title: Why properties-file line endings and escapes change
description: Diagnose noisy Java properties diffs caused by line endings, encodings, and native-to-ASCII conversion.
---

# Why line endings and escapes change in `.properties` files

Suppose you change one translation and Git reports that most of the file was
modified. Two common causes are line-ending conversion and escape conversion.

## Line endings

Text files can use LF, CRLF, or, occasionally, a mixture left by earlier edits.
A writer that emits the current operating system's default line separator can
turn a one-value change into a whole-file diff.

Check the repository rules before blaming the editor:

- inspect `.gitattributes` for `text` and `eol` settings;
- compare the file with whitespace changes visible;
- check whether the file ended with a newline before the edit; and
- avoid applying a whole-file formatter just to change one translation.

If line-ending normalization is intentional, make it a separate commit. It is
then easy to review and does not hide the translation change.

## Escape spelling

Properties values may contain literal characters or escaped forms. For example,
the following spellings can represent the same character in the relevant
encoding mode:

```properties
currency = €
currency = \u20AC
```

IntelliJ IDEA has a **Transparent native-to-ascii conversion** option for
properties files. When enabled, the editor can display native characters while
the file contains `\uXXXX` escapes. IntelliJ documents the setting under
[File Encodings](https://www.jetbrains.com/help/idea/settings-file-encodings.html).

Escape conversion is not necessarily corruption. It is still worth noticing:
rewriting untouched escapes makes reviews harder, and changing a backslash near
a continuation or separator can alter the parsed value.

## Keep the diff reviewable

For a small translation edit, preview the raw file change rather than only the
decoded value. Confirm that unrelated lines and their escape spelling remain
unchanged. [BundleSafe](https://plugins.jetbrains.com/plugin/33509-bundlesafe)
provides this preview for one key across a Java resource-bundle family and
applies the accepted changes transactionally.

[Back to BundleSafe](index.html)
