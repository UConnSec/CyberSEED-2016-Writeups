# Cipher #3 - 200

## Problem
We are given the following information:

- *Grundstellung*: The key for the ciphertext is outputted when the initial setting is 'GQT' and the message 'UKJ' is inputted.
- *Ringstellung*: The rotors are I, II, and III
- *Walzenlage*: Order of the three rotors is unknown.
- *Steckerverbindungen*: The plugs are 'BF CM DR HQ JK LU OY PW ST ??'. One is unknown.

Ciphertext is **GCXSSBPPIUDNWXJZGIICUEFYGISQOCGLLGMMKYHJ**

And most importantly, we know the string **BELADEN** is in the decrypted plaintext.

This solution assumes knowledge of the enigma machine. I recommend the following resources:
- https://www.theguardian.com/technology/2014/nov/14/how-did-enigma-machine-work-imitation-game
- https://en.wikipedia.org/wiki/Enigma_rotor_details

## Theory

There are different types of enigma machines. The one here has three rotors, so it is likely a very early version. [M3](http://enigma.louisedade.co.uk/enigma.html) is a good guess.

We are given a surprising amount of information but with missing parts. A brute force attack may seem feasible. Let's calculate the possible keys.

- `26 * 26 * 26` for the three ring settings (ringstellung).
- `3 * 2 * 1` orders for the ring. This is a permutation without repetition.
- `28` possibilities for the missing plugs. This is the [1+2+3+4](https://en.wikipedia.org/wiki/1_%2B_2_%2B_3_%2B_4_%2B_%E2%8B%AF) series. Since are 8 missing characters. This results in the [sum of i from i=1 to 7](https://www.wolframalpha.com/input/?i=sum+of+i+from+i%3D1+to+7), which is 28.

So: `26^3 * 6 * 28 = 2,952,768` possible decryptions.

Note: We are also missing the reflector. It turns out `B` is a good guess since this is the M3 Enigma.

## Theory (Analytical)

Brute forcing although doable, doesn't seem like an optimal solution. Surely there's a way to analyze the ciphertext of an encryption mechanism invented over half a century ago?

If you thought that, you're probably right, but the methods are much more difficult than brute forcing. The problem looks like it wants you to do crib dragging in my opinion, but this is difficult since there is only one ciphertext. The crib is just used to confirm the answer.

The wonderful person from Cisco that designed this great question confirmed that the intention of this problem was to have the solution be code that performed a brute force.

## Code

The code took about 45 minutes to execute on a 2.8 GHz processor. Your usage may vary.

```python
from pycipher.enigma import Enigma


ALPHABET = 'ABCDEFGHIJKLMNOPQRSTUVWXYZ'

ciphertext = 'GCXSSBPPIUDNWXJZGIICUEFYGISQOCGLLGMMKYHJ'
crib = 'BELADEN'

# rotors: The rotors and their order e.g. (2,3,1). There are 8 possible rotors.
allrotors = [(1, 2, 3), (1, 3, 2), (2, 1, 3), (2, 3, 1), (3, 1, 2), (3, 2, 1)]

# steckers:  The plugboard settings
steckers = [('B', 'F'), ('C', 'M'), ('D', 'R'), ('H', 'Q'), ('J', 'K'),
            ('L', 'U'), ('O', 'Y'), ('P', 'W'), ('S', 'T')]

posplugchars = 'AEGINVXZ'
posplugs = []

for i in range(len(posplugchars)):
    for j in range(i+1, len(posplugchars)):
        posplugs.append((posplugchars[i], posplugchars[j]))

# reflector: The reflector in use, a single character 'A','B' or 'C'
reflector = 'B'


# ringstellung: The ring settings, consists of 3 characters e.g. ('V','B','Q')
def allringstellung():
    for i in ALPHABET:
        for j in ALPHABET:
            for k in ALPHABET:
                yield (i, j, k)


for plug in posplugs:
    guessteck = steckers + [plug]

    for rotors in allrotors:
        for ringstellung in allringstellung():
            key = Enigma(('G', 'Q', 'T'), rotors, reflector, ringstellung, guessteck)
            setting = key.encipher('UKJ')

            e = Enigma(setting, rotors, reflector, ringstellung, guessteck)
            decrypt = e.decipher(ciphertext)
            print(decrypt)
```

Unfortunately I did not save the flag and do not want to wait 45 minutes for it. If you generate it, a pull request would be appreciated.

© Copyright to Cisco for this problem.
