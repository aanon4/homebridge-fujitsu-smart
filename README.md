# homebridge-fujitsu-smart
## Homebridge plug in for Fujistu Mini Split with web-based scheduling UI and smart temperature support.

This started life as an extension to the current mini-split module but with fan control, and rapidly got out of hand ...

**Warning** this plugin this is currently experimental, use at your own risk! It will probably burn your house down.

![UI](./assets/ui.png)
![Multiple Schedules](./assets/sched.png)
![Room Temps](./assets/sched2.png)
![Graphs](./assets/graph.png)

## Installation

1. Install [homebridge](https://github.com/nfarina/homebridge#installation-details)
2. Install this plugin: `npm install -g homebridge-fujitsu-smart`
3. Edit your `config.json` file (See below).

```json
"accessories": [
    {
        "accessory": "FGLairSmartThermostat",
        "name": "Fujitsu Mini Spit",
        "username": "FGLAIR USERNAME",
        "password": "FGLAIR PASSWORD",
        "temperatureDisplayUnits": 1,
        "smart": {
            "miio": {
                "username": "MII USERNAME",
                "password": "MII PASSWORD",
                "region": "cn"
            },
            "feelslike": true,
            "portnr": 8080,
            "weather": {
                "key": "OPENWEATHERMAPAPI KEY",
                "zipcode": "94707"
            }
        }
    },
]
```
| Key | Description |
| --- | --- |
| `accessory` | Must be `FGLairSmartThermostat` |
| `name` | Name to appear in the Home app |
| `username` | `FGLair` Username |
| `password` | `FGLair` Password |
| `model` _(optional)_ | Appears under "Model" for your accessory in the Home app |
| `region` _(optional)_ | Region for thermostat, change for China & E.U. (Default: "us") |
| `temperatureDisplayUnits` _(optional)_ | Celcius (0) or Fahrenheit (1) (Default: 0) |
| `includeFilter` _(optional)_ | Include a filter fan service. The filter fan is run when the program doesnt need to heat or cool and keeps the air moving through the system air filter (Default: false) |
| `smart` _(optional)_ | Smart configuration. |
| `smart.miio` _(optional)_ | Use multiple temperature and occupancy sensors to adapted thermostat. Currently supports the `Mi Home` platform |
| `smart.miio.username` | `Mi Home` Username |
| `smart.miio.password` | `Mi Home` Password |
| `smart.miio.region` _(optional)_ | Will default to `cn` which supports the latest range of sensor types, but can be set to other regions |
| `smart.feeslike` _(optional)_ | If `true` the temperatures will be adjusted based on the humidity, to better refect the temperatures rooms feel |
| `smart.portnr` _(optional)_ | Port number for Web UI (Default: 8080) |
| `smart.weather` _(optional)_ | If provided, allows the Web UI to display the current weather. Weather information is read from the OpenWeatherMap API. Provide one of `city`, `latLong`, `cityId` or `zipcode` |
| `smart.weather.key` | OpenWeatherMap API key |
| `smart.weather.lang` _(optional)_ | Language for weather information (Default: en) |
| `smart.weather.city` _(optional)_ | City name for weather |
| `smart.weather.latLong` _(optional)_ | `[Longitude,Latitude]` for weather |
| `smart.weather.cityId` _(optional)_ | OpenWeatherMap city id for weather |
| `smart.weather.zipcode` _(optional)_ | Zipcode for weather |

## Smart operation
Smart operation uses Xiomi temperature, humidity and movement sensors to adjust the thermostat target temperature depending on where you are in your house (in a similar way to the Ecobee thermostat) and the schedule defined. Smart operation takes over the scheduling from your home thermostat and you should disable it.

Sensors must be registered using the `Mi Home` application.

This plugin will read the sensors periodically, then caclulate a weighted temperature based on the defined schedule, and adjust the thermostat based on this. If the wall temperature is changed by hand, this will override the schedule for a set period of time before the schedule resumes.

## Current limitations
- Only one air conditioner is displayed, the API chooses the first device.  I only have one system, so feel free to contribute changes if you have more than one A/C unit.
- Timeout on token is not enabled, when token is invalid the API will re-authenticate.
- Auth. Token is not cached
- Previous thermostat state is not cached
- For US users, Aqara devices seem like the obvious sensor choice. If anyone knows how to easily read these (no ferreting around for tokens in encypted backups) please let me know as I've had no luck :-(

## Wouldn't it be nice ...
- If HomeKit devices could be allowed read other HomeKit devices state (e.g. sensors). I realize there are security concerns here, but still ...

## TODO
- **Lots and lots of testing. Seriously this could burn your house down at the moment.**

## Contributions
Portions of this software adapted from the projects listed below.  A huge thank you, for all their work.

- The original homebridge-fujitsu plugin https://github.com/smithersDBQ/homebridge-fujitsu

- The pyfujitsu project under Apache License
Copyright (c) 2018 Mmodarre https://github.com/Mmodarre/pyfujitsu

- The homebridge-dummy-thermostat under the Unlicense
https://github.com/X1ZOR/homebridge-dummy-thermostat

- The node-mihome library https://github.com/maxinminax/node-mihome
