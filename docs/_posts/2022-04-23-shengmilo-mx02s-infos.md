# Sheng Milo MX02S Infos

Here's some notes about my Sheng Milo MX02S.

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

# Controller

| pin | C colors | B colors | purpose |
|-----|----------|----------|---------|
| 5-pole | black<br>light blue<br>blue<br>yellow<br>red | | display |
| 2-pole | black<br>green/yellow | black<br>red | e-brake |
| 2-pole | black<br>green/yellow | black<br>red | e-brake |
| 3-pole | black<br>grey<br>red | black<br>white<br>red | throttle |
| 3-pole | black<br>red</br>purple | black<br>red<br>white | pedal sensor |
| 2-pole | black<br>orange | | unconnected |
| 3x2-pole | red<br>yellow<br>white<br>green<br>black<br>blue | | goes to back motor |

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
