# Windows Hello Biometrics with Synology Directory Server

My new Dell Latitude 7320 Detachable has Windows Hello support.  
Fingerprint, Face Unlock, etc.

Out of the box, this would only work with local accounts.

[Synology Forum](https://community.synology.com/enu/forum/1/post/138715) pointed me in the right direction, but says to disable Windows Hello for Business.  
Disabling or enabling "Windows Hello for Business" didn't work for me. Only setting it to "Not configured" allowed using Windows Hello Fingerprint.
This is also explained on [admx.help](https://admx.help/?Category=Windows_10_2016&Policy=Microsoft.Policies.MicrosoftPassportForWork::MSPassport_UsePassportForWork)

I ended up with the following policies in my GPO:

 * Windows Settings / Administrative Templates / System / Logon / Turn on convenience PIN sign-in : Enabled
 * Windows Settings / Administrative Templates / Windows Components / Biometrics / Allow domain users to log on using biometrics : Enabled
 * Windows Settings / Administrative Templates / Windows Components / Biometrics / Allow the use of biometrics : Enabled
 * Windows Settings / Administrative Templates / Windows Components / Biometrics / Allow users to log on using biometrics : Enabled
 * Windows Settings / Administrative Templates / Windows Components / Windows Hello for Business / Use Windows Hello for Business : Not configured
 
 
