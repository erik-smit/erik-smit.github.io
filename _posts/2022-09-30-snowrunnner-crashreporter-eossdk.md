# Snowrunner "EOSSDK-Win64-Shipping.dll was not found"

Just wasted some time debugging "EOSSDK-Win64-Shipping.dll was not found" with Snowrunner.

This error is a red herring. This error is actually the crash_reporter.exe crashing.  
The crash_reporter.exe relies on "EOSSDK-Win64-Shipping.dll" for some reason.  
I found c:\users\user\appdata\local\saber\crashdump.dmp. 
Loading this in a dumpviewer showed the original crash in the ati driver.
