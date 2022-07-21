# Sheng Milo MX02S Infos

Here's some notes about my Sheng Milo MX02S.

# Brake lever adjustment. (~2400km/2022-06-28)
The right brake lever was no longer depressing the brake cutoff switch, requiring constant outward pressure during cycling.
Solved by rotating the middle adjustment screw of the right brake lever counterclockwise.

# Brake Pads replacement. (~1950km)
I needed to replace my Sheng Milo MX02S Brake Pads at ~1950km

Brakes: XOD XD-H500. Seems to be a clone of the Tektro HD-E500.  
Replacement Pads: [BBS-52](https://bbbcycling.com/nl_nl/bbs-52-discstop-hp). Pads compatible with HD-E500 should be fine.

# Pedal replacement (1800km)
One of the pedals started havint trouble rotating around 1800km, so I replaced them.
Seems like pedal mounting is very standard.

# Bike Stem Riser replacement.
As the standard steering was a bit low for me, I replaced it with an adjustable bike stem riser.
I got an Hiland Universal Bike Stem Riser. 90mm/31.8mm.

# Battery
OEM Reention Polly DP-6C.  
Charging port: DC2.1.

# LCD-M5 details

| P | What | Range | meaning | notes |
|---|------|-------|---------|-------|
| P01 | Backlight Brightness | 1-3 | 1 min<br>2 mid<br>3 max |
| P02 | Speed unit | 0-1 | 0 km/h<br>1 miles/h |
| P03 | Battery Voltage | 24,36,48,60 | Volts |
| P04 | Timeout | 0-60 | minutes |
| P05 | PAS Range | 0-4 | 0 0-3<br>1 0-5<br>2 0-9<br>3 0-4<br>4 0-6 |
| P06 | Rim Size | 0-50 | inch |
| P07 | Motor magnetic poles ratio | 1 - 100 |
| P08 | Speed limit | 0-100 | km/h |
| P09 | Zero Start | 0-1 | 0 assist < 6 km/h<br>1 don't assist < 6km/h |
| P10 | Running Mode | 0-2 | 0 only pas<br>1 only throttle<br>2 pas and throttle |
| P11 | PAS Start Sensitivity | 1-24 | short - long |
| P12 | PAS Start Strength | 1-5 | weak - strong |
| P13 | PAS Sensor Type | 5-8 | |
| P14 | Current Limit | 1-22 | no effect for me |
| P15 | Undervoltage | | |
| P16 | Clear ODO | | |
| P17 | Automatic cruise | 0-1 | 0 No auto cruise<br>1 If keep certain speed for 6 seconds, go to cruise mode |
| P18 | Speed indication ratio | 50-150 | % |
| P19 | PAS 0 disable | 0-1 | 0 PAS 0 enabled<br>1 PAS 0 disabled |
| P20 | Communication type | 0-3 | 0 2 protocol<br>1 5S agreement<br>2 standby<br>3 standby |

![EB09X1](/images/2022-04-23-shengmilo-mx02s-infos/pcb.jpg)
![EB09X1](/images/2022-04-23-shengmilo-mx02s-infos/pcb2.jpg)

# Controller

| connector | pin | C colors | B colors | purpose |
|-----------|-----|----------|----------|---------|
| | 5-pole | black<br>light blue<br>blue<br>yellow<br>red | GND<br>cont->disp<br>disp->cont<br>VBAT when display on<br>VBAT | display |
| | 2-pole | black<br>green/yellow | black<br>red | e-brake |
| | 2-pole | black<br>green/yellow | black<br>red | e-brake |
| | 3-pole | black<br>grey<br>red | black<br>white<br>red | throttle |
| | 3-pole | black<br>red<br>purple | black<br>red<br>white | pedal sensor |
| | 2-pole | black<br>orange | | headlight |
| | 3x2-pole | red<br>yellow<br>white<br>green<br>black<br>blue | | motor hall |
| Deans plug | | | | battery |

# Protocol

## Display -> Controller

| byte | byte | what | examples |
| ---- | ---- | ---- | -------- |
| 00 | 01h |
| 01 | 14h |
| 02 | 01h | 
| 03 | 02h | P10 running mode | 0 only pas<br>1 only throttle<br>2 pas and throttle |
| 04 | 03h | PAS | ffh = no assist<br> 01h = min<br>0fh = max | 
| 05 | 80h | bitwise | bit 6 = P09 don't assist < 6km/h <br>bit 7 = manual light | 
| 06 | 01h | 
| 07 | 01h |
| 08 | 04h |
| 09 | 05h | P11 PAS start sensitivity| 01-24 |
| 10 | 01h | P12 PAS start strength | 01-05 |
| 11 | 00h |
| 12 | 64h |
| 13 | 16h | P14 / current limit |  
| 14 | 01h |
| 15 | B8h | P15/undervoltage | 34.0 = 54h<br>44.0 = B8h<br> |
| 16 | 00h
| 17 | 00h |
| 18 | 4Ch | bit-wise | bits 0-3 = PAS sensor type (P13)<br> bit 6 = cruisemode (P17) | 
| 19 | 12h | XOR | 

## Controller -> Display

| byte | byte | what | examples |
| ---- | ---- | ---- | -------- |
| 00-07 | 02h 0Eh 01h 00h 40h 00h 00h 00h |
| 08-09 | 00h 00h | wheel speed | 0507 = 05.7km/h<br>02e4 = 10.0km/h<br>0080 = 58.3km/h<br>0060 = 77.7km/h<br>005f = 78.5km/h<br>0058 = 84.8km/h |
| 10-12 | 00h 00h 00h | |
| 13 | 4Dh | XOR of 00-12 | |


| pin | purpose |
|-----|---------|
| P1 | Hall / No Hall? |
| AX1 | |
| AX2 | |
| TB | Anti-theft mode?|
| TA | Pedal Sensor? |
| SA/SB/SC | Motor Hall |
| CR | Cruise? |
| RX/TX | UART |
| BKL | |
| SP | Throttle? |
| SP5V | |
| +5V | |
| VB2+ | |


# Motor

Has a PCB inside with "LDX104-120-003X" silkscreen.

# Dates
Delivery: February 2021.

# Also sold under 

| name | bike | 
|------|------|
| Alfina | [ALFINA FX02S Turbo](https://alfina.net/shop/alfina-fx02s/) |
| CEAYA | [CEAYA MX02S](https://www.ceayabikes.com/en-nl/products/electric-bikes-ceaya-mx02s-2022) |
| GUNAI | |
| VOZCVOX | |


---

# LCD protocols

| name | source |
|------|--------|
| 2 | 
| 5S | Kingmeter KM5S? |
| J-LCD | |
| apt | |
| bafang | eight party |
| KT | Kunteng |


