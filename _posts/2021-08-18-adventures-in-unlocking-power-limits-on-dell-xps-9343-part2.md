Building on my work on 2021-08-01.

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

Things I wish I learned before
---------------------------
* Check _every_ change with a HWInfo64 report. If a change is not visible in HWInfo64, no dice.
* Making a CSV-report with HWInfo64 before and after and diffing is a nice way to check for effects.
* This would've saved me quite rabbitholes.

Things I learned
-----
* [modGRUBShell.efi](https://github.com/datasone/grub-mod-setup_var/releases/download/1.1/modGRUBShell.efi) didn't work for me. [This one](https://github.com/XDleader555/grub_setup_var/releases) worked for me.
* Putting on a USB Stick as \efi\boot\bootx64.efi works, but putting on main efi partition as grub_setup_var.efi and adding to boot menu is more comfortable!
* `setup_var` couldn't find `CpuSetup`. Found the name `Setup` in the IFR as `VarStore: VarStoreId: 0x2 Name: Setup`
* ThrottleStop and Intel XTU couldn't change PL1 or PL2, because they were locked. ThrottleStop shows a lock. HWInfo64 shows:
```
"CPU Power Limit 1 - Long Duration:"," (15.00 W) (28.00 sec) [Locked]"
"CPU Power Limit 2 - Short Duration:"," (25.00 W) (2.44 ms) [Locked]"
```
* Setting `Package power limit lock: 0x9` to 0x0 changed
```
"CPU Power Limit 1 - Long Duration:"," (15.00 W) (28.00 sec) [Unlocked]"
"CPU Power Limit 2 - Short Duration:"," (25.00 W) (2.44 ms) [Unlocked]"
```
* PLx changes in ThrottleStop didn't become visible in HWInfo64 or active until I ticked the "Disable and Lock Turbo Power Limits" box in the FIVR menu.
* Mission accomplished! No more Power Limit Throttling during Cinebench or 3DMark.
* But now I was running into thermal limits with the CPU package at 100c.
* Adding thermal pads to the CPU heatsink and heatpipe to sink heat into the alumium bottom cover is pretty effective.
* I got [these](https://www.aliexpress.com/item/1005002223426783.html) thermal pads.
* I can keep Snowrunner running at ~20w, at around 80c, without throttling. Yay! Now it's a steady 14fps instead of dipping to 13 every now and then!

What didn't work
----------------
* Setting any of the following to 0x0 has no HWInfo64-visible effect.
```
Overclocking lock: 0x4
VR Current value lock: 0xE
Config TDP LOCK 0x23
CFG lock: 0x37
Platform power limit lock: 0x46
```
* Neither does setting `Cpu Power Limit1: 0x18` to 25.
* Or changing `Configurable TDP 0x22` from NOMINAL to UP.
* Intel XTU didn't work great for me. Changing the PL1 to 25w in XTU was visible in HWInfo64. 
* However, after 30-60 seconds of Cinebench it still limits to 15w with a Power Limit when using Intel XTU.
