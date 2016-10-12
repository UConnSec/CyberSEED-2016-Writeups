# Missing Flag #2

We are given the following file:  https://github.com/gluxon/CyberSEED-2016-Writeups/blob/master/missingflag2

## What type of file is this?

Let's try `file`.
```sh
$ file missingflag2
missingflag2: data
```

Ok, that was helpful. Let's try a hex viewer instead.

```sh
$ cat missingflag2 | xxd
00000000: 5245 4441 4354 4544 0000 0022 1000 1000  REDACTED..."....
00000010: 0005 f900 3221 0ac4 42f0 0021 a817 2b52  ....2!..B..!..+R
00000020: 5e6a 2058 64a1 e848 561b 1cc8 5846 8400  ^j Xd..HV...XF..
00000030: 0028 2000 0000 7265 6665 7265 6e63 6520  .( ...reference
00000040: 6c69 6220 312e 332e 3020 3230 3133 3035  lib 1.3.0 201305
00000050: 3236 0000 0000 fff8 c918 00c2 4cfc fbfc  26..........L...
00000060: fafc dffc b8fc 91fc 80fc d8b5 39ff 5969  ............9.Yi
00000070: 0008 fff0 ebf8 386a 211d 1c56 f06e 527f  ......8j!..V.nR.
00000080: 5ab0 f45e c6a5 e7a1 cdc5 089c 2579 db12  Z..^........%y..
00000090: d998 b137 0f1a b679 48f5 99c2 4389 661e  ...7...yH...C.f.
```

Looks like some useful strings are in the header. Googling for *"reference lib 1.3.0 20130526"* reveals that we are working with a flac audio file.

## Fixing FLAC header
So REDACTED is probably replacing our proper flac header. Let's replace it with "fLaC".

```sh
$ sed -e 's/REDACTED/fLaC/' missingflag2 > missingflag2.flac
```

NOTE: the above command is for gnu sed. `sed` on macOS may not work. In this case, use a hex editor.

This doesn't appear to be enough. `metaflac` comes up with an MD5 mismatch and many audio players are unable to play it.

## Playing the file

There's an official solution, and also a workaround.

(1) For me, trying VLC seemed to work. To get it to work in other programs like Audacity, converting it to flac again with VLC fixes the header.

![Converting using VLC](https://github.com/gluxon/CyberSEED-2016-Writeups/blob/master/missingflag2-vlc.png)

(2) I've been informed after the CTF competition by students from the University of Tulsa (ascii_overflow) that the next step is to fix the part of the header that contains the byte length. (It might not be the byte length and I'm misremembering their valuable information.)

## Viewing the spectrogram

This is a forensics question, so looking at the spectrogram is likely required. The end of the file seems to have something interesting.

![Spectrogram of End](https://github.com/gluxon/CyberSEED-2016-Writeups/blob/master/missingflag2-spectrogram.png)

Alrighty, this looks like morse code.

### Getting the Flag

After converting the short amplitudes to `.` and the long amplitudes to `-`, you get a hexadecimal. Convert this to ASCII, and you will receive the flag.

I no longer have the morse code from the competition. If anyone generates a copy and can submit a pull request, it would be appreciated.
