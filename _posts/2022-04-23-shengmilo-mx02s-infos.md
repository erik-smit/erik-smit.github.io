# Sheng Milo MX02S Infos

Here's some notes about Sheng Milo MX02S.

# Motor
Has a PCB inside with "LDX104-120-003X" silkscreen.

# OEM Parts

Light and Horn Button - Star Union E-BIKE-DK226/2  
Light and Horn - [EF300](https://mayebikes.en.made-in-china.com/product/WmsrDybcLBkh/China-Cheap-Price-Electric-Bike-Light-Horn-36V-48V-Front-Head-Light-5W-12W-Universal-Bicycle-Horn-Headlight-for-Electric-Scooter.html) 

# Battery
OEM Reention Polly DP-6C.  
Layout: 13S5P
Charging port: DC2.1/DC2.5 (unsure)

# Also sold under 

| name | bike | 
|------|------|
| Alfina | [ALFINA FX02S Turbo](https://alfina.net/shop/alfina-fx02s/) |
| CEAYA | [CEAYA MX02S](https://www.ceayabikes.com/en-nl/products/electric-bikes-ceaya-mx02s-2022) |
| GUNAI | |
| VOZCVOX | |

# Controller

![EB09X1](/images/2022-04-23-shengmilo-mx02s-infos/pcb.jpg)
![EB09X1](/images/2022-04-23-shengmilo-mx02s-infos/pcb2.jpg)

| connector | PCB | C colors | B colors | purpose |
|-----------|-----|----------|----------|---------|
| JST-SM 5P M | GND<br>TX<br>RX<br>VK+<br>VB+ | black<br>green<br>blue<br>yellow<br>red | GND<br>cont->disp<br>disp->cont<br>VBAT when display on<br>VBAT | display |
| JST-SM 2P M | GND<br>BKL | black<br>green/yellow | black<br>red | e-brake |
| JST-SM 2P M | GND<br>BKL | black<br>green/yellow | black<br>red | e-brake |
| JST-SM 2P F | GND<br>VB+ | black<br>red | black<br>red | light/horn power |
| JST-SM 3P M | GND<br>SP<br>SP5V| black<br>grey<br>red | black<br>white<br>red | throttle |
| JST-SM 3P F | GND<br>+5VP <br>TA | black<br>red<br>purple | black<br>red<br>white | pedal sensor |
| Molex/Spade 1x2 F | GND<br>VK+ | black<br>orange | unconnected | headlight (?)? |
| Molex/Spade 3x2 F | +5VP<br>SC<br>CR<br>SB<br>GND<br>SA | red<br>yellow<br>white<br>green<br>black<br>blue | red<br>yellow<br>white<br>green<br>black<br>blue | +5v<br>Hall C<br>speed magnet?<br>Hall B<br>GND<br>Hall A |
| Deans plug F | VB-<br>VB+ | black<br>red | | battery |
| bullets F | A<br>B<br>C| blue<br>green<br>yellow | blue<br>green<br>yellow | motor power |

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

# UART Protocol

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
| 00 | 02h | 
| 01 | 0Eh |
| 02 | 01h |
| 03 | 00h | bitwise | bit 5 = throttle stuck / E 008 |
| 04 | 40h | bitwise | bit 5 = braking<br>bit 6 = ??? | 
| 05 | 00h |
| 06 | 00h | current | 01h = 01.0A<br>02h = 02.0A |
| 07 | 00h | PWM(?) | 00h-ffh = 0% - 100%? |
| 08-09 | 00h 00h | wheel speed (MS) | 0507 = 05.7km/h<br>02e4 = 10.0km/h<br>0080 = 58.3km/h<br>0060 = 77.7km/h<br>005f = 78.5km/h<br>0058 = 84.8km/h |
| 10-12 | 00h 00h 00h | |
| 13 | 4Dh | XOR of 00-12 | |



