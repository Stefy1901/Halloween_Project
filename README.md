# HalloweenBox 🎃

Motion-triggered Halloween sound box using an ESP32, HC-SR501 PIR sensor, Android phone, MIT App Inventor app, and Bluetooth speaker.

## How it works

1. The HC-SR501 detects motion and signals ESP32 GPIO 27.
2. The ESP32 sends `MOTION\n` over Bluetooth Classic to the Android app.
3. The app randomly selects one of six MP3 files.
4. The phone plays the sound through its selected audio output, such as a paired Bluetooth speaker.

```text
HC-SR501 --GPIO 27--> ESP32 --Bluetooth Classic--> Android app --> Bluetooth speaker
```

## Hardware

- ESP32 board supporting Bluetooth Classic SPP (for example, a suitable original ESP32 variant)
- HC-SR501 PIR motion sensor
- Android phone
- Bluetooth speaker
- USB power supply and wires
- Enclosure suitable for the intended environment

The DFPlayer Mini is not used by this version because the phone plays the audio.

## Wiring

| HC-SR501 | ESP32 |
|---|---|
| VCC | 5V/VIN, according to module requirements |
| GND | GND |
| OUT | GPIO 27 |

Power off before changing wiring. Ensure the ESP32 GPIO input is not exposed to a voltage above its rating.

## Firmware

Open `firmware/HalloweenBox.ino` in Arduino IDE, select your ESP32 board and port, then upload. Open Serial Monitor at 115200 baud. The Bluetooth device name is `HalloweenBox`.

`BluetoothSerial` requires an ESP32 target that supports Bluetooth Classic; not every ESP32 family supports it.

## MIT App Inventor app

The app setup is documented in [`docs/APP_INVENTOR_SETUP.md`](docs/APP_INVENTOR_SETUP.md). This repository does not include the original `.aia` project or the six MP3 assets, so the app blocks must be recreated and the sounds uploaded in MIT App Inventor.

Expected example filenames: `001.mp3` through `006.mp3`. Change the block text if your uploaded filenames differ.

## PIR tuning

- The HC-SR501's sensitivity potentiometer adjusts detection range. On common boards, a small counterclockwise adjustment reduces range, but check the markings on your module and test gradually.
- The time-delay potentiometer changes how long the output remains HIGH; it does not directly reduce sensitivity.
- Allow the PIR to stabilize after power-up (often around 30–60 seconds; check the module documentation).
- Avoid pointing it at moving trees, roads, heating vents, or other sources of unwanted movement.

The firmware cooldown reduces repeated triggers, but does not change physical detection range. Adjust `TRIGGER_COOLDOWN_MS` in the sketch to change it.

## Troubleshooting

- **No sound:** test the app's sound button, verify the phone audio is routed to the speaker, and check MP3 filenames.
- **Label displays MOTION but sound doesn't play:** ensure the app compares exactly with `MOTION`, sets `Player1.Source`, then calls `Player1.Start`.
- **No motion message:** check wiring, Bluetooth connection, and Serial Monitor.
- **Too many triggers:** reduce PIR sensitivity, reposition the sensor, and/or increase the firmware cooldown.

## Safety

Protect the electronics from rain and condensation, secure cables, and test the setup in its intended position. Check Android battery/background restrictions if the app must remain active for long periods.

## Repository layout

```text
HalloweenBox/
├── README.md
├── LICENSE
├── .gitignore
├── firmware/HalloweenBox.ino
└── docs/
    ├── APP_INVENTOR_SETUP.md
    └── HARDWARE_AND_TUNING.md
```

## License

MIT — see `LICENSE`.
