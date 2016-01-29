APDU shell
==========

A quick smartcard APDU read-evaluate-print loop shell with readline support.

![Screenshot](http://blog.goranrakic.com/archives/slike/apdu.png)

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

This will execute APDUs printed on command's standard output as hex strings, line by line, passing full response hex string (including sw1 and sw2 bytes) to command's standard input. External commands are useful to make mini scripts that depend on input/output sequence.

For example, I am using this with a command to do MIFARE Ultralight C authentication with <tt>RUN ultralightc_auth</tt>. This program does 3DES with RndA and RndB like [shown here](https://code.google.com/p/libfreefare/source/browse/libfreefare/mifare_ultralight.c#247), modified to just read APDUs from standard input and write them to standard output in a frame required by PC/SC driver for the SCM SCL011 reader that I am using. Running this inside the APDU shell will create an authenticated session with the card.

### Replay history and scripts

Use <tt>SAVE &lt;file name&gt;</tt> to save command history with comments to the file. Pipe this file to the shell to replay. Script file format is very simple, it is just a series of commands in plain text, one command per line. Blank lines are ignored.

There is also a <tt>CLEAR</tt> command to clear current history.

## Alternatives
- [cyberflex-shell](https://github.com/henryk/cyberflex-shell)
- [pcsc-tools scriptor/gscriptor](http://ludovicrousseau.blogspot.com/2010/05/pcsc-sample-in-scriptor.html)
- [OpenSCDP Smart Card Shell 3](http://www.openscdp.org/scsh3/)
- [JSmartCardExplorer](http://www.primianotucci.com/os/smartcard-explorer)
- Android APDU Sender: [Contact](https://play.google.com/store/apps/details?id=com.jmarroyo.apdusendercontact), [Contactless](https://play.google.com/store/apps/details?id=com.jmarroyo.apdusendercontactless)
- [smacadu](http://www.literatecode.com/smacadu)
