# COMPO-System Documentation (English Translation)

> **Original document:** COMPO-System documentation included with Version 2.13  
> **Original language:** Japanese  
> **Program:** `COMPO-System Version 2.13`  
> **Copyright holder:** `TORO / 高橋 良和`  
> **Translation status:** English translation prepared for reference and accessibility  
> **Translation note:** This file is an English translation of the original Japanese documentation.  
> The original archive contents, copyright status, and redistribution conditions are not changed by this translation.  
> Personal names are kept in their original Japanese form where no reliable reading or official English spelling is confirmed.

------------------------------------------------------------------------------
                        COMPO-System        Version 2.13

                                   (C)TORO 1994
------------------------------------------------------------------------------

COMPO-System is a command-driven multifunction debugger designed to make it
easier to develop machine-language programs on the E500.

---

## Features

- Can trace programs in ROM as well (with restrictions)
- Can refer to the label list output by 活用研究's assembler
- Can return to BASIC at any time and execute external programs
- Can redirect screen output to files and similar destinations
- By using History, it can be used easily and without side effects
- The main body exists in file form and does not occupy the machine-language area

---

## Operating environment

- Runs on the E500 series with 32K or more.  
  (`E650 / U6000` have some restrictions.)

- Only at the time of installation, a machine-language area of at least
  `0BC000H` (`3C00H` bytes) is required.

- If, when made resident, it would cross a 64K boundary (for example, if it
  were to reside across the boundary between `0AxxxxH` and `0BxxxxH`), it
  cannot be installed resident.

- Normally this has no practical impact, but a work area at `[0BFCF6H]` is
  reserved. Programs written by the manufacturer do not use this area, but
  anyone personally using this location will have a conflict. (`History`)

- `BFD80H` to `BFDFFH` (system reserved area) is used for COMPO.

- The COMPO / History main body operates normally even if it resides in a
  write-protected slot.

---

## Installation method

COMPO cannot operate by itself. It can only be used together with History.
Even users already using History must use the COMPO-specific History included
in this set (`Ver 1.11C`).

1. Confirm that a machine-language area of at least `0BC000H`
   (`3C00H` bytes) has been secured.

### Install History

2. Load the included `HISTORY.BIN`.

       >LOAD M "HISTORY.BIN"

3. There are two possible resident locations, so execute one of them.

       >CALL &HBE000        use on S1:
       >CALL &HBE003        use on S2:

4. If installation succeeds, the title is displayed briefly, then the screen
   is cleared and control returns to the command line.

### Install COMPO

5. Load the included `COMPO.BIN`.

       >LOAD M "COMPO.BIN"

6. There are two possible resident locations, so execute one of them.

       >CALL &HBC000        use on S1:
       >CALL &HBC003        use on S2:

7. If installation succeeds, the title is displayed briefly, then the screen
   is cleared and control returns to the command line.

8. At this point, files named `History.TSR` and `COMPO.TSR` are created on
   `S1:` (or `S2:`). These are the main body files. Do not move or delete
   them. For reference, the created file sizes are `587` and `5974` bytes,
   respectively.

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

If History cannot be used, please use the included `CC`.

---

## Usage

COMPO uses the extended-command execution capability of History.
To use COMPO functions, enter a command at the BASIC command line and then,
instead of pressing `[RETURN]`, execute it by pressing `[+/-]`.

Example: edit `BD000H`

    >EBD000[+/-]

---

## List of extended keys (History functions)

- `^[←]`  
  Go back one entry in the input string history

- `^[→]`  
  Move one step forward among the entries returned with `^[←]`

- `^[I]`  
  Tab key (inserts spaces for 8-tab width)

- `[+/-]`  
  Execute COMPO command / uninstall resident program

        @    uninstall resident program

---

## Function descriptions

COMPO uses one-letter commands plus parameters.
All numeric values are hexadecimal, and all spaces are meaningful.

Press `[ON]` to cancel command execution.

---

### `@`  Uninstall resident program

This command ends use of COMPO.
After executing it, the file `COMPO.TSR` may be deleted.

To uninstall History as well, execute `@` once more.

---

### `D`  Hex / ASCII dump

This command performs a hexadecimal dump and ASCII dump.

To dump starting from `BD000H`, execute:

    >DBD000

This dumps `8 * 4` bytes per screen.
In this state, pressing `[↑]` or `[↓]` scrolls one line at a time so that
you can view the next portion.

---

### `P`  Print

Even though the E500 screen is large for a pocket computer, it is still
narrow. COMPO makes work easier by allowing most screens to be written out
to a file. The command for this is `P`.

First, open an appropriate file beforehand using BASIC's `OPEN` statement:

    >OPEN "xxxxxxx" FOR OUTPUT AS #1

Then, by adding `P` before the command whose output you want to capture,
the displayed contents will also be sent to the file.

To save the previous dump to a file:

    >PDBD000[+/-]

When finished, `CLOSE` the file.

Now, it is inconvenient if file output stops after one screen.
To output from `BD000H` to `BE000H` without waiting for key input, execute:

    >PD/BD000 BE000[+/-]

---

### `A`  ASCII dump

This command extracts only the ASCII-dump portion of the `D` command and
allows more to be displayed. The input format is the same as for the `D`
command.

---

### `C`  Disassemble

Performs disassembly. The input format is the same as for the `D` command.

