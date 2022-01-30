# Gateway 2.000

This is the FDRS Multiprotocol Gateway sketch. The device listens for packets over ESP-NOW, UART, LoRa, and/or WiFi, then retransmits the packets over these interfaces using rules defined in the configuration file.

The most commonly used configuration tells the device to take any packet it receives and output the data over the serial port (UART):
```
#define ESPNOW1_ACT    sendSerial();
#define ESPNOW2_ACT    sendSerial();
#define ESPNOWG_ACT    sendSerial();
#define SERIAL_ACT     
#define MQTT_ACT       sendSerial();
#define LORA_ACT       sendSerial();
```
The companion for this device, connected via serial, takes any data it receives from the serial port and sends it via MQTT:
```
#define USE_WIFI
#define ESPNOW1_ACT  
#define ESPNOW2_ACT  
#define ESPNOWG_ACT  
#define SERIAL_ACT   sendMQTT();
#define MQTT_ACT     sendSerial();
#define LORA_ACT     sendSerial();
```
Splitting the gateway into two devices allows you to use ESP-NOW and WiFi simultaneously without channel conflicts. You can also connect the first device to a computer with a USB-UART adapter and get the data that way, eliminating WiFi altogether.

If you have sensors that are out of range of your first gateway, you can use a gateway as a repeater. First you will set the ESPNOW1 address to that of your first gateway, then you'll direct general ESP-NOW traffic to the ESPNOW1 interface:
```
#define UNIT_MAC     0x01  // THIS UNIT
#define ESPNOW1_MAC  0x00  // ESPNOW1 Address 
#define ESPNOW2_MAC  0x02  // ESPNOW2 Address

#define ESPNOW1_ACT  sendESPNOW(2);
#define ESPNOW2_ACT  sendESPNOW(1);
#define ESPNOWG_ACT  sendESPNOW(1);
#define SERIAL_ACT   
#define MQTT_ACT     
#define LORA_ACT  
```