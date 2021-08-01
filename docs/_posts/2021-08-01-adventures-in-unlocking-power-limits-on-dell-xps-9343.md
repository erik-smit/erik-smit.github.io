ThrottleStop didn't work on my Dell XPS 9343.  
Intel XTU said the Power Limits were locked.

Issues
-----
 * First modded GRUB I found complained about finding the wrong GUID things. [This one](https://github.com/XDleader555/grub_setup_var/releases) worked for me.
 * `setup_var` couldn't find `CpuSetup`. Found the name `Setup` in the IFR as `VarStore: VarStoreId: 0x2 Name: Setup`
 * Ended up setting the following to 0x0: 
 
```
Overclocking lock: 0x4, VarStore: 0x2
Package power limit lock: 0x9, VarStore: 0x2
Config TDP LOCK 0x23, VarStore: 0x2
CFG lock: 0x37, VarStore: 0x2
Platform power limit lock: 0x46, VarStore: 0x2
```
 * After this, ThrottleStop showed the TPL was unlocked, and showed the MSR got changed, but the changes didn't go into effect.
 * But after this, the Intel XTU no longer complained the Power Limits were locked. And changing PL1 to 25w took effect. Yay!
