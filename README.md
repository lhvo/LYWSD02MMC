 
# How to collect Temp and Humidity from XIAOMI clock with out MI HOME account - RASPBERRY PI
# LYWSD02MMC

![LYWSD02MMC](https://user-images.githubusercontent.com/32024959/150021795-dc90d4a5-64a9-40ae-bc4f-82fbd330171d.jpg)

# LYWSDCGQ/01ZM
![LYWSDCGQ-01ZM](https://user-images.githubusercontent.com/32024959/150075836-ea2589b2-6772-448d-9198-034720fc4acf.jpg)


# LYWSD03MMC

![LYWSD03MMC](https://user-images.githubusercontent.com/32024959/150021528-b9583cac-ff0a-4df0-8ffa-3ae525ca1bfe.jpg)


# GATTTOOL command
As a conclusion the gatttool command to be used is :
>gatttool --device="MAC Addr" --char-write-req -a 0x004c -n 0100  --listen  
Characteristic value was written successfully  
Notification handle = 0x004b value: c3 0a 36  
Notification handle = 0x004b value: c6 0a 36  

For the model: LYWSD03MMC
>gatttool --device="MAC Addr" --char-write-req -a 0x0038 -n 0100  --listen  
  
# Value conversions
>The temperature is c3 0a (little endian) => 0x0aC3(hexa) is 2755 (decimal) => 27°55 Celcius  
>The Humidity is 36 => 0x36(hexa) is 54(decimal) => 54% humidity.  

#Some informations about the approach to track and identification of the attributes: 
1- BLE Characteristic identification
To look for Temperature & Humidity characteristic - Using BLE Scanner.  
With the "Characteristic" can be identified. In the case of LYWSD02MMC the UUID is ebe0ccc1-7a0a-4b0c-8a1a-6ff2997da3a6.
2- BLE communication tracking
On one terminal run the "hcidump -X" to dump the BLE communication.
3- Turn on the notification 
On a second terminal run "bluetoothctl"  
Run "scan on" to detect and identify the MAC of the devices  
Run "connect <MAC Addr>" to connect to the device. 
A list of characteristic will be displayed. Identify the attribute wit the appropriate UUID (ebe0ccc1-7a0a-4b0c-8a1a-6ff2997da3a6) 
Run "select-attribute /org/bluez/hci0/dev_E7_2E_00_80_99_07/service003c/char004a" 
  
Turn the notification on "notify on"  

Check on the "hcidump" terminal - to identify the handle.  
  
> ACL data: handle 64 flags 0x00 dlen 9  
  ATT: Write req (0x12)  
     handle 0x004c value  0x01 0x00  
 ACL data: handle 64 flags 0x02 dlen 5  
   ATT: Write resp (0x13)  
 HCI Event: Number of Completed Packets (0x13) plen 5  
    handle 64 packets 1  

In our case we did see that the handle "0x004c" is used.
