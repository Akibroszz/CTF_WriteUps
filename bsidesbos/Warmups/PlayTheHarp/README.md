# Play The Harp

Using strings on the image we can find a weird block of data

```
HDNR6GFf
6LLIJK9l
18NL1HWa
GCU85U5g
RQ9CGTH{
T47Y9SUt
2SKZJOBh
H06K09Ze
3BWV54X_
C1VY4EIh
GO0DK9Ua
ZZLVBMZr
8CK8FTGp
TNDQURH_
CEHGS41i
ONSNNRTn
DYAKGQMs
AX9CNZ7t
CS5R3KQr
U4A6BBVu
F2RULTOm
D2NLIUPe
KYKGKGVn
AN98O3Ht
G9STPVD_
ETGMLPCh
TFUFSALa
PK4CD5Ss
6EDFJ45_
CIOL1S0v
VIJP3WFe
OU3CPSBr
O0F6WTWt
NKIWW0Ri
QPFWGVNc
CJUPZL9a
CEC4YQ8l
YC23ZR6_
DTUT5VJs
113O5FVt
VY2QV4Br
C498PXFi
NO6EMR1n
ND8JBSNg
OQJOHJUs
8IOJ9LD}
```
I got stuck here for hours while trying to decode this data but that wasn't actually the solution.
Look back to the block of data we found, you may think it's encoded but it's not, the flag is actually already in there.
The actual solution to this is to look at the rightmost letters in the data and read it downwards doing so reveals the flag.

**flag{the_harp_instrument_has_vertical_strings}**