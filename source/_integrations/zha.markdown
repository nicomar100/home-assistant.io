---
title: Zigbee Home Automation
description: Instructions on how to integrate your Zigbee Home Automation (ZHA) devices within Home Assistant.
ha_category:
  - Hub
  - Binary Sensor
  - Climate
  - Fan
  - Light
  - Lock
  - Sensor
  - Switch
  - Cover
ha_release: 0.44
ha_iot_class: Local Polling
featured: true
ha_config_flow: true
ha_codeowners:
  - '@dmulcahey'
  - '@adminiuga'
ha_domain: zha
---

[Zigbee Home Automation](https://zigbeealliance.org) (ZHA)
integration for Home Assistant allows you to connect many off-the-shelf Zigbee based devices to Home Assistant, using one of the available Zigbee radio modules that is compatible with [zigpy](https://github.com/zigpy/zigpy) (an open source Python library implementing a Zigbee stack, which in turn relies on separate libraries which can each interface a with Zigbee radio module a different manufacturer).

There is currently support for the following device types within Home Assistant:

- Binary Sensor
- Climate (beta)
- Cover
- Fan
- Light
- Lock
- Sensor
- Switch

There is also support for grouping of lights, switches, and fans (i.e. support for commanding device groups as entities). At least two entities must be added to a group before the group entity is created.

## ZHA exception and deviation handling

Zigbee devices that deviate from or do not fully conform to the standard specifications set by the [Zigbee Alliance](https://zigbeealliance.org) may require the development of custom [ZHA Device Handlers](https://github.com/dmulcahey/zha-device-handlers) (ZHA custom quirks handler implementation) to for all their functions to work properly with the ZHA integration in Home Assistant. These ZHA Device Handlers for Home Assistant can thus be used to parse custom messages to and from Zigbee devices.

The custom quirks implementations for zigpy implemented as ZHA Device Handlers for Home Assistant are a similar concept to that of [Hub-connected Device Handlers for the SmartThings Classics platform](https://docs.smartthings.com/en/latest/device-type-developers-guide/) as well as that of [Zigbee-Shepherd Converters as used by Zigbee2mqtt](https://www.zigbee2mqtt.io/how_tos/how_to_support_new_devices.html), meaning they are each virtual representations of a physical device that expose additional functionality that is not provided out-of-the-box by the existing integration between these platforms.

## Compatible hardware

ZHA integration uses a hardware independent Zigbee stack implementation with modular design which means that it can support any one of the many Zigbee coordinator radio modules/adapters available from different manufacturers, as long as that module/adapter is compatible with [zigpy](https://github.com/zigpy/zigpy).

### Known working Zigbee radio modules

- dresden elektronik deCONZ based Zigbee radios (via the [zigpy-deconz](https://github.com/zigpy/zigpy-deconz) library for zigpy)
  - [ConBee II (a.k.a. ConBee 2) USB adapter from dresden elektronik](https://phoscon.de/conbee2)
  - [ConBee USB adapter from dresden elektronik](https://phoscon.de/conbee)
  - [RaspBee II (a.k.a. RaspBee 2) Raspberry Pi Shield from dresden elektronik](https://www.dresden-elektronik.com/product/raspbee-II.html)
  - [RaspBee Raspberry Pi Shield from dresden elektronik](https://phoscon.de/raspbee)
- Silicon Labs EmberZNet based radios using the EZSP protocol (via the [bellows](https://github.com/zigpy/bellows) library for zigpy)
  - [ITEAD Sonoff ZBBridge](https://www.itead.cc/smart-home/sonoff-zbbridge.html) (Note! This first have to be flashed with [Tasmota firmware and Silabs EmberZNet NCP EZSP UART Host firmware](https://www.digiblur.com/2020/07/how-to-use-sonoff-zigbee-bridge-with.html))
  - [Nortek GoControl QuickStick Combo Model HUSBZB-1 (Z-Wave & Zigbee USB Adapter)](https://www.nortekcontrol.com/products/2gig/husbzb-1-gocontrol-quickstick-combo/)
  - [Elelabs Zigbee USB Adapter](https://elelabs.com/products/elelabs_usb_adapter.html)
  - [Elelabs Zigbee Raspberry Pi Shield](https://elelabs.com/products/elelabs_zigbee_shield.html)
  - Bitron Video/Smabit BV AV2010/10 USB-Stick with Silicon Labs Ember 3587
  - Telegesis ETRX357USB (Note! This first have to be flashed with other EmberZNet firmware)
  - Telegesis ETRX357USB-LRS (Note! This first have to be flashed with other EmberZNet firmware)
  - Telegesis ETRX357USB-LRS+8M (Note! This first have to be flashed with other EmberZNet firmware)
- XBee Zigbee based radios (via the [zigpy-xbee](https://github.com/zigpy/zigpy-xbee) library for zigpy)
  - Digi XBee Series 3 (xbee3-24) modules
  - Digi XBee Series 2C (S2C) modules
  - Digi XBee Series 2 (S2) modules (Note! This first have to be flashed with Zigbee Coordinator API firmware)

### Experimental support for additional Zigbee radio modules

- Texas Instruments CC253x, CC26x2R, and CC13x2 based radios (via the [zigpy-cc](https://github.com/zigpy/zigpy-cc) library for zigpy)
  - [CC2531 USB stick hardware flashed with custom Z-Stack coordinator firmware from the Zigbee2mqtt project](https://www.zigbee2mqtt.io/getting_started/what_do_i_need.html)
  - [CC2530 + CC2591/CC2592 USB stick hardware flashed with custom Z-Stack coordinator firmware from the Zigbee2mqtt project](https://www.zigbee2mqtt.io/getting_started/what_do_i_need.html)
  - [CC2538 + CC2592 dev board hardware flashed with custom Z-Stack coordinator firmware from the Zigbee2mqtt project](https://www.zigbee2mqtt.io/getting_started/what_do_i_need.html)
  - [CC2652P/CC2652R/CC2652RB USB stick or dev board hardware flashed with custom Z-Stack coordinator firmware from the Zigbee2mqtt project](https://www.zigbee2mqtt.io/information/supported_adapters)
  - [CC1352P/CC1352R USB stick or dev board hardware flashed with custom Z-Stack coordinator firmware from the Zigbee2mqtt project](https://www.zigbee2mqtt.io/information/supported_adapters)
- ZiGate based radios (via the [zigpy-zigate](https://github.com/zigpy/zigpy-zigate) library for zigpy and require firmware 3.1a or later)
  - [ZiGate USB-TTL](https://zigate.fr/produit/zigate-ttl/)
  - [ZiGate USB-DIN](https://zigate.fr/produit/zigate-usb-din/)
  - [PiZiGate](https://zigate.fr/produit/pizigate-v1-0/)
  - [Wifi ZiGate](https://zigate.fr/produit/zigate-pack-wifi-v1-3/)

## Configuration - GUI

From the Home Assistant front page go to **Configuration** and then select **Integrations** from the list.

Use the plus button in the bottom right to add a new integration called **ZHA**.

In the popup:

- Serial Device Path - List of detected serial ports on the system. You need to pick one to which your
radio is connected
- Submit

Press `Submit` and the integration will try to detect radio type automatically. If unsuccessful, you will get
a new pop-up asking for a radio type. In the pop-up:

- Radio Type

| Radio Type | Zigbee Radio Hardware |
| ------------- | ------------- |
| `ezsp`  | Silicon Labs EmberZNet protocol (ex; Elelabs, HUSBZB-1, Telegesis) |
| `deconz` | dresden elektronik deCONZ protocol: ConBee I/II, RaspBee I/II |
| `ti_cc` | Texas Instruments Z-Stack ZNP protocol (ex: CC253x, CC26x2, CC13x2) |
| `zigate` | ZiGate Serial protocol: PiZiGate, ZiGate USB-TTL, ZiGate WiFi |
| `xbee` | Digi XBee ZB Coordinator Firmware protocol (Digi XBee Series 2, 2C, 3) |

- Submit

Press `Submit` to save radio type and you will get a new form asking for port settings specific for this
radio type. In the pop-up:
- Serial device path
- port speed (not applicable for all radios)
- data flow control (not applicable for all radios)

Most devices need at very least the serial device path, like `/dev/ttyUSB0`, but it is recommended to use
device path from `/dev/serial/by-id` folder,
e.g., `/dev/serial/by-id/usb-Silicon_Labs_HubZ_Smart_Home_Controller_C0F003D3-if01-port0`

Press `Submit`. The success dialog will appear or an error will be displayed in the popup. An error is likely if Home Assistant can't access the USB device or your device is not up to date. Refer to [Troubleshooting](#troubleshooting) below for more information.

If you are use ZiGate or Sonoff ZBBridge you have to use some special usb_path configuration:

- ZiGate USB TTL or DIN: `/dev/ttyUSB0` or `auto` to auto discover the zigate
- PiZigate : `pizigate:/dev/serial0`
- Wifi Zigate : `socket://[IP]:[PORT]` for example `socket://192.168.1.10:9999`
- Sonoff ZBBridge : `socket://[IP]:[PORT]` for example `socket://192.168.1.11:8888`

{% configuration %}
database_path:
  description: _Full_ path to the database which will keep persistent network data.
  required: true
  type: string
enable_quirks:
  description: Enable quirks mode for devices where manufacturers didn't follow specs.
  required: false
  type: boolean
  default: true
{% endconfiguration %}

To add new devices to the network, call the `permit` service on the `zha` domain. Do this by clicking the Service icon in Developer tools and typing `zha.permit` in the **Service** dropdown box. Next, follow the device instructions for adding, scanning or factory reset.

### OTA firmware updates

ZHA component does have the ability to automatically download and perform OTA (Over-The-Air) firmware updates of Zigbee devices if the OTA firmware provider source URL for updates is available. OTA firmware updating is set to disabled (`false`) in the configuration by default.

Currently, OTA providers for firmware updates are only available for IKEA and LEDVANCE devices. OTA updates for device of other manufactures could possible also be supported by ZHA dependencies in the future, if these manufacturers publish their firmware publicly.

To enable OTA firmware updates for the ZHA integration you need to add the following configuration to your `configuration.yaml` and restart Home Assistant:

```yaml
zha:
  zigpy_config:
    ota:
      ikea_provider: true                        # Auto update Trådfri devices
      ledvance_provider: true                    # Auto update LEDVANCE devices
      #otau_directory: /path/to/your/ota/folder  # Utilize .ota files to update everything else
```

You can choose if the IKEA or LEDVANCE provider should be set to enabled (`true`) or disabled (`false`) individually. After the OTA firmware upgrades are finished, you can set these to `false` again if you do not want ZHA to automatically download and perform OTA firmware upgrades in the future.

Note that the `otau_directory` setting is optional and can be used for any firmware files you have downloaded yourself.

## Adding devices

To add a new device:

1. Go to the **Integrations** page, find the **Zigbee Home Automation** integration that was added by the configuration steps above, and select **Configure**.
1. Click on the plus button at the bottom right corner to start a scan for new devices.
1. Reset your Zigbee devices according to the device instructions provided by the manufacturer (e.g., turn on/off lights up to 10 times, switches usually have a reset button/pin). It might take a few seconds for the devices to appear. You can click on **Show logs** for more verbose output.
1. Once the device is found, it will appear on that page and will be automatically added to your devices. You can optionally change its name and add it to an area (you can change this later). You can search again to add another device, or you can go back to the list of added devices.

## Troubleshooting

### Reporting issues

When reporting issues, please provide the following information in addition to information requested by issue template:

1. Debug logs for the issue, see [debug logging](#debug-logging)
2. Model of Zigbee radio being used
3. If issue is related to a specific Zigbee device, provide device Zigbee signature. Signature is available at
**Configuration** -> **Integrations** -> **Zigbee Home Automation** (click **Configure**) -> **Devices** (pick your device) -> **Zigbee Device Signature**

### Debug logging

To enable debug logging for ZHA component and radio libraries, add the following [logger](/integrations/logger/) configuration to `configuration.yaml`:

```yaml
logger:
  default: info
  logs:
    homeassistant.core: debug
    homeassistant.components.zha: debug
    bellows.zigbee.application: debug
    bellows.ezsp: debug
    zigpy: debug
    zigpy_cc: debug
    zigpy_deconz.zigbee.application: debug
    zigpy_deconz.api: debug
    zigpy_xbee.zigbee.application: debug
    zigpy_xbee.api: debug
    zigpy_zigate: debug
    zhaquirks: debug
```

### Add Philips Hue bulbs that have previously been added to another bridge

Philips Hue bulbs that have previously been added to another bridge won't show up during search. You have to restore your bulbs back to factory settings first. To achieve this, you basically have the following options.

#### Philips Hue Dimmer Switch

Using a Philips Hue Dimmer Switch is probably the easiest way to factory-reset your bulbs. For this to work, the remote doesn't have to be paired with your previous bridge.

1. Turn on your Hue bulb you want to reset
2. Hold the Dimmer Switch near your bulb (< 10 cm)
3. Press and hold the (I)/(ON) and (O)/(OFF) buttons of the Dimmer Switch for about 10 seconds until your bulb starts to blink
4. Your bulb should stop blinking and eventually turning on again. At the same time, a green light on the top left of your remote indicates that your bulb has been successfully reset to factory settings.

#### hue-thief

Follow the instructions on [https://github.com/vanviegen/hue-thief/](https://github.com/vanviegen/hue-thief/) (EZSP-based Zigbee USB stick required)

### ZHA Start up issue with Home Assistant or Home Assistant Container

On Linux hosts ZHA can fail to start during HA startup or restarts because the Zigbee USB device is being claimed by the host's modemmanager service. To fix this disable the modemmanger on the host system.

To remove modemmanager from a Debian/Ubuntu host run this command:

```bash
sudo apt-get purge modemmanager
```

### Can't connect to USB device and using Docker

If you are using Docker and can't connect, you most likely need to forward your device from the host machine to the Docker instance. This can be achieved by adding the device mapping to the end of the startup string or ideally using Docker compose.

#### Docker Compose

Install Docker-Compose for your platform (Linux - `sudo apt-get install docker-compose`).

Create a `docker-compose.yml` with the following data:

```yaml
version: '2'
services:
  homeassistant:
    # customisable name
    container_name: home-assistant

    # must be image for your platform, this is the rpi3 variant
    image: homeassistant/raspberrypi3-homeassistant
    volumes:
      - <DIRECTORY HOLDING HOME ASSISTANT CONFIG FILES>:/config
      - /etc/localtime:/etc/localtime:ro
    devices:
      # your usb device forwarding to the docker image
      - /dev/ttyUSB0:/dev/ttyUSB0
    restart: always
    network_mode: host
```
