# Cipher #2 - 100

## Problem
We are given the following ciphertext.

```
YHTEQAPSSQWLTLSILYPENZIZSJLVPVIPVWLKDLZRCWZXGEZEWCDHDBRCSIEXHLWKRKTOYIUPVFCLBRDIWULGSPOIYKLHZMMYBLLVPVQGXAYEDXILWGWDVRPMPOWNKDAIHSQSLLEESAMSQAIEMPALPEJKAXIPOEHEPLZRTWYTZJPOMPSNSD
```

Our hints:

1. The first 50 characters of the decoding is the flag.
2. The last character of the flag should be a Y after decoding.

## What is this cipher?

We first need to determine the type of cipher this is encrypted with. I recommend the following article, it goes deeply into identifying ciphers from unknown ciphertext using statistical analysis:
http://practicalcryptography.com/cryptanalysis/text-characterisation/identifying-unknown-ciphers/

## Polyalphabetic cipher?

Using [this tool](http://practicalcryptography.com/cryptanalysis/text-characterisation/index-coincidence/) from the same site, the index of coincidence of this ciphertext is `0.045324700057131975`, which means the ciphertext isn't very english-like. This is smaller than the index of coincidence of a substitution cipher `~0.06`, which is english-like.

So is this not encrypted english? It probably is, but just uses a good cipher that properly masks letter frequencies. Polyalphabetic ciphers are looking to be a good candidate now.

## Vigenere cipher
Either after making a good guess, or doing a full statistical analysis from the article above, let's try to decrypt this assuming it's a Vigenere cipher.

A key characteristic of Vigenere ciphers is that it will begin to show a high index of coincidence when analyzing every nth letter if `n` is a multiple of the size of the key. This is because every nth letter will be have the same Caeser shift.

I don't want to get too deep, but the following article has great description of vegenere ciphers and how to break them.

http://practicalcryptography.com/cryptanalysis/stochastic-searching/cryptanalysis-vigenere-cipher/

## Getting the flag

Using the [python_cryptoanalysis](https://github.com/jameslyons/python_cryptanalysis) tools, the process of looking at the index of coincidence for ever `n` letter becomes easy and automated.

```sh
$ python2 break_vigenere.py
-1280.55526865 Vigenere, klen 3 :"HEW", RDXXMEIOWJSPMHWBHCIARSEDLFPOLZBLZPHOWHDKYASTKXVIPYHAZFKYWBABAHADNOMKCBQTOBGEXVWEANHKLLSBUOEDDFICUHPOLZJCBTUIWTMESKPZZKLQIKAGGHTELLMWEHIXOEFOUTEIFLEELICGEQETHALXLPSNXPUXSFTHITLJWW
-1231.45186737 Vigenere, klen 4 :"LWEA", NLPEFELSHUSLIPOIACLECDEZHNHVEZEPKAHKSPVRRAVXVIVELGZHSFNCHMAXWPSKGOPONMQPKJYLQVZILYHGHTKINOHHOQIYQPHVEZMGMEUESBELLKSDKVLMESSNZHWIWWMSAPAEHEISFEEEBTWLEIFKPBEPDIDEEPVRIAUTONLOBTONHH
-945.763250349 Vigenere, klen 5 :"WHITE", CALLMEIKZMAELSOMEQWARSAGONENWRMINDHOWDGNGPRECISWDYHAVINGLALTLEORNOMGFEYINMYPUJKEANDNOTHAFGPARTICUDSRTOINTERWKTMEONSHOJWITHOUGHTAOOULDSAILSTOUTALITTDWANDSEETHWOATERYPARLGFTHEWORLV
-1215.3527378 Vigenere, klen 6 :"HALAHR", RHIEJJISHQPUMLHIEHIECZBILJAVIEBPKWETWLORVFSXVESNPCSHWKKCHIXGALLKKTMONINYOFRLUAWILUEPLPDIRTEHOMFHULAVIEJGMARNWXXLPPPDKRIVIOLNDMTIWSJBELTELJFSFABNFPPLINCKPXBYHEWEIUSRIWRCSJEOFYLNHD
-1172.65983999 Vigenere, klen 7 :"YPELJTL", ASPTHHEUDMLCAAUTHNGLCBTVHASKRGEEMDAMOHOIJLBICTQLLEODSSYRUTAMYSLMCGIFFXWARUTSQTOELLSVUAKXPRAJKIBPIANGLKHNMCJASOPAYRSSMYEOAKLERSCTDHHZANPAHRTHSLETDWPNAAYBHMKAKTYLENKNINFIBULDDWHPDZ
-1163.02289077 Vigenere, klen 8 :"ESLALEWE", UPIEFWTOOYLLIHWEHGEECVMVORAVERMLREAKSHDNYEOXVADASKSHSXVYOQTXWHAGNSIONEYLRNRLQNHESCAGHLSEUSAHOIQUXTAVERUCTINESTMHSOLDKNTILWLNZZEEDAFSAHIAOIBSFWMAIXPLEANGWFXPDALALTORISCPVREOBLWJOL
-1183.70604366 Vigenere, klen 9 :"RELEESWWE", HDIAMITWOZSAPHAMPUYACVEHWNHELKELDAPGMHONYEDBCNVTSYLLHXAYHEAFLPSTNZPKGMYLEBRHXZHMSDHVOLWMCGUDOIIGFPHELKMCFECAMTXHSOAHRALBLKEROZJEWOMAPPANOPIOYEMAVLPHLMNOWGEEKAPITHINISUBDNLXIEOJAH
-730.102942975 Vigenere, klen 10 :"WHITEWHALE", CALLMEISHMAELSOMEYEARSAGONEVERMINDHOWLONGPRECISELYHAVINGLITTLEORNOMONEYINMYPURSEANDNOTHINGPARTICULARTOINTERESTMEONSHOREITHOUGHTIWOULDSAILABOUTALITTLEANDSEETHEWATERYPARTOFTHEWORLD
```

```sh
$ python -c "print('CALLMEISHMAELSOMEYEARSAGONEVERMINDHOWLONGPRECISELYHAVINGLITTLEORNOMONEYINMYPURSEANDNOTHINGPARTICULARTOINTERESTMEONSHOREITHOUGHTIWOULDSAILABOUTALITTLEANDSEETHEWATERYPARTOFTHEWORLD'[0:50])"
CALLMEISHMAELSOMEYEARSAGONEVERMINDHOWLONGPRECISELY
```

© Copyright to Cisco for this problem.
