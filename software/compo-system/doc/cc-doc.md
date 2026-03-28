# CC Documentation (English Translation)

> **Original document:** documentation included with `CC Version 1.00`  
> **Original language:** Japanese  
> **Program:** `Program for using COMPO-System on E650 / U6000`  
> **Copyright holder:** `TORO / 高橋 良和`  
> **Translation status:** English translation prepared for reference and accessibility  
> **Translation note:** This file is an English translation of the original Japanese documentation.  
> The original archive contents, copyright status, and redistribution conditions are not changed by this translation.  
> Personal names are kept in their original Japanese form where no reliable reading or official English spelling is confirmed.

------------------------------------------------------------------------------
 Program for using COMPO-System on E650 / U6000     Version 1.00

                                   (C)TORO 1994
------------------------------------------------------------------------------

COMPO-System runs on the E650 and U6000 (or should), but because it depends
on History, it could not be used on the E650 or U6000.

This program allows COMPO-System to be used without going through History.

---

## Operating environment

- Runs on the E500 series with 32K or more
- Uses `0BFDC0H` to `0BFDE8H`

---

## Usage

- First, make COMPO resident.  
  (Refer to the COMPO manual. History does not need to be made resident.)

- Load `CC`.

      >LOAD M "CC"

- Then use COMPO as follows:

      >CALL &BFDC0"COMPO command

  Example: edit `BD000H`

      >CALL &BFDC0"EBD000

- This does not affect COMPO's own functionality.

---

## Final notes

- This is freeware. Copyright belongs to `TORO / 高橋 良和`.
- We assume no responsibility whatsoever for any profit, loss, or damage
  arising from use of this program.
- Modification is freely permitted for personal use.
- If redistributing this program elsewhere, do not modify it; redistribute it
  in the same contents as obtained.
- Please contact the author if you wish to use it for commercial purposes.

---

## Contact

`TORO / 高橋 良和`

- `POCKET通信 Ver.3` … `3054`
- `NIFTY-Serve` … `GHE00667`

---

## Source and status

- **Source document:** documentation included with `CC Version 1.00`
- **Source program version:** `Version 1.00`
- **Translation status:** complete draft translation
- **Formatting:** adapted for Markdown / GitHub readability
- **Content policy:** no functional changes intended; wording has been translated,
  but original meaning should take precedence if any ambiguity remains

## Notes

- This English translation is intended as a practical reference for users who
  cannot easily read the original Japanese document.
- Archive contents, program behavior, and redistribution conditions should be
  understood according to the original package and original author statement.
- Where technical nuance is uncertain, the original Japanese wording should be
  treated as authoritative.
