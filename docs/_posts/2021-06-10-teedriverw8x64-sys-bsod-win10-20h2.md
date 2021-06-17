For some reason Microsoft supplies a driver with Windows Update for the Intel Management Engine that BSODs on load. Intel stopped updating the driver for the ME on my Supermicro X10SLQ and we're left with an unbootable system. It's kinda annoying to work around.

  * Let it BSOD and reboots until you get into WinRE. 
  * Go into command-line.
  * 
```
cd \windows
dir TeeDriverW8x64.sys /s
```
  * For every instance of *TeeDriverW8x64.sys*
```
copy con <TeeDriverW8x64.sys>
```
  * Reboot back into a working Windows.
  * Go to Device Manager, search for Intel(R) Management Engine Interface with <!>.
  * Disable device.


Things I tried but didn't work:
  * Deleting TeeDriverW8x64.sys and replacing with `copy con TeeDriverW8x64.sys<ENTER><F6>` and `attrib +r TeeDriverW8x64.sys`. Windows puts it back.
  * Moving TeeDriverW8x64.sys from C:\Windows\System32\Drivers\. Windows puts it back.
  * In WinRE: Removing drivers with pnputil, but this edits the driverstore in WinRE, not on c:\
  * In WinRE: Removing drivers with dism /remove-drivers (https://superuser.com/questions/621604/how-to-uninstall-a-driver-from-recovery-console)  but windows kept reinstalling it on reboot.
  * Booting in safemode rejected because install wasn't completed.
