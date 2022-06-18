# Experimenting with RWS-371 and TWS-BS-6

Some notes of my experiments with RWS-371 and TWS-BS-6.

Transmitter: TWS-BS-6  
Receiver: RWS-371  
Oscilloscope: DSO Quad  
Measurement: Mustool UD18

Everything worked as expected on a breadboard. Used breadboard wires as antennas.

* Powerconsumption of TWS-BS-6 without signal was ~0.0w.
* Generating a signal with the scope into the DI (Data In) increased power consumption to ~0.2w and made a signal appear around 433.9Mhz on my RTLSDR.
* Connecting the TWS-BS-6 increased to ~0.2w idle and ~0.4w with signal.
* Connecting the RWS-371 DO (Digital Out) or LO (Linear Out) to the scope worked as expected with squarewave signals.
* With sawtooth or triangle waves, not much remained on the Linear Out, maybe needs smaller frequencies, dunno.
