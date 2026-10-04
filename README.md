# Airthings Bluetooth Proxy

ESPHome configuration for a Waveshare ESP32-C6-GEEK (ESP32-C6, 16 MB flash) used as an active Bluetooth proxy for Home Assistant and Airthings BLE sensors.

## LCD status screen

The built-in 1.14-inch ST7789P3 LCD is oriented for a vertically plugged-in USB connector and shows one compact page with temperature, humidity, CO2, radon, VOC, PV power, battery percentage, load power in kW, the age of the last value received from Home Assistant, and the currently allocated active BLE connections out of three slots.

The displayed Airthings values are subscribed from existing Home Assistant entities; this ESP does not independently connect to the Airthings sensor.

The board's official LCD pins and initialization parameters are used. The project also contains a local copy of ESPHome 2026.6.5's `bluetooth_proxy` component under `components/bluetooth_proxy/`. It adds one read-only method needed to show the exact number of allocated active BLE connections. Review or rebase this local component whenever ESPHome is upgraded.

The TF-card slot and all unrelated sensors remain unconfigured.

## Initial flash

1. Add Wi-Fi credentials and generated secrets to `secrets.yaml` (this file is not tracked by Git).
2. Connect the board directly to the Mac with a USB-C-to-USB-A data adapter.
3. Verify and compile with `esphome config airthings-proxy.yaml` and `esphome compile airthings-proxy.yaml`.
4. Flash only after the newly appeared serial port has been identified: `esphome run --device /dev/cu.<port> airthings-proxy.yaml`.

## Home Assistant

Once online, Home Assistant should discover the ESPHome device. Add it with the API encryption key in `secrets.yaml`; its Bluetooth proxy will then be automatically aggregated by the Bluetooth integration.
