# NOTES:  a simple notes script.

The script notes allows a user to write a small note, and
maintain a text file (`NOTES.txt`) of these notes, in a format that
is easily read automatically or by a human.

The file adds a header to the note made by the user, accounting
for the time period while editing the note, the actual text and
a checksum protecting the whole text of the note.  After an added
note, the program add a checksum at the end of the file to
protect the whole thing.

The checksums can be verified individually by selecting the lines
they cover and passing all those lines through `sha256` command.
The sha checksum after a separator line covers all the lines from
the beginning of the file up to the separator line previous to
the checksum.  The individual notes (just before a separator
line) cover the lines from the previous file checksum upto the
line before the checksum to be tested.

You can execute the following command in `vi(1)` to check the
whole file:

```
:1,$-1!sha256
```

you'll exchange all the selected lines by theyr checksum, so you
should get two checksum equal lines on your screen.  To undo,
just press `u` and the operation will be undone.

An easy way to check this in `vi(1)` is to create a map to one key,
and search the previous line containing the pattern `^Start-date :`,
and the following line containing a checksum (previous line to
pattern, where pattern is `/^[0-9a-f]\{64\}/`) so something like:

```
:map ^V<F4> :?^Start-date :?,/^[0-9a-f]\{64\}$/-1!sha256^V^M
```

(where ^V is Ctrl-V and \<F4\> is the F4 key, and ^M is Ctrl-M,
in `vim(1)` it will work as shown, but you have the possibility
of naming the F4 key as shown above, so you don have to use the
^V approach)
