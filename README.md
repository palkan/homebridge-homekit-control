<div align="center">

<a href="https://github.com/minamoanes/homebridge-homekit-control">
    <img src="https://github.com/minamoanes/homebridge-homekit-control/blob/master/homebridge-homekit-control.png" width="250" />
</a>

# Homebridge HomeKit Control

[![verified-by-homebridge](https://badgen.net/badge/homebridge/verified/purple)](https://github.com/homebridge/homebridge/wiki/Verified-Plugins)
[![npm](https://badgen.net/npm/v/homebridge-homekit-control)](https://www.npmjs.com/package/homebridge-homekit-control)
[![downloads](https://badgen.net/npm/dt/homebridge-homekit-control)](https://www.npmjs.com/package/homebridge-homekit-control)
[![donate](https://badgen.net/badge/PayPal/donate/003087?icon=https://simpleicons.now.sh/paypal/fff)](https://paypal.me/MinaMoanes)

</div>

**HomeKit Control** lets you control <img src="https://github.com/minamoanes/homebridge-homekit-control/blob/master/icons/homekit-icon.svg" width="15" /> Apple HomeKit Exclusive accessories through Homebridge, This is especially useful to gain all the benefits of Homebridge alongside controlling them using other services and plugins through Homebridge, such as:

- <img src="https://github.com/minamoanes/homebridge-homekit-control/blob/master/icons/google-assistant-icon.svg" width="15" /> Google Assistant (using `homebridge-gsh` plugin)
- <img src="https://github.com/minamoanes/homebridge-homekit-control/blob/master/icons/alexa-icon.svg" width="15" /> Amazon Alexa (using `homebridge-alexa` plugin)
- and many more...

## Supported Devices

The Plugin currently supports the following Services provided by WLAN-based HomeKit enabled devices:

- Switch
- Outlet
- Motion Sensors
- Temperatur Sensors
- Humidity Sensors
- CO2 Sensors
- Carbon Monoxid Sensors
- Air Quality Sensors
- Ambient Light Sensors
- Light Bulbs
- Battery/Charging State

Please note that BLE is currently not supported, as it seems to be unstable when multiple BLE-enabled Plugins are installed. All supported sensors on any configured device are automatically discovered and added when Homebridge starts up.

## Install

```
sudo npm i -g homebridge-homekit-control
```

## Pairing Devices

To pair your devices with Homebridge HomeKit Control, follow these steps:

1.  Discover Devices:
    ```
    homekitPair
    ```
    - You could also: `homekitPair [network] [interface]`, for example: `homekitPair -i wlan0`
2.  Select the device you want to pair, and enter the pairing code (including `-`, example `123-45-678`).
3.  Enter a filename to save pairing information in JSON format.
4.  In the JSON file, you'll find the following information:

    ```
    {
        "id": "device id",
        "name": "device name",
        "address": "device ip",
        "port": device-port,
        "pairingData": {
            "AccessoryPairingID": "xxx",
            "AccessoryLTPK": "xxx",
            "iOSDevicePairingID": "xxx",
            "iOSDeviceLTSK": "xxx",
            "iOSDeviceLTPK": "xxx"
        },
        "accessories": {
            "accessories": [...]
        }
    }
    ```

5.  Note down the `"id"`, `"name"`, `"address"`, `"port"` and `"pairingData"` from the JSON file.
6.  Go to **Plugins** page and Click on the **SETTINGS** button under **Homebridge Homekit Control** and fille out the information we noted from the JSON file in the previous steps
    - For further information, please refer to the [Config](#config) section for details.

## Config

This Plugin supports the Configuration Interface in Homebridge UI (homebridge-config-ui-x). Simply add a new Device and paste the values you obtained above.

Here are some additional configuration parameters, not obtained by the Device Pairing:

- `services.enableHistory` (_boolean_): **true** will enable fakeGato for compatible Sensors. This will allow you to view a history in the EVE-App
- `services.historyInterval` (optional, _numeric_): Determins the Interval used to take readings. The valus range is **[30, 600]**. The default value is **600**
- `services.proxyAll`(optional, _boolean_): By default we only proxy tested services and characteristics from your devices. When this mode is enabled, all available services and characteristics are proxied. Be carefull when activating, this may cause damage to your devices. The default value is **false**
- `services.uniquePrefix`(optional, _string_): When a device is added to multiple HomeBridge instances in the same network, you will need to provide a unique prefix. Otherwise Eve-App will get confused.

#### Example

```
{
    "platforms": [
        {
            "platform": "HomekitControl",
            "services": [
                {
                    "id": "device id",
                    "name": "device name",
                    "address": "device ip",
                    "port": "device port",
                    "enableHistory": false,
                    "pairingData": {
                        "AccessoryPairingID": "xxx",
                        "AccessoryLTPK": "xxx",
                        "iOSDevicePairingID": "xxx",
                        "iOSDeviceLTSK": "xxx",
                        "iOSDeviceLTPK": "xxx"
                    }
                }
            ]
        }
    ]
}
```

## How to fix Known Errors:

**if you get the error `M2: Error: 6`:**

1. Add the device to your Homekit
2. Once the device is added and connected to your Wifi network
3. Remove device from your Home
4. Device should still be connected to your Wifi (check it from your router's connected clients)
5. Run the `homekitPair` command in your Homebridge and enter the pin
6. And voila, you get the JSON

## Tested Accesories

(!!! Use at your own risk !!!)

- Quingping Air Monitor Lite (Cleargrass Luftdetektor Lite, Cleargrass Air Monitor Lite)
- Nanoleaf Aurora / Light Panels
- Gardena Smart Control Hub
  - Gardena Smart Irrigation Control
  - Gardena Smart Sensor II

Please let me know if you use this plugin and got it to work with a new HomeKit-Accesory that is not listed above

## Building for developers

The project was converted to the build system from https://github.com/homebridge/homebridge-plugin-template. Visual Studio Code is set up to lint on save.

Using `npm run dev` will launch the develop environment, where your code is automatically compiled on save and the local homebridge server is automatically restarted.

The command `npm rund build` will trigger the compilation of your project manually.
