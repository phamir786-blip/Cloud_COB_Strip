# Cloud COB Strip

ESP32-C3 COB LED PWM controller copied from [amiraplle/PWM](https://github.com/amiraplle/PWM), with Arduino IoT Cloud support added while preserving the existing local web controller and firmware features.

## Firmware features retained

- ESP32-C3 hardware PWM with adjustable brightness, frequency, dimming curve, and fade softness
- Local web UI, Wi-Fi setup, mDNS, and settings saved in NVS
- Countdown timer and web-based OTA firmware updates
- GitHub Actions firmware and web-app builds, including merged and separate firmware binaries

## Arduino IoT Cloud setup

The Arduino Cloud Thing should contain these Read & Write variables with matching names and types:

- `powerOn` — Boolean
- `brightness` — Integer (1–100)

The current Arduino Cloud Device ID is supplied through the `ARDUINO_CLOUD_DEVICE_ID` Actions secret; no Device ID is hard-coded.

### Configure the device key

For a local build, copy `firmware/esp32c3_cob_pwm/arduino_cloud_secrets.h.example` to `firmware/esp32c3_cob_pwm/arduino_cloud_secrets.h` and set `DEVICE_KEY` to the Device Secret Key from Arduino Cloud. The real header is ignored by Git; never commit or share the key.

For GitHub Actions builds, add repository Actions secrets named `ARDUINO_CLOUD_DEVICE_ID` and `ARDUINO_CLOUD_DEVICE_KEY`. The workflow creates the ignored header at build time only when both are present. Without that secret, the firmware still builds, but Arduino IoT Cloud remains disabled.

The ESP32 must first be configured for its home Wi-Fi using the existing local web UI. Cloud startup is skipped while the controller is in setup-AP mode or when no device key is configured.

## Build

GitHub Actions builds the ESP32-C3 firmware and the web app. The firmware build publishes the merged factory image and separate flash binaries as artifacts. A successful compile does not by itself confirm cloud connectivity on physical hardware.
