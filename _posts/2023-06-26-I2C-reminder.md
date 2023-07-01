# I2C 7-bit addressing reminder

Since I keep re-remembering:

i2c_msg.write(0x37, [0x50, 0x80]) turns into: 0x6E, 0x50, 0x80 because I2C uses 7-bit addressing.  
0x37 = 00110111  
0x6E = 01101110  

0x50 = 01010000  
0xA0 = 10100000  

In other words: 0x37 address in software turns into 0x6E byte on the wire, because the last bit in the byte is used to signify read or write.
