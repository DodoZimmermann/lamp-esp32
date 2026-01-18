# ESP32 RGB Lamp

A Bluetooth-controlled RGB LED lamp using an ESP32 microcontroller and the RemoteXY app.

## Features

- **Bluetooth Low Energy (BLE) control** via the RemoteXY mobile app
- **RGB color picker** for full color selection
- **Persistent settings** - lamp remembers the last color after power cycling

## Hardware Requirements

- Corresponding lamp HW
- Appropriate power supply for LEDs
- ESP32 development board

### Pin Configuration

| GPIO | Channel | Color |
|------|---------|-------|
| 18   | 0       | Red   |
| 39   | 1       | Green |
| 40   | 2       | Blue  |
| 21   | 3       | White (unused) |

## Software Requirements

### Arduino IDE Setup

1. Install [Arduino IDE](https://www.arduino.cc/en/software)
2. Add ESP32 board support:
   - Go to **File → Preferences**
   - Add to "Additional Board Manager URLs":
     ```
     https://raw.githubusercontent.com/espressif/arduino-esp32/gh-pages/package_esp32_index.json
     ```
   - Go to **Tools → Board → Board Manager**
   - Search for "esp32s3 Dev Module" and install

3. Install the **RemoteXY** library:
   - Go to **Sketch → Include Library → Manage Libraries**
   - Search for "RemoteXY" and install

### Board Settings

- **Board**: ESP32 Dev Module (or your specific board)
- **Partition Scheme**: Default (with enough space for BLE)

## Mobile App Setup

1. Download the **RemoteXY** app:
   - [iOS App Store](https://apps.apple.com/app/remotexy/id1168130280)
   - [Google Play Store](https://play.google.com/store/apps/details?id=com.remotexy)

2. Open the app and connect via Bluetooth
3. Look for device named **"RemoteXY"**
4. Use the RGB color picker to control the lamp

## How It Works

### Color Control
The app sends RGB values (0-255) over BLE. The ESP32 receives these values and outputs corresponding PWM signals to drive the LEDs. Values are scaled by 50% to limit maximum brightness.

### Persistent Storage
Color settings are automatically saved to flash memory using the ESP32's Preferences library:
- Changes are detected in real-time
- A 2-second delay prevents excessive flash writes while adjusting
- On power-up, the lamp restores the last saved color

## Customization

### Change Bluetooth Name
Edit the `REMOTEXY_BLUETOOTH_NAME` define:
```cpp
#define REMOTEXY_BLUETOOTH_NAME "MyLamp"
```

### Adjust Brightness Scaling
Modify the division factor in the loop (currently `/2` = 50%):
```cpp
rgbw_values[0] = RemoteXY.rgb_R / 2;  // Change to /1 for full brightness
```

### Change Default Color
Edit the default values in `preferences.getUChar()`:
```cpp
RemoteXY.rgb_R = preferences.getUChar("R", 128);  // Default red value
RemoteXY.rgb_G = preferences.getUChar("G", 128);  // Default green value
RemoteXY.rgb_B = preferences.getUChar("B", 128);  // Default blue value
```

### Adjust Save Delay
Change `SAVE_DELAY_MS` to adjust how long after the last change before saving:
```cpp
const unsigned long SAVE_DELAY_MS = 2000;  // 2 seconds
```

## License

This project is open source. Feel free to modify and distribute.
