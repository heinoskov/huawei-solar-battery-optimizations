# Huawei Solar Battery Optimization

This Home Assistant package provides a set of scripts and automations to optimize the use of a Huawei solar battery system. It aims to maximize self-consumption, optimize charging/discharging of the Huawei Battery based on electricity prices and solar forecast, as well as manage grid export based on electricity spot prices. In Denmark we can even have negative spot prices and this package will manage these situation and disable grid export.

## Features

- Daily optimization of battery usage
- Seasonal mode switching
- Grid export management based on spot prices
- Next-day charging strategy calculation
- Various working modes (Time-of-Use, Maximize Self-Consumption, Fully Fed to Grid)

## Requirements

To use this package, you need the following integrations:

- [Huawei Solar integration by wlcrs](https://github.com/wlcrs/huawei_solar)
- [Solcast integration by oziee](https://github.com/BJReplay/ha-solcast-solar)
- [Energi Data Service integration by MTrab](https://github.com/MTrab/energidataservice)
- [Huawei Solar PEES package by JensenNick](https://github.com/JensenNick/huawei_solar_pees) (optional but recommended)

## Installation

1. Place the `huawei_solar_battery_optimization.yaml` file in your Home Assistant packages directory (usually `/config/packages/`).
2. Restart Home Assistant to load the new package.

For more information on using packages in Home Assistant, see the [official documentation](https://www.home-assistant.io/docs/configuration/packages/).

## Configuration

You may need to adjust the following input sensors in the package file to match your setup:

- `sensor.solcast_pv_forecast_forecast_today` (from Solcast integration)
- `sensor.energi_data_service_total_price` (from Energi Data Service integration)
- `sensor.energi_data_service_spot_price` (from Energi Data Service integration)
- `sensor.batteries_state_of_capacity` (from Huawei Solar Integration)
- `sensor.inverter_active_power_control` (from Huawei Solar Integration)

## How It Works

### Daily Processes

- **6:00 AM**: Daily optimization runs, calculating energy thresholds, analyzing hourly data, and generating a summary.
- **6:05 AM**: The system implements the daily optimization, setting the working mode to Time-of-Use with disabled battery, and potentially scheduling a mode change.
- **Every 5 minutes**: The system checks if it's time to change the mode to Maximize Self-Consumption.
- **5:00 PM**: The system switches to an appropriate mode based on the current month.
- **9:00 PM**: The system calculates the charging strategy for the next day.

### Monthly Process

- On the 1st of each month, the system checks and sets the appropriate working mode based on the season.

### Continuous Processes

- The system manages inverter export based on spot prices, triggered by price changes or hourly checks.

## Available Scripts

The package includes several scripts for setting different working modes, managing grid export, and calculating charging strategies. Some of these are used in automations, while others are available for manual use or further automation improvements.

## Roadmap

Future planned features include:

1. Adjustable AMP configuration for EV Charging (Wallbox Pulsar Plus) based on excessive PV production after house load.
2. Peak shaving / Capacity Control.

## Contributing

This package is created by Heino Skov. While no official support is provided, you're free to use and edit it to your needs. Contributions, suggestions, and improvements are welcome via GitHub issues and pull requests.

## License

This project is open-source and available under the [MIT License](LICENSE).

## Disclaimer

This package is provided as-is, without any warranty or guarantee of its functionality or suitability for any particular purpose. Use at your own risk.

