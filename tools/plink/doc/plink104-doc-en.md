# PLINK.SYS Ver. 1.04 Documentation (English Translation)

> **Original document:** `PLINK.DOC`  
> **Original language:** Japanese  
> **Program:** Pocket Link System Device Driver `PLINK.SYS Ver. 1.04`  
> **Copyright holder:** 近 成人  
> **Translation status:** English translation prepared for reference and accessibility  
> **Translation note:** This file is an English translation of the original Japanese documentation.  
> The original archive contents, copyright status, and redistribution conditions are not changed by this translation.  
> Personal names are kept in their original Japanese form where no reliable reading or official English spelling is confirmed.

------------------------------------------------------------------------
           Pocket Link System Device Driver
                    PLINK.SYS Ver. 1.04

                    (c)1990,93,94 近 成人
------------------------------------------------------------------------



The Pocket Link System Device Driver is the device driver required on the
pocket computer side when using the Pocket Link System with the PC-E500
series pocket computers.

---

## Package contents

This package contains the following files:

- `PLINK.DOC` … the document you are reading now
- `PLINK` … the program used to install the device driver
- `PLINK.S` … the source program for PLINK (can be reassembled with XASM)
- `PLINK.BAS` … a BASIC program that writes PLINK into memory

The check values for these files are as follows:

| Name        | Size  | CRC32    |
|-------------|------:|----------|
| `plink`     |  2076 | 129e1318 |
| `plink.bas` |  4738 | eac025b4 |
| `plink.s`   | 20956 | 2833ce38 |

---

## Preparation for use (installation method)

This device driver is written in machine language, but it is structured to
remain resident and function in one of the memory slots `"S1:"`, `"S2:"`,
or `"S3:"`. Therefore, it does not require a BASIC program area or a machine
language area for normal operation. However, because a machine language
program must be executed in order to make it resident, a machine language
area is required at the time of installation.

To run the installation program, the area from `$BF000` onward must be
assigned as the machine language area. Prepare it by performing an operation
such as:

    POKE &BFE03,&1A,&FD,&B,0,&C,0
    CALL &FFFD8

`PLINK.BAS` is a BASIC program that describes, using BASIC comments and
`POKE` / `CALL` commands, the machine language program used to install the
device driver as a resident driver. When you `RUN` it, the machine language
program for making the device driver resident is written into memory and then
executed automatically. Therefore, after securing the machine language area,
you only need to transfer `PLINK.BAS` to the pocket computer by some means
and run it.

If execution succeeds and the driver is installed correctly, the message
`done` will be displayed. At the same time, a file named `PLINK.SYS` should
be created in `"S1:"` with the write-protect attribute (`"P"`). This file
`PLINK.SYS` is the device driver itself, so please be careful not to modify
it thereafter.

If the installation fails, an error message will be displayed. Improve the
environment as indicated. The error messages and their meanings are as
follows:

- `Not enough memory`  
  … There is not enough free space in the memory slot where the driver was
  to be made resident.

- `PLINK already exist`  
  … `PLINK.SYS` already exists.

- `Bridged over between different pages`  
  … The device driver would span across different memory pages.

- `fatal error`  
  … The structure of the memory slot is abnormal (there is no workaround
  other than performing an ALL RESET).

This completes preparation of the device driver. Once it has been made
resident, neither the installation BASIC program nor the machine language
area is needed anymore. After that, prepare the PC-side program and serial
input/output environment as required.

For serial I/O, the PC must be able to receive data sent from the pocket
computer, and the pocket computer must be able to receive data sent from the
PC. Also, the communication settings must use an 8-bit data length, and all
possible 8-bit data values must be able to pass through without alteration.
(If the bit length is not 8 bits, or if XON/XOFF flow control is enabled,
these conditions may not be satisfied.) The recommended communication
settings are:

- **Communication speed**: 9600 bps or higher
- **Parity**: none
- **Bit length**: 8 bits
- **Stop bits**: 1 bit
- **X control**: disabled

The bit length on the pocket computer side is automatically set to 8 bits at
runtime.

---

## Usage

The Pocket Link System is an environment that provides a virtual drive named
`"L:"`, so there is no special usage procedure as such. You can operate
`"L:"` in exactly the same way as you normally use `COPY`, `SAVE`, or `LOAD`
with RAMFILE drives such as `"E:"` or `"F:"`. For example, try:

    COPY "S1:TEXT.BAS" TO "L:"
    FILES "L:"

You should then see the same contents in `"L:"` as in `"S1:TEXT.BAS"`.

---

## How to terminate the Pocket Link System

