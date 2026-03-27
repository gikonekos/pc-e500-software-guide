# LZH Archives

Many PC-E500 series software packages are distributed in older archive formats such as LZH.

For new users, this can be one of the first barriers to actually trying archived software.

## What LZH is

LZH is an older archive format that was widely used in Japan.

Many historical PC-E500 software packages were distributed in this format.

## Why this matters

If you find an original archive and cannot open it easily on a modern PC,
you may incorrectly assume that the software is unusable.

In many cases, the problem is simply that you need an LZH-capable extraction tool.

## Practical advice

Use an archive tool that can open older Japanese LZH archives.

Reference link:

- [WinZip: How to Open LZH Files](https://www.winzip.com/en/learn/file-formats/lzh/?srsltid=AfmBOorKl9I1lBfE1cJd9VXMpqZy4Yh5RGRUUQcN_hAByAwqCcgv2TfB)

One practical example used by this repository owner is:

- [Lhasa (Japanese Wikipedia)](https://ja.wikipedia.org/wiki/Lhasa)

On modern Windows systems, support may vary depending on the tool and environment.

## Important note about text encoding

Even after extracting files successfully, you may still see garbled text if the
character encoding is handled incorrectly.

For practical PC-E500 workflows, CP932-compatible handling is often safer than UTF-8,
especially when older Japanese text files or half-width kana are involved.

In other words:

- extraction success does not always mean text will display correctly
- UTF-8 is not always safe for older Japanese PC-E500 files
- CP932-compatible handling is often the more practical choice

## Also remember

Depending on the original package, you may encounter:

- old text files
- BASIC source files
- machine-language related files
- documentation written for older Japanese PC environments

## Related pages

- [Transfer Tools](transfer-tools.md)
- [Redistribution Policy](redistribution-policy.md)
