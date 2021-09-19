Some notes while playing with WiFi Direct with my Sony ILCE-QX1

ILCE-QX1 Notes
=====
* To accept the WiFi Direct connection on the ILCE-QX1, hold the WiFi button in battery compartment for 3 seconds
* Add push button to highest order of preference, otherwise it'll ask for a pin you don't know

Windows UWP WiFiDirect sample Notes
=================
The WiFiDirect sample has some issues for me
* Pairing status doesn't seem to be passed back correctly in the `DeviceInformationUpdate`. It always shows unpaired, but trying to connect again says "PairAsync failed: Already Paired".
* "Connect operation threw an exception" is a red herring. The 50001 port it shows is hardcoded in the sample. It's not a WiFi Direct thing received from the peer.
