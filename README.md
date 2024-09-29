# Huawei Solar Battery Optimization

Maximize the potential of your Huawei solar battery system with this powerful Home Assistant package. By intelligently optimizing battery usage, solar production, and grid interaction, this solution helps you:

- Significantly reduce your energy costs
- Maximize returns on surplus solar energy
- Minimize your carbon footprint
- Increase your energy independence

Perfect for homeowners looking to make the most of their solar investment while contributing to a greener future.
<br><br>

## Features

- Daily optimization of battery usage:
  - Calculates energy thresholds and pricing at 6:00 AM
  - Analyzes hourly solar production forecast and electricity prices
  - Generates a summary of recommendations for the day
- Comprehensive daily mode management:
  - Automatically switches between different working modes based on time, solar production, and house load
  - Supports Time-of-Use (TOU), Maximize Self-Consumption, and Fully Fed to Grid modes
  - Adjusts mode based on seasons (different strategies for winter/spring and summer months)
- Grid export management based on spot prices:
  - Enables or disables grid export based on current electricity spot prices
  - Avoids exporting during negative price periods
- Next-day charging strategy calculation:
  - Runs at 9:00 PM to prepare for the following day
  - Determines required grid charging and recommends working mode
- EV charging optimization:
  - Switches to a special TOU mode during EV charging to prioritize vehicle charging
  - Reverts to optimal mode after EV charging is complete
- Seasonal mode switching:
  - Automatically adjusts strategy based on the current month
- Smoothed sensor readings:
  - Uses Exponential Moving Average (EMA) for solar production and house load to handle fluctuations
- Configurable parameters:
  - Allows input configuration of all sensors used from integrations, such as Huawei device IDs, Max battery capacity
- Detailed logging and notifications:
  - Provides system logs for troubleshooting and monitoring
  - Sends notifications for major mode changes and actions<br><br>

## Requirements

To use this package, you need the following integrations:

