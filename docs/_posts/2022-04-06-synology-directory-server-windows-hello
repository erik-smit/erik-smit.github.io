# Windows Hello Biometrics with Synology Directory Server

My new Dell Latitude 7320 Detachable has Windows Hello support.  
Fingerprint, Face Unlock, etc.

Out of the box, this would only work with local accounts.

https://community.synology.com/enu/forum/1/post/138715 pointed me in the right direction, but says to disable Windows Hello for Business.  
For me I need to set it to "Not Configured", otherwise it either disables it fully, or tries the key/certificate thing.  
This is also explained on https://admx.help/?Category=Windows_10_2016&Policy=Microsoft.Policies.MicrosoftPassportForWork::MSPassport_UsePassportForWork

I ended up with the following policies in my GPO:

 * Windows Settings / Administrative Templates / System / Logon / Turn on convenience PIN sign-in : Enabled
 * Windows Settings / Administrative Templates / Wiundows Components / Biometrics / Allow domain users to log on using biometrics : Enabled
 * Windows Settings / Administrative Templates / Wiundows Components / Biometrics / Allow the use of biometrics : Enabled
 * Windows Settings / Administrative Templates / Wiundows Components / Biometrics / Allow users to log on using biometrics : Enabled
 * Windows Settings / Administrative Templates / Wiundows Components / Windows Hello for Business / Use Windows Hello for Business : Not configured
 
 
