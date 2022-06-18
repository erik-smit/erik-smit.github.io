ThrottleStop didn't work on my Dell XPS 9343.  
Intel XTU said the Power Limits were locked.

Found some tutorials for Dell XPS to unlock this. 

Summary
-------
 * Dump UEFI with FPTW64.exe for your Intel ME version.
 * Use `UEFITool` to search for "CFG Lock", as text, and extract the PE32 file
 * Use `Universal IFR Extractor` on the PE32 file.
 * Read the text file and search for " lock" stuff.
 * When setting with `setup_var $varstore $offset $data`, pay attention to `Offset $offset is: $old_data` to doublecheck the old status.

Things I learned
-----
 * [modGRUBShell.efi](https://github.com/datasone/grub-mod-setup_var/releases/download/1.1/modGRUBShell.efi) didn't work for me. [This one](https://github.com/XDleader555/grub_setup_var/releases) worked for me.
 * `setup_var` couldn't find `CpuSetup`. Found the name `Setup` in the IFR as `VarStore: VarStoreId: 0x2 Name: Setup`
 * Ended up setting the following to 0x0: 
```
Overclocking lock: 0x4, VarStore: 0x2
Package power limit lock: 0x9, VarStore: 0x2
Config TDP LOCK 0x23, VarStore: 0x2
CFG lock: 0x37, VarStore: 0x2
Platform power limit lock: 0x46, VarStore: 0x2
```
 * Throttlestop still didn't work after this. Changing PL1 showed the MSR got changed, but package power didn't increase and hwinfo64 still showed the same PL1 limit.
 * But Intel XTU no longer complained the Power Limits were locked. And changing PL1 to 25w took effect. Yay!
