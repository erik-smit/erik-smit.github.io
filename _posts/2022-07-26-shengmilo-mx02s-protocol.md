# Protocol

Baud Rate: 9600

## Display -> Controller

| byte | byte | what | examples |
| ---- | ---- | ---- | -------- |
| 00 | 01h |
| 01 | 14h |
| 02 | 01h | 
| 03 | 02h | P10 running mode | 0 only pas<br>1 only throttle<br>2 pas and throttle |
| 04 | 03h | PAS | ffh = no assist<br> 01h = min<br>0fh = max | 
| 05 | 80h | bitwise | bit 6 = P09 don't assist < 6km/h <br>bit 7 = manual light | 
| 06 | 01h | P07 Magnetic poles ratio | 01 = 01<br>100 = 64h |
| 07-08 | 01h 04h | P06 Rim Size | 5(.)0 = 0032<br>26(.)0 = 0104<br>50(.)0 = 01f4 |
| 09 | 05h | P11 PAS start sensitivity| 01-24 |
| 10 | 01h | P12 PAS start strength | 01-05 |
| 11 | 00h |
| 12 | 64h | P08 speed limit | 0 = 0<br>100 = 64 |
| 13 | 16h | P14 / current limit |  
| 14-15 | 01h B8h | P15/undervoltage | 34(.)0 = 0154h<br>44(.)0 = 01B8h<br> |
| 16 | 00h |
| 17 | 00h |
| 18 | 4Ch | bit-wise | bits 0-3 = PAS sensor type (P13)<br> bit 6 = cruisemode (P17) | 
| 19 | 12h | XOR | 

## Controller -> Display

| byte | byte | what | examples |
| ---- | ---- | ---- | -------- |
| 00-07 | 02h 0Eh 01h 00h 40h 00h 00h 00h |
| 08-09 | 00h 00h | wheel speed (MS) | 0507 = 05.7km/h<br>02e4 = 10.0km/h<br>0080 = 58.3km/h<br>0060 = 77.7km/h<br>005f = 78.5km/h<br>0058 = 84.8km/h |
| 10-12 | 00h 00h 00h | |
| 13 | 4Dh | XOR of 00-12 | |