Even when disassembling backward, it can still disassemble reasonably well
to some extent. Beyond that point, it goes back one byte at a time.

COMPO's disassembler can refer to the label table output by 活用研究's
assembler and replace matching values with labels. It also has a function
that considers the current `BP` register in order to determine internal RAM
addresses accurately. These can be enabled using the following `U` command.

---

### `U`  Change disassembly options

Changes disassembly-related options.

- omitted  
  Display current settings and defined labels

- `address`  
  Set the address of the label table.  
  Specify the beginning of the assembler output.

- `K`  
  Use COMPO's built-in label table

- `I`  
  Convert addresses / data into corresponding labels

- `O`  
  Do not perform label conversion

- `R`  
  Keep internal RAM references as coded

- `S`  
  Convert internal RAM references to absolute addresses by adding `BP` and
  `PRE` code. However, correct values cannot be obtained when going backward.

---

### `G`  Graphics dump

Performs a dump of graphic image data.
The `P` command cannot be used with this.

The input format is the same as for the `D` command.

---

### `I`  Internal RAM dump

Performs a dump of internal RAM.

The input format is the same as for the `D` command.

---

### `E`  Edit

A so-called monitor function.
The `P` command cannot be used with this.

If an address is specified, editing begins from that point.

Operations are as follows:

- `[↓][↑][←][→]`  
  Move cursor

- `[TITLE]`  
  Toggle hexadecimal input / character input

  - Hex mode: `0–9`, `A–F`
    (`[/][*][-][+][.][+/-]`)
  - Character mode: characters can be entered except kana and lowercase letters

- `[INS][DEL]`  
  Insert / delete

If a full-screen dump is not shown when this is executed, or if the screen
looks untidy after exit, that is not a bug. It is done so that previous values
remain visible.

---

### `F`  Data search

Searches for specific data in memory.
Up to 32 bytes of data can be searched.

For a hexadecimal sequence:

    >Fstart-address end-address data data ...

For a string:

    >Fstart-address end-address"string

In both cases, the wildcard `?` can be used.

---

### `J`  User call

Executes the program starting at the specified address.
When it returns with `RETF`, a register list is displayed.
If it returns from a breakpoint, the display is shown in reverse video.

---

### `T`  Trace

Traces one instruction at a time starting from the specified address.
A register list and the instruction to be executed are displayed. Press any key
to trace one instruction.

When the instruction is `CALL` or `CALLF`, pressing the following keys lets
you choose whether to trace into the called destination:

- `[P]`  
  Pass over tracing inside the subroutine  
  (in ROM trace mode, passing over a `CALL` instruction is not possible)

- `[T]`  
  Trace inside the subroutine

COMPO can also trace programs in ROM.
However, the ROM trace feature must first be enabled using the following
`V` command.

---

### `V`  Change trace options

Changes tracing-related options.

- omitted  
  Display current settings

- `I`  
  If not otherwise specified, trace into the destination of `CALL / CALLF`

- `O`  
  If not otherwise specified, do not trace into the destination of
  `CALL / CALLF`

- `R`  
  Enable tracing on ROM  
  (however, for a `CALL` instruction, the called destination must also be traced)

- `W`  
  Disable tracing on ROM

---

### `B`  Set breakpoint

Specifies the address at which the `J / T` command will forcibly stop.
If execution stops at a breakpoint, the register display is shown in reverse
video.

Breakpoints are ineffective if set on ROM.

- omitted  
  Display configured address

- `address`  
  Set a new breakpoint

- `O`  
  Clear the breakpoint

---

### `Q`  Display / set registers

Displays or modifies the registers used by the `J / T` commands.

- omitted  
  Display the current values of all registers

- `register-name value`  
  Change the specified register  
  Any displayed register name may be used:

  `F BA I X Y U S U BP PX PY BX CX DX SI DI IMR`

---

### `L`  Raw file load

`SAVE M` can load machine-language files, but it requires specific data at the
start of the file. This command simply reads any file, for the time being,
starting from the beginning of the machine-language area.

If an address is specified after the file name, loading begins from there.

---

### `S`  Raw file save

The reverse of the `L` command.
This command writes the specified range directly to a file.

Specify it as follows:

    >Sfilename start-address end-address[+/-]

---

### `N`  Set / display user machine-language area

If an address is specified, that address and above become the machine-language
area.

If nothing is specified, the current start address of the machine-language
area is displayed.

---

### `X`  Memory block transfer

Moves (copies) data in a specified range to another location.

Specify it as follows:

    >X source-start-address source-end-address destination-start-address[+/-]

---

### `Y`  Memory data fill

Fills the specified range with the specified one-byte data.

Specify it as follows:

    >Y start-address end-address hex-value[+/-]

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

**Version 2.13**  
- Fixed a problem where installation would not complete correctly when trying
  to install with `HMF` (`(C)HiM氏`)

**Version 2.12**  
- Fixed a problem in the `T` command where the `PRE` code was not reflected

**Version 2.11**  
- Fixed `J` and `T` commands not operating correctly
- Corrected the built-in label list

**Version 2.10**  
- First public release

---

## Contact

`TORO / 高橋 良和`

- `POCKET通信 Ver.3` … `3054`
- `NIFTY-Serve` … `GHE00667`

---

## Source and status

- **Source document:** documentation included with `COMPO-System Version 2.13`
- **Source program version:** `Version 2.13`
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
