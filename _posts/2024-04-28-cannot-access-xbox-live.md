Notes so far:

https://answers.microsoft.com/en-us/xbox/forum/all/cannot-access-xbox-live-at-all-on-pc/b3e1547d-14bd-496c-8854-2246ddb3edb9

PS C:\Users\erikl> type C:\Windows\Logs\CBS\CBS.log
2024-04-28 08:12:28, Info                  DEPLOY [Pnp] Corrupt file: C:\Windows\System32\drivers\BthHfEnum.sys
2024-04-28 08:12:28, Info                  DEPLOY [Pnp] Repaired file: C:\Windows\System32\drivers\BthHfEnum.sys
2024-04-28 08:12:28, Info                  DEPLOY [Pnp] Corrupt file: C:\Windows\System32\drivers\bthmodem.sys
2024-04-28 08:12:28, Info                  DEPLOY [Pnp] Repaired file: C:\Windows\System32\drivers\bthmodem.sys

PS C:\Users\erikl> Dism /Online /Cleanup-Image /ScanHealth

Deployment Image Servicing and Management tool
Version: 10.0.22621.2792

Image Version: 10.0.22631.3447

[==========================100.0%==========================] The component store is repairable.
The operation completed successfully.
PS C:\Users\erikl> Dism /Online /Cleanup-Image /RestoreHealth

Deployment Image Servicing and Management tool
Version: 10.0.22621.2792

Image Version: 10.0.22631.3447

[==========================100.0%==========================] The restore operation completed successfully.
The operation completed successfully.
PS C:\Users\erikl>
