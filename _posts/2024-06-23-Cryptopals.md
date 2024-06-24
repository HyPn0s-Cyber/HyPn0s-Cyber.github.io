---
title : Learn Crypto With the Cryptopals Challenges
date : 2024-06-23 15:13:00 +0200
categories : [crypto,learn]
tags : [crypto, writeup]
---


## This serie is about the [Cryptopals](https://cryptopals.com/) Challenges to learn Crypto

Get ready to code, this is the begining of a long and wonderful journey!


If you are like me and want to improve your crypto skills for CTFs, this is the place, follow along while I myself, learn more about this exiting world.


# Set 1 

#### **As I wish to improve for CTFs, I'm going to resolve the challenges in Python since it's the common language.**
You can do them in any language that you like :)

## Challenge 1 Hex to Base64


The first challenge is to transform the hex string:

`49276d206b696c6c696e6720796f757220627261696e206c696b65206120706f69736f6e6f7573206d757368726f6f6d`

To base64, which will produce:

`SSdtIGtpbGxpbmcgeW91ciBicmFpbiBsaWtlIGEgcG9pc29ub3VzIG11c2hyb29t`


For this one, I tried to use the minimum dependencies, only the base64 module to encode. 

Here is my code for this challenge:


```python 
from base64 import b64encode

#Getting the input 
input = '49276d206b696c6c696e6720796f757220627261696e206c696b65206120706f69736f6e6f7573206d757368726f6f6d'

#The input should be treated as bytes, so converted to bytes en then encode in base64, the .decode() is to transform the bytes back to a string
result = b64encode(bytes.fromhex(input)).decode()

#Output =SSdtIGtpbGxpbmcgeW91ciBicmFpbiBsaWtlIGEgcG9pc29ub3VzIG11c2hyb29t 
#Output without the .decode() = b'SSdtIGtpbGxpbmcgeW91ciBicmFpbiBsaWtlIGEgcG9pc29ub3VzIG11c2hyb29t'

print(result)
```

It's this simple! 


Let's continue with the second challenge 

## Challenge 2 Fixed XOR

This time, we have to implement a function which takes in input a hex string, decode it, and then XOR it against another string. 

For example : 


If your function works properly, then when you feed it the string:

`1c0111001f010100061a024b53535009181c`

after hex decoding, and when XOR'd against:

`686974207468652062756c6c277320657965`

should produce:

`746865206b696420646f6e277420706c6179`

Let's do this! 

***Note that the two strings are of equal length***

```python
# Function to decode hex to bytes
def HEXDecode(input):
    return bytes.fromhex(input)

# Function to XOR to equal length sequences and return the hex value
def XOR(input1, input2):
    result = bytes(a ^ b for a, b in zip(input1, input2))
    return result.hex()

# Inputs
input1 = HEXDecode('1c0111001f010100061a024b53535009181c')
input2 = HEXDecode('686974207468652062756c6c277320657965')

print(XOR(input1,input2))  # Prints the results
#Output = 746865206b696420646f6e277420706c6179
```
_The trick here, is to not decode the byte sequence in the HEXDecode() function,keep it as bytes for the XOR to work. I made that mistake too_

## Challenge 3 Single-byte XOR cipher

This is starting to get interesting. Our new challenge is to find the key of a XOR'd string input. 
More precisely the hex encoded string:

`1b37373331363f78151b7f2b783431333d78397828372d363c78373e783a393b3736`

has been XOR'd against _a single character_. Find the key, decrypt the message.

__Hint:__
How? Devise some method for "scoring" a piece of English plaintext. Character frequency is a good metric. Evaluate each output and choose the one with the best score.



I started with absolutly no idea on how to implement this, but I already knew the character frequency technique. 

I first tried to only execute all the possibilities and then implement the key finding. 

