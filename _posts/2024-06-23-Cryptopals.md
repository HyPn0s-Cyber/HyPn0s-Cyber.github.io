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