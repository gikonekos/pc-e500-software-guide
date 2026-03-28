# History Documentation (English Translation)

> **Original document:** documentation included with `History Version 1.11`  
> **Original language:** Japanese  
> **Program:** `History Version 1.11`  
> **Copyright holder:** `TORO / 高橋 良和`  
> **Translation status:** English translation prepared for reference and accessibility  
> **Important compatibility note:** `E650 / U6000 are not supported.`  
> **Translation note:** This file is an English translation of the original Japanese documentation.  
> The original archive contents, copyright status, and redistribution conditions are not changed by this translation.  
> Personal names are kept in their original Japanese form where no reliable reading or official English spelling is confirmed.

------------------------------------------------------------------------------
                        History        Version 1.11

                                   (C)TORO 1994
------------------------------------------------------------------------------

History is software that stores strings entered at the BASIC command line and
allows them to be reused through key operations. It also provides features
such as a tab key.

---

## Operating environment

- Runs on the E500 series with 32K or more.  
  Because the specification was changed, `E650 / U6000 cannot run it`.  
  Also, only at the time of installation, a machine-language area of at least
  `0BE000H` (`1C00H` bytes) is required.

- If, when made resident, it would cross a 64K boundary (for example, if it
  were to reside across the boundary between `0AxxxxH` and `0BxxxxH`), it
  cannot be installed resident.

- Normally this has no practical impact, but a work area represented by
  `[0BFCF6H]` is reserved. Programs written by the manufacturer do not use
  this area, but anyone personally using this location will have a conflict.

- The History main body operates normally even if it resides in a
  write-protected slot.

---

## Installation method

1. Confirm that a machine-language area of at least `0BE000H`
   (`1C00H` bytes) has been secured.

2. Load the included `HISTORY.BIN`.

       >LOAD M "HISTORY.BIN"

3. There are two possible resident locations, so execute one of them.

       >CALL &HBE000        use on S1:
       >CALL &HBE003        use on S2:

4. If installation succeeds, the screen is cleared and control returns to the
   command line.

5. At this point, a file named `History.TSR` is created on `S1:` (or `S2:`).
   This is the main body file. Do not move or delete it.
   For reference, the created file size is `569` bytes.

If installation fails, the following messages are displayed:

- `Already staying`  
  A file with the same name already exists.

- `Out of memory`  
  The minimum memory required for resident installation cannot be secured.

- `Check sum error`  
  The program body has somehow been altered. This also happens if it is
  executed a second time without reloading.

- `Can't install over page`  
  Resident installation failed because it would cross a 64K boundary.

---

## Usage

After entering a few commands normally two or three times
(and do not forget to press `[RETURN]`), try pressing `^[←]`
(meaning cursor-left while holding the `[CTRL]` key).

Previously entered items should appear. If you keep pressing it, they should
come up in order from the most recent. Recalled items can be edited as usual,
or executed by pressing the `[RETURN]` key.

When it is no longer needed, press `[@]` and then `[+/-]`
(the symbol `@` and the key to the right of `0`) to uninstall the resident
program. After that, the file `History.TSR` may be deleted.

---

## List of extended keys

- `^[←]`  
  Go back one entry in the input string history

- `^[→]`  
  Move one step forward among the entries returned with `^[←]`

- `^[I]`  
  Tab key (inserts spaces for 8-tab width)

- `[+/-]`  
  Execute extended command

        @    uninstall resident program

---

## About the source

- History is assembled using 活用研究's assembler.

- Sections commented out with `"<<< for TORO >>>"` are included as personal
  extras. They are not used in the public release.

- At present, the following functions are dormant.  
  By modifying the source, the functionality can be expanded.

  1. `^[CAPS]`  
     The author uses this to start a filer.

  2. `[command-name][+/-]`  
     Execute the specified command.

### About (1)

Remove the leading `;` from lines `227-228` and `249-251` of the source,
and simply change the address specified on line `250` to the desired address.
Then it can be used.

Programs that can be called with BASIC's `CALL` statement can be used.

### About (2)

The first character is placed in register `A`, and the rest of the string is
placed, terminated by `0DH`, from the address pointed to by register `X`.
By writing the desired processing at line `334`, and then jumping to the label
`RETED` when done, functionality can be added.

As done with `@`, it is recommended that commands be one character or more,
and that if the command is not your own, processing should be able to move on
to the next one.

The entered string is stored in the history buffer.

In either case, if the routine returns with the carry flag set, `Error`
will be displayed.

### Program notes

Because it is equipped with a full relocator, relocation is possible as long
as the following points are observed. There is no need to worry about jump
addresses and the like.

1. When specifying 3-byte values, `0ExxxxH` is not allowed.  
   This value is treated as program code, so use something like `1ExxxxH`
   instead.

2. Address references through tables are not allowed.  
   (`JP [X]` and similar forms)

   Only program code is relocated; data is not.

Because it operates while trapping the BASIC editor, the `U` stack is reduced
by `100H` or more, and internal registers are in use at addresses `60H` or
above.

When using internal registers, do it the same way the system does: subtract
only the necessary amount from `BP`, and keep the amount of usage down.
Programs that take the easy route by fixing `BP` will almost certainly run
out of control for this reason.

After assembling, execute it once before saving. Otherwise, the checksum will
not be written.

---

## Final notes

- This is freeware. Copyright belongs to `TORO / 高橋 良和`, so please do not
  infringe upon it.

- We assume no responsibility whatsoever for any profit, loss, or damage
  arising from use of this program.

- Modification is freely permitted for personal use.

- If redistributing this program elsewhere, do not modify it; redistribute it
  in the same contents as obtained.

- Please contact the author if you wish to use it for commercial purposes.

---

## History

**Version 1.11**  
- Fixed a problem where installation would not complete correctly when trying
  to install with `HMF` (`(C)HiM氏`)

**Version 1.10**  
- Changed behavior so that identical contents are not stored repeatedly in the
  history buffer

**Version 1.03**  
- Fixed incorrect text for the TAB key
- Changed the key assignment for TAB
- Fixed a bug in the installer

**Version 1.02**  
- Fixed a problem where data extended one byte beyond the work area

**Version 1.01**  
- Display a message when resident installation fails
- Fixed a problem where installation would hang when `S1:` exceeded 64K

**Version 1.00**  
- First release

---

## Contact

`TORO / 高橋 良和`

- `POCKET通信 Ver.3` … `3054`
- `NIFTY-Serve` … `GHE00667`

---

## Source and status

- **Source document:** documentation included with `History Version 1.11`
- **Source program version:** `Version 1.11`
- **Translation status:** complete draft translation
- **Formatting:** adapted for Markdown / GitHub readability
- **Content policy:** no functional changes intended; wording has been translated,
  but original meaning should take precedence if any ambiguity remains

## Notes

- This English translation is intended as a practical reference for users who
  cannot easily read the original Japanese document.
- `E650 / U6000 are explicitly unsupported in this version.`
- Archive contents, program behavior, and redistribution conditions should be
  understood according to the original package and original author statement.
- Where technical nuance is uncertain, the original Japanese wording should be
  treated as authoritative.
