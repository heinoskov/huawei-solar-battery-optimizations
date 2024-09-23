# huawei-solar-battery-optimizations
This yaml package file for Home Assistant provides a set of scripts and automations to optimize the use of a Huawei solar battery system.

# Huawei Solar Battery Optimization
huawei_solar_battery_optimization.yaml
Place this file in your packages directory (usually /config/packages/)
See Home Assistant Packages documentation for further details on how to use this
Packages in Home Assistant: https://www.home-assistant.io/docs/configuration/packages/
This packages is created by Heino Skov. I do not provide any support on this but its free to use and
edit to your needs. 

This package provides a set of scripts and automations to optimize the use of a Huawei solar battery system.
The scripts and automations in this package is setup for a 10kWh inverter and 10kW battery from Huawei.
It aims to maximize self-consumption, optimize charging/discharging of Huawei Battery based on electricity prices,
and manage grid export based on spot prices.

--------------------------------------------------------------------------------------
# Brief explanation #
Daily processes:
The daily optimization runs at 6:00 AM, triggering energy threshold calculations, hourly data analysis, and
summary generation.
At 6:05 AM, the system implements the daily optimization, initially setting the working mode to TOU with 
disabled battery, and potentially scheduling a mode change.
Throughout the day, the system checks every 5 minutes if it's time to change the mode to Maximize Self Consumption.
At 5:00 PM, the system switches to an appropriate mode based on the month.
At 9:00 PM, the system calculates the charging strategy for the next day, setting either TOU or Maximise Self 
Consumption for the coming night

Monthly process:
On the 1st of each month, the system checks and sets the appropriate working mode based on the season.

Continuous processes:
The system manages inverter export based on spot prices, triggered by price changes or hourly checks.

Available scripts:
There is various scripts available for setting different working modes, managing grid export, and calculating 
charging strategies.
Some of these are used in automations and some is there to use manually or for further automation improvements. 
--------------------------------------------------------------------------------------

--------------------------------------------------------------------------------------
# Requirements to use below scripts / automations #
Huawai Solar integration by wlcrs: https://github.com/wlcrs/huawei_solar
Solcast integration by oziee: https://github.com/BJReplay/ha-solcast-solar 
Energi Data Service integration by MTrab: https://github.com/MTrab/energidataservice 
Huawei Solar PEES package by JensenNick https://github.com/JensenNick/huawei_solar_pees (not required but nice addon to Huawei)
--------------------------------------------------------------------------------------

--------------------------------------------------------------------------------------
# A few input sensors may need to be adjusted:
sensor.solcast_pv_forecast_forecast_today (from Solcast integration)
sensor.energi_data_service_total_price (from Energi Data Service integration)
sensor.energi_data_service_spot_price (from Energi Data Service integration)
sensor.batteries_state_of_capacity (from Huawei Solar Integration)
sensor.inverter_active_power_control (from Huawei Solar Integration)
--------------------------------------------------------------------------------------

--------------------------------------------------------------------------------------
# Roadmap features:
1. Adjustable AMP configuration for EV Charging (Wallbox Pulsar Plus) based on excessive PV production after house load
2. Peak shaving / Capacity Control
--------------------------------------------------------------------------------------
