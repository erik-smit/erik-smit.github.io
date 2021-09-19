Some notes while playing with WiFi Direct with my Sony ILCE-QX1

ILCE-QX1 Notes
=====
* To accept the WiFi Direct connection on the ILCE-QX1, hold the WiFi button in battery compartment for 3 seconds
* Add push button to highest order of preference, otherwise it'll ask for a pin you don't know
* `ActionList_URL` is determined through SSDP. `M-SEARCH` for `ST: urn:schemas-sony-com:service:ScalarWebAPI:1`.

Windows UWP WiFiDirect sample Notes
=================
* Pairing status doesn't seem to be passed back correctly in the `DeviceInformationUpdate`. It always shows unpaired, but trying to connect again says "PairAsync failed: Already Paired". Changing the test to `if (result.Status != DevicePairingResultStatus.Paired && result.Status != DevicePairingResultStatus.AlreadyPaired)` works to continue.
* "Connect operation threw an exception" is a red herring. The sample is hardcoded to make a TCP connection to 50001. It's not a WiFi Direct thing received from the peer.
* There's also an **DeviceEnumerationAndPairing** sample with WiFi Direct bits.
