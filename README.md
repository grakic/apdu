APDU shell
==========

A quick smartcard APDU read-evaluate-print loop shell with readline support.

Built with Python and [pyscard](http://pyscard.sourceforge.net/).

## Select PC/SC reader

Run <tt>apdu -l</tt> to list all PC/SC readers.

Select a reader by partial name or index (starting at 0) with <tt>apdu -r Gemplus</tt> or <tt>apdu -r 0</tt>.

On a selected reader, card can be inserted / removed during the shell session.

## Inside the shell
### APDU input
Type APDU commands as hex strings like <tt>00 A4 0400 08 A000000003000000 00</tt>. Spaces are ignored unless theya are separating single numbers. Hexstring input <tt>00 &nbsp; 1 2 0304</tt> is the same as <tt>00 01 02 03 04</tt>.

### Save data response to a file
Append <tt>> &lt;file name&gt;</tt> to save data output (response without sw1 and sw2 bytes) to a file.

Example: <tt>00 A4 04 00 08 A0 00 00 00 18 43 4D 00   00 > fci.bin</tt>

### Comments
Comments starts with a #.

    # Select "first" aid
    00 A4 04 00 00
    00 A4 04 00  08 A0 00 00 00 03 00 00 00  00    # not found?


### Passthru external commands

External commands are supported with<tt>RUN &lt;command&gt;</tt>.

This will execute APDUs printed on standard output, line by line, passing full response (including sw1 and sw2 bytes) to standard input. External commands are useful to make mini scripts that depend on input/output sequence. I am using this to do MIFARE Ultralight C authentication with <tt>RUN ultralightc_auth</tt>. This script does 3DES with RndA and RndB creating the authenicated session.

### Replay history and scripts

Use <tt>SAVE &lt;file name&gt;</tt> to save command history with comments to the file. Pipe this file to the shell to reply. Script file format is very simple, it is just a series of commands in plain text, one command per line. Blank lines are ignored.

There is also a <tt>CLEAR</tt> command to clear current history.

