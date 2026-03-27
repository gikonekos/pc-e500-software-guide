# Transfer Tools

This page introduces the PC-side tools and connection hardware commonly needed to
move software to a SHARP PC-E500 series system.

## Core idea

To make archived software actually usable, you need more than just download links.
You also need a workable transfer path from a modern PC to the pocket computer.

## Recommended focus

For this repository, the most important combination is:

- PLINK on the pocket computer side
- a PC-side communication or server tool
- suitable serial connection hardware

## PC-side software

For simple serial communication and text transfer, one practical PC-side tool is
[Tera Term](https://teratermproject.github.io/index-en.html).

For binary-oriented workflows and more convenient file transfer, PLINK is one of
the most important tools in the PC-E500 ecosystem.

A commonly referenced Windows-side server for Pocket Link System is
APLINKS for Win32.

In other words:

- Tera Term is useful for basic serial communication and text transfer
- PLINK is especially important for binary file transfer and practical software use

## Why this matters

Without a usable transfer environment, many archived software packages are difficult
for new users to try in practice.

A working transfer setup turns software discovery into actual use.

## Connection hardware

Depending on your setup, you may need a dedicated serial cable or similar connection hardware.

One example source for PC-E500 series connection hardware is:

- [Takamatsu Factory: PC-E500 / PC-E650 series products](https://tmfg.jp/products/list?category_id=17)

Please check compatibility, connector type, and current availability before purchase.

In Japan, related items may also sometimes appear through secondary markets.

## Text encoding

In practical transfer workflows, text encoding can become a real problem even when
the file transfer itself appears to succeed.

For many older Japanese PC-E500 files, CP932-compatible handling is often safer
than UTF-8.

This is especially important when:

- older Japanese text files are involved
- half-width kana are used
- text looks correct on one side but becomes garbled after transfer

In practice:

- successful transfer does not always mean correct text display
- UTF-8 may cause garbled text in older Japanese workflows
- CP932-compatible handling is often the safer choice

## Also remember

Depending on the software and workflow, you may also need:

- archive extraction tools for LZH files
- machine-side initialization steps
- attention to disconnect procedures

## Related pages

- [PLINK](../tools/plink/README.md)
- [Redistribution Policy](redistribution-policy.md)
