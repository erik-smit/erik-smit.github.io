Some notes while playing with WiFi Direct with my Sony ILCE-QX1

ILCE-QX1 Notes
=====
* To accept the WiFi Direct connection on the ILCE-QX1, hold the WiFi button in battery compartment for 3 seconds
* Add push button to highest order of preference, otherwise it'll ask for a pin you don't know
* `ActionList_URL` is determined through SSDP. `M-SEARCH` for `ST: urn:schemas-sony-com:service:ScalarWebAPI:1`.

ILCE-QX1 Wifi Notes
===================
* WiFi Button in battery compartment cycles between 3 modes. No wifi, Single Wifi and Multi Wifi. 
* In "Multi Wifi" mode, the DIRECT:ILCE-QX1 SSID AP and WiFi Direct are no longer visible. 
* In "Multi Wifi" mode, when I hold the "wifi" button for a few seconds, the camera starts a slow repeating chime. I suspect it's looking for some WPS PushButton thing.

Windows UWP WiFiDirect sample Notes
=================
* Pairing status doesn't seem to be passed back correctly in the `DeviceInformationUpdate`. It always shows unpaired, but trying to connect again says "PairAsync failed: Already Paired". Changing the test to `if (result.Status != DevicePairingResultStatus.Paired && result.Status != DevicePairingResultStatus.AlreadyPaired)` works to continue.
* "Connect operation threw an exception" is a red herring. The sample is hardcoded to make a TCP connection to 50001. It's not a WiFi Direct thing received from the peer.
* There's also an **DeviceEnumerationAndPairing** sample with WiFi Direct bits.

Sony Remote Shooting Notes
===============
Sony has a few Remote protocols.
The ILCE-QX1 supports [Camera Remote API](https://developer.sony.com/develop/cameras/), which uses webtechnologies like SSDP, JSON-RDP over HTTP, etc. Camera Remote API was deprecated on 2020-02-11.

The new Sony [Camera Remote SDK](https://support.d-imaging.sony.co.jp/app/sdk/en/index.html), introduced on 2020-02-06, runs over PTP. 
And the PTP part is not documented, Sony just delivers binaries for Windows, Linux, macOS, implementing their new PTP protocol over USB and IP.