```python
def HEXDecode(input):
    return bytes.fromhex(input)

def XOR(key,input2):
    return bytes([a ^ key for a in input2])

input = '1b37373331363f78151b7f2b783431333d78397828372d363c78373e783a393b3736'

for i in range(256)  : 
    print(XOR(i,HEXDecode(input)).decode('utf-8', errors='replace'))
```
With this code I had an output with all the possibilities (non UTF8 characters are remplaced by a ? within the decode function)
```
7316?x§413=x9x(7-6<x7>x:9;76
¶§▼[¶↔[↓→↑¶§
?‼‼↨§↕1?[\►§↨↓\↔\
‼       ↕↑\‼→\▲↔▼‼↕
‼↓]↕→^◄↑^∟▼↔◄►▬↑]∟]
<►►¶▬◄↑_2<X
_‼▬¶→_▲_►
◄↓▲↨§→↑▼▬Q<2V☻Q↔↑→¶Q►Q☺▲♦▼§Q▲↨Q‼►↕▲▼
1↔↔↓∟§?1U☺R▲↓↨‼R☻↔∟▬R↔¶R►‼◄↔∟
0∟∟↑→↔¶S>0TS▼→↑▬S↕S♥∟♠↔↨S∟§S◄↕►∟↔
7▼↔→‼T97ST↑↔▼◄T§T♦☺→►T↕▬§↨→
6→→▲∟↕86R♠U↓∟▲►U¶U♣→◄→‼U↨¶▬→

↓↓↔▼↑◄V;5Q♣V→▼↔‼V↨V♠↓♥↑↕V↓►V¶↨§↓↑
4↑↑∟▲↓►W:4P♦W▲∟↕▬W↑☻↓‼W↑◄W§▬¶↑↓
H♥☺♠H%+OH↑↔♠
HH

♠
*♠♠☻I$*N→I♣☻
I♠I∟

♠
J')M↓J♠♥☺J
J→♣▼♦J♣
J
K♦▲♣♦
♦♣
/♥♥♣☻
L∟♥↓L♥♣ L
♥☻

♦C. D►C
♠C☻C‼
C
♣C☺☻

'


☺D♣D¶↨D
◄
D
☻D♠♣
Cooking MC's like a pound of bacon
Bnnjhof!LB&r!mhjd!`!qntoe!ng!c`bno
Ammikle"OA%q"nkig"c"rmwlf"md"`caml
@llhjmd#N@$p#ojhf#b#slvmg#le#ab`lm
Gkkomjc$IG#w$hmoa$e$tkqj`$kb$fegkj
Fjjnlkb%HF"v%iln`%d%ujpka%jc%gdfjk
Eiimoha&KE!u&jomc&g&vishb&i`&dgeih
Dhhlni`'JD t'knlb'f'whric'ha'efdhi
jFFB@GN djZ     E@BL    H       YF\GM   FO      KHJFG
iEEACDM
Yi
FCAO
K
ZE_DN
EL
HKIED
hDD@BEL
fh
X
GB@N
J
DM
IJHDE
oCCGEBK
ao
_
@EGI
M
\CYBH
CJ
l@@DFAHb\CFDJN_@ZAK@IMNL@A
cOOKINGmcSLIKEAPOUNDOFBACON
bNNJHOF☺lb♠R☺MHJD☺@☺QNTOE☺NG☺C@BNO
aMMIKLE☻oa♣Q☻NKIG☻C☻RMWLF☻MD☻@CAML
`LLHJMD♥n`♦P♥OJHF♥B♥SLVMG♥LE♥AB@LM
gKKOMJC♦ig♥W♦HMOA♦E♦TKQJ@♦KB♦FEGKJ
fJJNLKB♣hf☻V♣ILN@♣D♣UJPKA♣JC♣GDFJK
eIIMOHA♠ke☺U♠JOMC♠G♠VISHB♠I@♠DGEIH
dHHLNI@jdTKNLBFWHRICHAEFDHI
����������������������������������
����������������������������������
����������������������������������
����������������������������������
����������������������������������
����������������������������������
����������������������������������
����������������������������������
����������𼹻��𠿥���𲱳��
����������񽸺��񡾤���񳰲��
����������򾻹��򢽧���򰳱��
����������󿺸��󣼦���󱲰��
���������������������������������
���������������������������������
���������������������������������
���������������������������������
�������腋�褡���踧���觮誩���
```
_Most of it has been excluded for simplicity_

Did you find the uncoded sentence? I did after reading all of it. No spoil if you didn't, let's complete the code to find the key and the sentence automatically. 

