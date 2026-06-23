# Neatsvor

This platform can be used to control Neatsvor vacuums connected to Home Assistant using the [Neatsvor integration](https://github.com/CoderAS-ru/hass-neatsvor).

## Available templates

### Zone cleaning (`vacuum_clean_zone`)

Uses 4 coordinates to clean rectangular zones.

**Used service:** `neatsvor.vacuum_clean_zone`
  <details>
  <summary>Example configuration</summary>
    
  **Example configuration:**
  ```yaml
  map_modes:
    - template: vacuum_clean_zone
  ```

  </details>
  <details>
  <summary>Example video</summary>

    https://user-images.githubusercontent.com/6118709/141666913-d95f082d-f5bf-4ab5-a478-ba44effe6f34.mp4

  </details>


## Supported entities
For full functionality, your Neatsvor vacuum should provide the following entities in Home Assistant:

| **Entity type**	| **Example ID**	| **Description** |
|-----------------|-----------------|-----------------|
| `vacuum.*`	| vacuum.neatsvor_vacuum	| Main vacuum entity |
| `select.*` | select.neatsvor_fan_speed	| Fan speed control |
| `select.*` | select.neatsvor_water_level	| Water level control (for mopping models) |
| `select.*` | select.neatsvor_clean_mode	| Cleaning mode selection |
| `sensor.*` | sensor.neatsvor_main_brush	| Main brush wear (percent) |
| `sensor.*`	| sensor.neatsvor_side_brush	| Side brush wear (percent) | 
| `sensor.*`	| sensor.neatsvor_hepa_filter	| HEPA filter wear (percent) | 
| `sensor.*	` | sensor.neatsvor_total_cleanings	| Total cleaning count | 
| `sensor.*`	| sensor.neatsvor_cleaning_area	| Total cleaned area (m²) | 
| `sensor.*` | sensor.neatsvor_cleaning_time	| Total cleaning time (minutes) | 

## Additional configuration
The Neatsvor platform supports tiles for maintenance status, cleaning statistics, and menu icons for fan speed, water level, and cleaning mode. These will appear automatically if the corresponding sensors are available.

For more information about the integration, visit the [Neatsvor integration](https://github.com/CoderAS-ru/hass-neatsvor) repository.
