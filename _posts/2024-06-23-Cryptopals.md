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
→66207>y¶→~*y502<y8y)6,7=y6?y;8:67
↓55134=z↨↓})z631?z;z*5/4>z5<z8;954
↑44025<{▬↑|({720>{:{+4.5?{4={9:845
▼33752;|◄▼{/|0579|=|,3)28|3:|>=?32
▲22643:}►▲z.}1468}<}-2(39}2;}?<>23
↔115709~‼↔y-~275;~?~.1+0:~18~<?=10
∟004618↕∟x,364:>/0*1;09=><01
‼??;9>7p↔‼w#p<9;5p1p ?%>4p?6p213?>
↕>>:8?6q∟↕v"q=8:4q0q!>$?5q>7q302>?
◄==9;<5r▼◄u!r>;97r3r"='<6r=4r031=<
►<<8:=4s▲►t s?:86s2s#<&=7s<5s120<=
↨;;?=:3t↓↨s't8=?1t5t$;!:0t;2t657;:
▬::><;2u↑▬r&u9<>0u4u%: ;1u:3u746:;
§99=?81v§%v:?=3v7v&9#82v90v47598
¶88<>90w→¶p$w;><2w6w'8"93w81w56489

''#!&/h♣
o;h$!#-h)h8'=&,h'.h*)+'&

&&" '.i♦
n:i% ",i(i9&<'-i&/i+(*&'
        %%!#$-j m9j&#!/j+j:%?$.j%,j(+)%$
$$ "%,kl8k'" .k*k;$>%/k$-k)*($%
##'%"+l☺k?l %')l-l<#9"(l#*l.-/#"
""&$#*mj>m!$&(m,m="8#)m"+m/,."#
i=n"'%+n/n>!; *n!(n,/-!

  $&!(o☻
h<o#&$*o.o? :!+o )o-., !
♥g3`,)+%`!`0/5.$`/&`"!#/.
☻..*(/&a
☻f2a-(*$a a1.4/%a.'a# "./
☺--)+,%b☺e1b.+)'b#b2-7,&b-$b #!-,
,,(*-$cd0c/*(&c"c3,6-'c,%c!" ,-
++/-*#d c7d(-/!d%d4+1* d+"d&%'+*
♠**.,+"♠b6e),. e$e5*0+!e*#e'$&*+
♣))-/(!f
♣a5f*/-#f'f6)3("f) f$'%)(
♦((,.) g
♦`4g+.,"g&g7(2)#g(!g%&$()
;↨↨‼◄▬▼X5;_
▬∟X↨▲X→↓↨▬

▬▬↕►↨▲Y4:^
Y§►↕∟Y↑Y        ▬
↨↔Y▬▼Y↑→▬↨
9§§◄‼¶↔Z79]     Z▬‼◄▼Z
§¶▲Z§∟Z↑↓§¶
¶¶►↕§∟[68[↨↕►▲[→[
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
        ♣♦
(♦♦☻♣
K&(L↑K☻K
K♦▲♣♦
♦♣
/♥♥♣☻
L∟♥↓L♥♣ L
♥☻
.☻☻♠♦♥
M .J▲M☺♦M
M↔☻↑♥   M☻
M
☻♥
-☺☺♣    N#-I↔N☻♣
NN▲☺
N
☺
,♦♠O",H∟O♥♠♦
OO▼→☺
O       O
☺
#
        @-#G‼@

♣@☺@►§♦@♠@☻☺♥
"
♠A,"F↕A
♦AA◄¶♣AA♥☻
!

♣B/!E◄B
↨       B♥B↕
♦B♥☺




♦C. D►C
♠C☻C‼
C
♣C☺☻

'


☺D♣D¶↨D
◄
D
☻D♠♣


&



☻E(&B▬E
E♦E§
►
☺E
♥E♦♠


☺F+%A§F
♥FF▬    ☻F      F♦♣
$
        G*$@¶G

☻G♠G↕   ♥☺G♣♠♦
[wwsqv8U[?k8tqs}8y8hwmv|8w~8zy{wv
Zvvrpw~9TZ>j9upr|9x9ivlw}9v9{xzvw
Yuuqst}:WY=i:vsq:{:juot~:u|:x{yut
Xttpru|;VX<h;wrp~;z;ktnu;t};yzxtu
_sswur{<Q_;o<puwy<}<lsirx<sz<~}sr
^rrvtsz=P^:n=qtvx=|=mrhsy=r{=|~rs
]qquwpy>S]9m>rwu{>>nqkpz>qx>|}qp
\pptvqx?R\8l?svtz?~?opjq{?py?}~|pq
S{y~w0]S7c0|y{u0q0`e~t0v0rqs~
R~~zxv1\R6b1}xzt1p1a~du1~w1spr~
Q}}y{|u2_Q5a2~{yw2s2b}g|v2}t2psq}|
P||xz}t3^P4`3zxv3r3c|f}w3|u3qrp|}
W{{}zs4YW3g4x}q4u4d{azp4{r4vuw{z
Vzz~|{r5XV2f5y|~p5t5ez`{q5zs5wtvz{
Uyy}xq6[U1e6z}s6w6fycxr6yp6twuyx
Txx|~yp7ZT0d7{~|r7v7gxbys7xq7uvtxy
Kggcafo(EK/{(dacm(i(xg}fl(gn(jikgf
Jffb`gn)DJ.z)e`bl)h)yf|gm)fo)khjfg
Ieeacdm*GI-y*fcao*k*zedn*el*hkied
Hdd`bel+FH,x+gb`n+j+{d~eo+dm+ijhde
Occgebk,AO+,`egi,m,|cybh,cj,nmocb
Nbbfdcj-@N*~-adfh-l-}bxci-bk-olnbc
Maaeg`i.CM)}.bgek.o.~a{`j.ah.loma`
L``dfah/BL(|/cfdj/n/`zak/`i/mnl`a
Cooking MC's like a pound of bacon
Bnnjhof!LB&r!mhjd!`!qntoe!ng!c`bno
Ammikle"OA%q"nkig"c"rmwlf"md"`caml
@llhjmd#N@$p#ojhf#b#slvmg#le#ab`lm
Gkkomjc$IG#w$hmoa$e$tkqj`$kb$fegkj
Fjjnlkb%HF"v%iln`%d%ujpka%jc%gdfjk
Eiimoha&KE!u&jomc&g&vishb&i`&dgeih
Dhhlni`'JD t'knlb'f'whric'ha'efdhi
{WWSQV_↑u{▼K↑TQS]↑Y↑HWMV\↑W^↑ZY[WV
zVVRPW^↓tz▲J↓UPR\↓X↓IVLW]↓V_↓[XZVW
yUUQST]→wy↔I→VSQ_→[→JUOT^→U\→X[YUT
xTTPRU\x∟HRP^TNU_]ZXTU
SSWUR[∟q∟PUWY∟]∟LSIRX∟SZ∟^]_SR
~RRVTSZ↔p~→N↔QTVX↔\↔MRHSY↔R[↔_\^RS
}QQUWPY▲s}↓M▲RWU[▲_▲NQKPZ▲QX▲\_]QP
|PPTVQX▼r|↑L▼SVTZ▼^▼OPJQ[▼PY▼]^\PQ
s__[Y^W►}s↨C►\Y[U►Q►@_E^T►_V►RQS_^
r^^ZX_V◄|r▬B◄]XZT◄P◄A^D_U◄^W◄SPR^_
q]]Y[\U↕q§A↕^[YW↕S↕B]G\V↕]T↕PSQ]\
p\\XZ]T‼~p¶@‼_ZXV‼R‼C\F]W‼\U‼QRP\]
w[[_]ZS¶yw‼G¶X]_Q¶U¶D[AZP¶[R¶VUW[Z
vZZ^\[R§xv↕F§Y\^P§T§EZ@[Q§ZS§WTVZ[
uYY]_XQ▬{u◄E▬Z_]S▬W▬FYCXR▬YP▬TWUYX
tXX\^YP↨zt►D↨[^\R↨V↨GXBYS↨XQ↨UVTXY
kGGCAFekDACXG]FGJIKGF
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
[D^EO
DM
IJHDE
oCCGEBK
ao
_
@EGI
M
\CYBH
CJ
NMOCB
`nBFDCJ
OLNBC
mAAEG@Icm       ]BGEKO^A[@JAHLOMA@
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
�������鄊�饠���鹦���馯髨���
�������ꇉ��ꦣ���꺥���ꥬꨫ���
�������놈�맢���뻤���뤭멪���
�������쁏�젥���켣���죪쮭���
�������퀎�����������������������
�����������������
�����������￠���ﭮ���
�����������଩���య���௦ࢡ���
�������ጂ�᭨���ᱮ���ᮧᣠ���
�������⏁�⮫���ⲭ���⭤⠣���
�������㎀�㯪���㳬���㬥㡢���
�������䉇�䨭���䴫���䫢䦥���
�������分�婬���嵪���媣姤���
�������担�檯���涩���橠椧���
�������犄�竮���編���稡祦���
�������ص�ߋؔ���ؙ؈����ؗ�ؚ����
�������ٴ�ފٕ���٘ى����ٖ�ٛ����
�������ڷ�݉ږ���ڛڊ����ڕ�ژ����
�������۶�܈ۗ���ۚۋ����۔�ۙ����
�������ܱ�ۏܐ���ܝ܌����ܓ�ܞ����
�������ݰ�ڎݑ���ݜݍ����ݒ�ݟ����
�������޳�ٍޒ���ޟގ����ޑ�ޜ����
�������߲�،ߓ���ߞߏ����ߐ�ߝ����
�������н�׃М���БЀ����П�В����
�������Ѽ�ւѝ���ѐс����ў�ѓ����
�������ҿ�ՁҞ���ғ҂����ҝ�Ґ����
�������Ӿ�Ԁӟ���ӒӃ����Ӝ�ӑ����
�������Թ�ӇԘ���ԕԄ����ԛ�Ԗ����
�������ո�҆ՙ���ՔՅ����՚�՗����
�������ֻ�х֚���֗ֆ����֙�֔����
�������׺�Єכ���זׇ����ט�ו����
�������ȥ�ϛȄ���ȉȘ����ȇ�Ȋ����
�������ɤ�ΚɅ���Ɉə����Ɇ�ɋ����
�������ʧ�͙ʆ���ʋʚ����ʅ�ʈ����
�������˦�̘ˇ���ˊ˛����˄�ˉ����
�������̡�˟̀���̜̍����̃�̎����
�������͠�ʞ́���͌͝����͂�͏����
�������Σ�ɝ΂���ΏΞ����΁�Ό����
�������Ϣ�Ȝσ���ώϟ����π�ύ����
����������Ǔ����������������������
����������ƒ����������������������
�������¯�ő������������
�������î�ĐÏ���ÂÓ����Ì�Á����
�������ĩ�×Ĉ���ąĔ����ċ�Ć����
�������Ũ�ŉ���ńŕ����Ŋ�Ň����
�������ƫ���Ɗ���ƇƖ����Ɖ�Ƅ����
�������Ǫ���ǋ���ǆǗ����ǈ�ǅ����
���������ۿ����������������������
���������ھ����������������������
���������ٽ����������������������
���������ؼ����������������������
���������߻����������������������
���������޺����������������������
���������ݹ�����������������������
���������ܸ����������������������
���������ӷ���������������������
���������Ҷ���������������������
���������ѵ���������������������
���������д���������������������
��������׳�������������������
��������ֲ�������������������
��������ձ�������������������
��������԰�������������������
��������˯��������������������
��������ʮ��������������������
���������ɭ�������������������
��������Ȭ��������������������
��������ϫ��������������������
��������Ϊ�������������������
��������ͩ�������������������
��������̨�������������������
��������ç������������������
��������¦������������������
����������������������������
����������������������������
��������ǣ�������������������
��������Ƣ�������������������
��������š�������������������
��������Ġ�������������������
������ߘ���˘���ݘ٘����ܘ�ޘ�����
������ޙ���ʙ���ܙؙ����ݙ�ߙ�����
������ݚ���ɚ���ߚۚ����ޚ�ܚ�����
������ܛ���ț���ޛڛ����ߛ�ݛ�����
������ۜ���Ϝ���ٜݜ����؜�ڜ�����
������ڝ���Ν���؝ܝ����ٝ�۝�����
������ٞ���͞���۞ߞ����ڞ�؞�����
������؟���̟���ڟޟ����۟�ٟ�����
������א��Ð���Րѐ����Ԑ�֐�����
������֑�����ԑБ����Ց�ב�����
������Ւ�������גӒ����֒�Ԓ�����
������ԓ�������֓ғ����ד�Փ�����
������Ӕ���ǔ���єՔ����Д�Ҕ�����
������ҕ���ƕ���Еԕ����ѕ�ӕ�����
������і���Ŗ���Ӗז����Җ�Ж�����
������З���ė���җ֗����ӗ�ї�����
������ψ��ۈ���͈Ɉ����̈�Έ�����
������Ή��ډ���̉ȉ����͉�ω�����
������͊��ي���ϊˊ����Ί�̊�����
������̋��؋���΋ʋ����ϋ�͋�����
������ˌ��ߌ���Ɍ͌����Ȍ�ʌ�����
������ʍ��ލ���ȍ̍����ɍ�ˍ�����
������Ɏ��ݎ���ˎώ����ʎ�Ȏ�����
������ȏ��܏���ʏΏ����ˏ�ɏ�����
������ǀ��Ӏ���ŀ������Ā�ƀ�����
������Ɓ��ҁ���ā������Ł�ǁ�����
������ł��т���ǂÂ����Ƃ�Ă�����
������ă���Ѓ���ƃ����ǃ�Ń�����
������Ä��ׄ�����ń������������
��������օ�����ą�������Å�����
����������Ն���Æǆ������������
����������ԇ���Ƈ����Ç��������
```

Did you find the uncoded sentence? I did after reading all of it. No spoil if you didn't, let's complete the code to find the key and the sentence automatically. 

