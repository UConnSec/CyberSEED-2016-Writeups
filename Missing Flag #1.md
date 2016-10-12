# Missing Flag #1

## Problem

We are given the following file: https://github.com/gluxon/CyberSEED-2016-Writeups/blob/master/missingflag1

## What type of file?

Let's first determine what type of file this is. `strings` and `xxd` to the rescue.

```sh
$ cat file2 | xxd
00000000: 890d 0a1a 0a00 0000 0d49 4844 5200 0003  .........IHDR...
00000010: c000 0002 2a08 0600 0000 eb7e 23cb 0000  ....*......~#...
00000020: 0c18 6943 4350 4943 4320 5072 6f66 696c  ..iCCPICC Profil
00000030: 6500 0048 8995 9707 5853 c916 c7e7 9614  e..H....XS......
00000040: 4242 0b44 404a e84d 905e a5f7 2220 1d6c  BB.D@J.M.^.." .l
00000050: 8424 4028 2104 828a 1d5d 5470 ed22 8aa2  .$@(!....]Tp."..
00000060: a22b 208a ae05 90b5 20a2 5858 042c d817  .+ ..... .XX.,..
00000070: 0b2a 2beb 62c1 86ca 9b14 d0e7 7bfb bdef  .*+.b.......{...
00000080: cdf7 cdbd bf9c 39e7 dcff cc9d b999 0140  ......9........@
00000090: d19e 2510 64a1 4a00 64f3 f385 5181 3ecc  ..%.d.J.d...Q.>.
```

Looks like we've got an iCC, or a picture with an iCC profile embedded. This is a CTF, so the latter is more likely. After googling *"IHDR and iCCPICC Profile"* several top results come with PNG in the title.

## Fixing the header

Ok, so we've got a PNG. Still can't open it with any editors. Let's check the header and make sure the file is properly reporting itself as PNG. Wikipedia is great for this.

https://en.wikipedia.org/wiki/Portable_Network_Graphics#File_header

Our header is supposed to have `89504E470D0A1A0A` as the file header if it's a PNG. Our file has `890D0A1A`. Ah, there's some bytes missing in the middle.

## Solution

Once the bytes are added patched, we get the following image:

![Vim is the best editor](https://github.com/gluxon/CyberSEED-2016-Writeups/blob/master/missingflag1.png)

And it is absolutely correct.