- [Huawei Solar integration by wlcrs](https://github.com/wlcrs/huawei_solar)
- [Solcast integration by oziee](https://github.com/BJReplay/ha-solcast-solar)
- [Energi Data Service integration by MTrab](https://github.com/MTrab/energidataservice)

### Optional integrations 
- [Huawei Solar PEES package by JensenNick](https://github.com/JensenNick/huawei_solar_pees) (optional but recommended)
- [Smoothing Analytics Sensors by woopstar](https://github.com/woopstar/smoothing_analytics_sensors) (optional for now)<br><br>


## Installation

1. Place the `huawei_solar_battery_optimization.yaml` file in your Home Assistant packages directory (usually `/config/packages/`).
2. Restart Home Assistant to load the new package.

For more information on using packages in Home Assistant, see the [official documentation](https://www.home-assistant.io/docs/configuration/packages/).<br><br>


## Configuration

You may need to adjust the following input sensors in the package file to match your setup:

- `sensor.solcast_pv_forecast_forecast_today` (from Solcast integration)
- `sensor.solcast_pv_forecast_forecast_tomorrow` (from Solcast integration)
- `sensor.energi_data_service_total_price` (from Energi Data Service integration)
- `sensor.energi_data_service_spot_price` (from Energi Data Service integration)
- `sensor.batteries_state_of_capacity` (from Huawei Solar Integration)
- `sensor.inverter_active_power_control` (from Huawei Solar Integration) <br><br>


## Automations

The package includes the following automations to manage and optimize your Huawei solar battery system:

1. `hsbo_daily_battery_optimization`: 
   - Runs daily at 6:00 AM
   - Performs the battery optimization analysis for the upcoming day
   - Calculates energy thresholds/pricing, analyzes hourly forecast data, and generates a summary of recommendations

2. `hsbo_set_battery_charge_time`:
   - Runs daily at 6:05 AM
   - Sets the optimal time to charge the battery based on the daily optimization results

3. `hsbo_calculate_next_day_charging_at_9_pm`:
   - Runs daily at 9:00 PM
   - Calculates the charging strategy for the next day
   - Determines required grid charging and recommends working mode for the following day

4. `hsbo_comprehensive_daily_mode_management`:
   - Runs every 5 minutes and at specific trigger points
   - Manages the mode of the system throughout the day based on various factors:
     - EV charging status
     - Time of day
     - Solar production vs. house load
     - Spot prices
     - Seasonal considerations
   - Handles working mode switches for:
     - EV charging optimization
     - Transitioning between TOU, Maximize Self-Consumption, and Fully Fed to Grid modes
     - Adapting to changes in solar production and house load
     - Managing grid export based on spot prices
   - Provides detailed logging of mode changes and reasons

These automations work together to provide a comprehensive, adaptive system that optimizes your solar battery usage, maximizes self-consumption, and manages grid interaction based on current conditions and forecasts.<br><br>


## Scripts

The package includes the following scripts for managing and optimizing your Huawei solar battery system:

1. `hsbo_calculate_energy_thresholds`: Calculates the average, high, and low energy price thresholds for the day.

2. `hsbo_analyze_hourly_data`: Analyzes the hourly solar production forecast and electricity prices, making decisions about battery charging and grid selling.

3. `hsbo_process_hourly_data`: Processes data for a specific hour, determining whether to sell to the grid, charge the battery, or use the energy.

4. `hsbo_update_hourly_recommendation`: Updates the recommendation for a specific hour based on the decision made in the hourly data processing.

5. `hsbo_generate_summary`: Generates a summary of the battery optimization analysis, including potential grid selling, battery charging, and recommendations.

6. `hsbo_set_working_mode_to_default_tou_periods`: Sets the system to use default Time-of-Use (TOU) periods, configuring charging and discharging times.

7. `hsbo_set_working_mode_to_fully_fed_to_grid`: Sets the system to feed all generated power to the grid.

8. `hsbo_set_working_mode_to_maximise_self_consumption`: Sets the system to maximize self-consumption of generated power.

9. `hsbo_set_working_mode_to_tou_and_disable_battery_charging_and_discharging`: Sets a special TOU mode where battery charging and discharging are disabled.

10. `hsbo_disable_grid_export`: Disables exporting power to the grid.

11. `hsbo_enable_grid_export`: Enables exporting power to the grid.

12. `hsbo_calculate_next_day_charging`: Calculates the charging strategy for the next day, including required grid charging and recommended working mode.

These scripts are used in various automations within the package and can also be called manually or incorporated into your own custom automations as needed.<br><br>


## Lovelace card

You can use the following lovelace card to show all entities related to HSBO using the [custom auto entities card](https://github.com/thomasloven/lovelace-auto-entities):

```yaml
- type: custom:auto-entities
  card:
    type: entities
    title: HSBO
  filter:
    include:
      - entity_id: '*hsbo*'
    exclude: []
```
<br><br>

## How It Works

### Daily Processes

- **6:00 AM**: Daily optimization runs, calculating energy thresholds, analyzing hourly data, and generating a summary.
- **6:05 AM**: The system implements the daily optimization, setting the working mode to Time-of-Use with disabled battery, and potentially scheduling a mode change.
- **Every 5 minutes**: The system checks if it's time to change the mode to Maximize Self-Consumption.
- **5:00 PM**: The system switches to an appropriate mode based on the current month.
- **9:00 PM**: The system calculates the charging strategy for the next day.

### Continuous Processes

- The system manages inverter export based on spot prices, triggered by price changes or hourly checks.<br><br>


## Working Modes

This package supports several working modes to optimize your Huawei solar battery system:

### 1. Maximize Self Consumption

**Description**: This mode prioritizes using the energy generated by your solar panels to power your home and charge your battery, minimizing the amount of energy drawn from the grid.

**Use Case**: Ideal for households looking to reduce their reliance on grid electricity and maximize the use of their solar energy production.

**When to Use**: 
- During periods of high solar production
- When grid electricity prices are consistently high

### 2. Time of Use (Battery Charge / Discharge)

**Description**: This mode optimizes battery charging and discharging based on time-of-use electricity rates. It charges the battery when electricity rates are low and discharges when rates are high.

**Use Case**: Perfect for users with time-of-use electricity tariffs who want to minimize their electricity costs.

**When to Use**:
- In areas with significant differences between peak and off-peak electricity rates
- To reduce strain on the grid during peak hours
- To take advantage of lower nighttime electricity rates for charging

**Default TOU Periods** (configured in this package)
- Off-peak (charging): 00:01 - 05:59 (all days)
- Peak (discharging): 06:00 - 10:00 (all days)
- Peak (discharging): 17:00 - 23:59 (all days)

These default periods are set to optimize battery usage based on typical daily electricity demand patterns. The system charges the battery during early morning hours when demand is typically low, and discharges during morning and evening peak hours. You can adjust these periods to match your specific utility's rate schedule or your household's energy consumption patterns.

### 3. Fully Fed to Grid

**Description**: In this mode, all excess solar energy is fed back to the grid instead of being stored in the battery.

**Use Case**: Useful in scenarios where feeding energy to the grid is more beneficial than storing it.

**When to Use**:
- When grid feed-in tariffs are particularly high
- During periods of consistently high electricity spot prices
- If there's a need to reduce battery cycling to extend its lifespan

### Grid Export Management

**Description**: This feature allows you to enable or disable the export of excess energy to the grid based on current electricity spot prices.

**Use Case**: Helps optimize the financial benefits of your solar system by exporting energy when it's most profitable.

**How it Works**:
- When enabled, the system will export excess energy to the grid if the current spot price is above a set threshold.
- When disabled, excess energy will be used to charge the battery or will be curtailed if the battery is full.

**When to Use**:
- Enable during periods of high electricity spot prices to maximize revenue from energy export
- Disable when spot prices are low, and it's more beneficial to store energy in the battery for later use

By utilizing these working modes and the grid export management feature, you can tailor your Huawei solar battery system's operation to your specific needs, local energy market conditions, and personal energy goals.<br><br>


## EV Charging

This package now includes features to optimize battery usage when an electric vehicle (EV) is charging. Here's how it works:

### EV Charging Detection

The system monitors the state of EV charging using a binary sensor (`binary_sensor.hsbo_ev_charging`). When EV charging starts or stops, it triggers specific actions to optimize energy usage.

### Behavior During EV Charging

When EV charging starts:
1. The system switches to a special Time-of-Use (TOU) mode where battery charging and discharging are disabled.
2. This ensures that all available power (from solar and grid) is directed to charging the EV, without drawing from or storing energy in the home battery.
3. The current priority mode is set to "ev_charging" to indicate this special state.

When EV charging stops:
1. The system reverts to the recommended working mode (either standard TOU or Maximize Self Consumption).
2. This allows the home battery to resume normal operation based on the current energy situation and time of day.

### Benefits

- Prioritizes EV charging when needed, ensuring the fastest possible charge for your vehicle.
- Prevents unnecessary cycling of the home battery during EV charging, which could reduce overall system efficiency.
- Automatically returns to optimal home energy management when EV charging is complete.

### Configuration

To use this feature, ensure you have a binary sensor set up in Home Assistant that indicates when your EV is charging. Update the `input_text.hsbo_ev_charger_status` in the package configuration to point to your EV charging sensor.

This EV charging optimization works seamlessly with the other features of the Huawei Solar Battery Optimization package, providing a comprehensive energy management solution for homes with both solar systems and electric vehicles.

Please note, if you already has existing automations configuring the Huawai working mode, these should be disabled or removed as the Daily mode management automation would conflict or reset any other configurations.<br><br>


## What's New

This section highlights key updates and new features added to the Huawei Solar Battery Optimization package.

### September 28, 2024
- Added EV charging optimization feature
  - Updated Daily Mode Management to include EV charging mode on and off
  - Added a new section in the README for EV charging specifics
- Introduced smoothed sensors with EMA (Exponential Moving Average) to handle fluctuations for calculating morning routing when solar production is higher than house load

### September 27, 2024
- Updated README to include a comprehensive description of working modes
- Configured two new helpers: Battery Max Capacity and Morning Energy Need (for between 6am to 9am)
- Optimized the script for calculating next day charging strategy

### September 26, 2024
- Added new automation: hsbo_comprehensive_daily_mode_management
  - This new automation incorporates and replaces several previous automations, streamlining the daily mode management process

### September 25, 2024
- Implemented changes to improve cutoff percentage calculations
- Fixed various bugs and made small improvements to enhance overall performance

### September 24, 2024
- Added all external sensors into input_texts for easier configuration
- Added a Lovelace card to the README for easy visualization of HSBO-related entities
- Corrected datetime errors in daily changes automation

### September 23, 2024
- Initial commit of the Huawei Solar Battery Optimization package
- Added detailed README with installation instructions and configuration guidance
- Included the Home Assistant YAML package file for easy integration<br><br>


## Roadmap

Future planned features include:

1. Dynamic, time-specific, and seasonally-adaptive 14-day average consumption power sensors to make more accurate and personalized energy predictions and charging strategies.
    - Improved Accuracy: By using historical data specific to your home, the system can make more accurate predictions about your energy needs.
    - Adaptability: The system will automatically adapt to changes in your usage patterns over time, as the 14-day average updates.
    - Time-of-Day Specificity: Instead of using a single estimate for morning usage, we now have hour-by-hour estimates, which can lead to more precise charging strategies.
    - Seasonal Adaptation: As the 14-day average naturally shifts with seasons, your system will automatically adjust without needing manual intervention.
2. Adjustable AMP configuration for EV Charging based on excessive PV production after house load.
3. Peak shaving / Capacity Control.
4. Integration with smart home devices for enhanced energy management<br><br>


## Contributing

This package is created by Heino Skov. While no official support is provided, you're free to use and edit it to your needs. Contributions, suggestions, and improvements are welcome via GitHub issues and pull requests.

### Thanks to Contributors

A big thank you to all the contributors who have helped improve this project:

- [woopstar](https://github.com/woopstar): For numerous pull requests, bug fixes, and feature implementations.

Your contributions, whether through code, documentation, or suggestions, have been invaluable in making this project better for everyone. Thank you for your time and effort!<br><br>


## License

This project is open-source and available under the [MIT License](LICENSE).<br><br>


## Disclaimer

This package is provided as-is, without any warranty or guarantee of its functionality or suitability for any particular purpose. Use at your own risk.

