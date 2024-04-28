# We couldn't sign you into Xbox Live

- Started to get this message yesterday whenever trying to start a Xbox Game Pass game, stopping me from playing "Spacelines from the Far Out".
- For a while now, after every reboot Edge would complain about the sign-in that it couldn't sync and needed to confirm sign-in data.

# What didn't work
- wsreset
- signing out out edge, xbox, etc. and signing in again

# What did work

Followed the steps on: https://answers.microsoft.com/en-us/xbox/forum/all/cannot-access-xbox-live-at-all-on-pc/b3e1547d-14bd-496c-8854-2246ddb3edb9

```
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
```

After a reboot, Edge no longer complains and I can continue Xbox Game Pass gaming.
