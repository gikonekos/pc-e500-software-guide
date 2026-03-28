# PLINK

**PLINK** is one of the **most important tools** for getting started with software on the
**SHARP PC-E500 series**.

In practical terms, it provides a way to connect a PC and a pocket computer over
a serial link and use a virtual drive from the pocket computer side.

This makes software transfer much easier than relying only on simple text transfer.

## Why it matters

For many users, PLINK is the point where the PC-E500 becomes much more practical.

It helps with:

- moving files more easily
- working with software distributed on a PC
- handling binary-oriented workflows more safely
- reducing friction when trying archived software

## What you need

Typical requirements include:

- a SHARP PC-E500 series machine
- a serial connection method
- a compatible PLINK driver on the pocket computer
- a PC-side server tool

## Windows-side use

For Windows-based use, a practical setup often includes:

- PLINK or PLINKC on the pocket computer side
- APLINKS for Win32 on the PC side

For simple serial communication and text transfer, Tera Term can also be useful,
but PLINK is especially important for binary file transfer and practical software use.

## Important note

Before disconnecting, always make sure the virtual drive contents are properly handled
according to the tool’s procedure.

## Original distribution

For Windows-based use, the most important original components are:

- [APLINKS for Win32 / Pocket Link System overview](https://yuuichiakagawa.github.io/pocketcom/plink/)
- [PLINKC reference and setup notes](https://ht-deko.com/pce500/#04_01)

Please check the original pages for details, compatibility, and usage notes.

This repository does **not** redistribute these original packages.

## Documentation

English translations of bundled documentation:

- [PLINK.SYS Ver. 1.04 documentation (English translation)](doc/plink104-doc-en.md)  
  Main documentation for the PLINK.SYS device driver, including installation,
  communication requirements, virtual drive usage, abnormal-condition handling,
  and technical notes about the block-device implementation.

## Related pages

- [Getting Started](../../docs/getting-started.md)
- [Transfer Tools](../../docs/transfer-tools.md)
- [LZH Archives](../../docs/lzh.md)
- [Redistribution Policy](../../docs/redistribution-policy.md)
