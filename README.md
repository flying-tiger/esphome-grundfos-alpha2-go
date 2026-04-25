# ESPHome Component for Grundfos Alpha2 GO Pumps
ESPHome component for reading measurement values from Grundfos Alpha2 Go pumps via bluetooth on esphome devices.

Original code is from [https://github.com/esphome/esphome/tree/dev/esphome/components/alpha3](https://github.com/esphome/esphome/tree/dev/esphome/components/alpha3) <br />
I just adjusted: <br />
geni_request_flow_head <br />
geni_request_power <br />
GENI_RESPONSE_TYPE_FLOW_HEAD <br />
GENI_RESPONSE_TYPE_POWER <br />
to the new(?) values that work with the Alpha2 GO. <br />
I used Android's HCI logging feature + Wireshark to find the values (while using the official Grundfos Go app). <br />

If anyone wants to use this to add official support in esphome feel free to do so. I probably won't get around to it (and figuring out if/how it should maybe be integrated into a more general grundfos Alpha component because maybe these new values are also necessary for new models of the alpha3 pump).

### flying-tiger updates
* Reorganize repository for use as non-local external component.

## How to add it as a external component in your home assistant

Add the following as your esphome device configuration:

```
external_components:
 - source: github://flying-tiger/esphome-grundfos-alpha2-go
   components: [alpha2]

esp32_ble_tracker:

ble_client:
  - mac_address: XX:XX:XX:XX:XX:XX # your pump's bluetooth mac here
    id: heatingpump

sensor:
  - platform: alpha2
    ble_client_id: heatingpump
    flow:
      name: "Pump Flow"
    head:
      name: "Pump Pressure"
    speed:
      name: "Pump Speed"
    power:
      name: "Pump Power"
    voltage:
      name: "Pump Voltage"
    current:
      name: "Pump Current"
```

### Local Method
1. Access your /config/esphome/ folder (e.g. via the SMB Addon in HassOS)
2. Create a folder named "components" if it does not exit
3. Copy the "alpha2" folder from the "components" folder in this repository to the
   one on your machine

In your esphome device yaml config add this:

```
external_components:
 - source:
     type: local
     path: components
   components: [alpha2]

esp32_ble_tracker:

ble_client:
  - mac_address: XX:XX:XX:XX:XX:XX # your pump's bluetooth mac here
    id: heatingpump

sensor:
  - platform: alpha2
    ble_client_id: heatingpump
    flow:
      name: "Pump Flow"
    head:
      name: "Pump Pressure"
    speed:
      name: "Pump Speed"
    power:
      name: "Pump Power"
    voltage:
      name: "Pump Voltage"
    current:
      name: "Pump Current"
```

For the first connection I think you need to press the "wifi" button on the pump to put it into pairing mode. <br />
To find out your pump's bluetooth mac use an app like e.g. [nRF Connect](https://play.google.com/store/apps/details?id=no.nordicsemi.android.mcp) .