The pocket-computer-side device driver does not have any special concept of
“termination,” so leaving it resident when not in use causes no harm.
On the other hand, while the server program is running, the PC cannot do
other work, so if you want to use the PC for something else, you must stop
the server program.

This can be done from the pocket computer side with:

    INIT "L:D"

---

## If an abnormal condition occurs

Under normal usage, abnormalities should almost never occur, but it is still
possible that communication may fail due to some accident. In such a case,
try the following on the pocket computer side:

    INIT "L:I"

This sends 130 bytes of `0x00` from the pocket computer and simultaneously
reinitializes the internal state of the device driver. If the server side
responds normally after this operation, it is advisable to check whether the
contents of `"L:"` have been improperly damaged, using a block device check
tool or similar, before resuming use.

If the server side still does not respond even after `INIT "L:I"`, forcibly
stop the server-side program by `RESET` or similar, and then tell `"L:"`
that the server side has stopped by executing:

    INIT "L:"

In such a case, what happens to the contents of the virtual drive `"L:"`
depends on the server program. It may appear normal at first glance, but
management data may have been corrupted. Therefore, it is advisable to check
whether the contents of `"L:"` have been improperly damaged using a block
device check tool or similar before resuming use.

The pocket-computer-side device driver should remain valid unless a runaway
condition, ALL RESET, or some similar event occurs.

---

## Notes on use

- When stopping the server program, always do so by using `INIT "L:D"` or an
  equivalent operation. If the server program is terminated by itself
  directly, the data on the pocket computer side and the PC side may become
  inconsistent the next time the server is started, which can result in
  improper destruction of the data on `"L:"`.

- Never perform operations that change the address where the device driver
  resides, such as deleting files located before the driver. Doing so will
  definitely cause a runaway condition. Also, while `"L:"` is still in a
  state where it does not produce the message `Bad drive name` when accessed,
  never delete `PLINK.SYS`.

---

## About the program

The device driver `PLINK.SYS` does not return correct values for every command
required of a standard block device. Media check (command number `0x10`)
always returns “no error,” and verify-related commands merely perform the
transfer operation and return “no error” without actually performing a
verification.

Furthermore, due to the structure of the Pocket Link System, the command that
obtains a sector address (command number `0x17`) cannot return a correct
sector address. Instead, it reads the specified sector into a buffer and
returns the address of that buffer. Therefore, the value that should be
obtained in `(di)` is completely meaningless.

Also, after completion of the processing for command numbers `0x12` through
`0x15`, as well as `0x17`, the values in the logical registers and the X
register are completely meaningless. This is not a problem when using the
driver through FCS (File Control System), but care must be taken if you are
accessing the device directly.

---

## History

**Ver. 1.00 (90/05/18)**  
- First public release

**Ver. 1.01 (93/01/16)**  
- Improved the processing for command number `17h` so that unnecessary sector
  reads are no longer performed
- Reduced use of `CALLF` / `RETF` to the minimum necessary and replaced them
  with `CALL` / `RET`
- Automated address correction processing during source program creation
- Significantly improved the installer program
- Improved the BASIC installation program

**Ver. 1.02 (93/04/17)**  
- Removed a bug that caused data inconsistency when command numbers `12h–15h`
  and `17h` were used together

**Ver. 1.03 (93/06/06)**  
- Following advice from `"Euphonium 吹き☆えみゅ"`:

  1. Forced the SIO bit length to `"8"` during send/receive
  2. Changed the order of transmit-data write and busy-wait
  3. Removed unnecessary `push` / `pop`
  4. Prevented installation when the code would span across different pages

**Ver. 1.04 (94/03/18)**  
- Following advice from `"本物モデム"`, fixed a bug in the address
  correction table

---

## References

- 加古 英児, **“KNJSCRN”**, freeware
- 近 成人, **“Pocket Link System”**, *Pocket Computer Journal*,
  June 1990 issue

---

## Copyright and redistribution

The copyright for this program is owned by 近 成人.

As long as the contents of the archive are not modified, redistribution of the
archive is permitted. Transfer to others is treated in the same way.

---

## Closing remarks

Please send any comments you may have, whether good impressions, bad
impressions, or suggestions for improvement.

The following electronic mail/contact methods are available:

- **NIFTYserve** … `KFD02231`
- **JUNET** … `kon@cs.titech.ac.jp`
- **Amateur Radio** … `JK1OSG @ JH2EUO`
- **Pocket Communication** … `ID = 1481 (N.Kon) [TEL: 03-3299-8661]`

As for JUNET, the account still exists at present, but there is no telling
when it may disappear.

                                                         Mar. 26th, 1994
                                                                 近 成人

---

## Source and status

- **Source document:** `PLINK.DOC`
- **Source program version:** `PLINK.SYS Ver. 1.04`
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
