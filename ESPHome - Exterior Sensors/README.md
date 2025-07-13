# Exterior Sensors

In 2025, I became very invested in making the lawn in my yard as nice, lush, and green as possible. While the results once the pacific northwest heat arrived were mixed (still better than it was in 2024), I wanted to have as much data as I could to improve my chances.

## The sensors:
As it seems moisture, tempurature, and sun exposure were the most important, thats where I started.

- Moisture: Analog sensor that reads the conductivity of the soil*
- Tempurature: One-wire sensor, added as a dallas sensor
- Lux Sensor: To review and estimate sun exposure**

*This sensor is based on conductivity. Not actual moisture content. While its not perfect, it gives me enough data to show if the grass needs to be watered.

**Sticking a lux sensor in a box with a shaded lid (or any lid) requires calibration. I took 5 different measurements with different light sources (sun, phone light, desk lamps, etc) with the lid open and then closed. Averaged them. Then came up with a delta between the lid open and closed. Its dumb. But again, good enough.

## MQTT reporting:

This was something I didnt want to do as it would require me to start using MQTT. I wanted just the ESPHome API as thats what all the other devices I built use. I spent more time than I care to admit trying to just use the API.

I was wrong. MQTT was the **correct** choice. Here's why:

1. The API is slow. When using deep sleep, it doesn't connect fast enough.
2. Disabling Deep Sleep would have been obnoxiously difficult.

Here's how the config works:

1. The ESPHome API is removed to reduce boot time
2. WIFI is direct to a specific SSID. Speeds up connection.
3. On connection, MQTT looks for a topic message.
    - Is OTA "ON"? Disable deep sleep
    - Is OTA "OFF"? Go back to sleep after the set time.
4. Rinse. Repeat.

## Battery:
 This is easy. Since I'm using a LOLIN32, I just bought batteries from Aliexpess that were compatible.

 FOR THE LOVE OF GOD DOUBLE CHECK THE BATTERY POLARITY. Don't ask.

 ## Conclusion:
 Past the above notes, its just an ESPHome project. Nothing special. I'll likely move this to an above ground garden to regulate solinoids for water. But thats for another project.