# Hardware validation plan

Record each result as **PASS**, **FAIL**, **NOT TESTED**, or **BLOCKED**, together with build fingerprint, conditions, and relevant logs.

## Pre-flash safety
- [ ] Exact model confirmed as SM-G9650 / star2qltechn
- [ ] Full personal-data backup completed
- [ ] Original stock firmware available
- [ ] Recovery route verified
- [ ] Candidate ZIP/image source and SHA-256 verified
- [ ] Battery sufficiently charged

## Boot and system
- [ ] First boot completes
- [ ] Reboot and cold boot
- [ ] No bootloop or repeated crash
- [ ] Settings and storage work
- [ ] Encryption and lock-screen behavior
- [ ] Recovery can read/install supported packages
- [ ] Sleep/wake and overnight idle
- [ ] Thermal behavior under ordinary use

## Connectivity
- [ ] SIM recognized
- [ ] Registration on supported network mode
- [ ] Incoming/outgoing calls
- [ ] SMS
- [ ] Mobile data
- [ ] VoLTE where supported by carrier and configuration
- [ ] Wi-Fi 2.4 GHz
- [ ] Wi-Fi 5 GHz
- [ ] Bluetooth enabled, scan, pair and reconnect
- [ ] Bluetooth audio / hands-free
- [ ] NFC, if applicable and supported by the tested configuration
- [ ] GPS / GNSS location fix

## Hardware
- [ ] Earpiece, loudspeaker and microphone
- [ ] Wired audio / USB audio where applicable
- [ ] Front and rear cameras
- [ ] Flashlight
- [ ] Display, brightness and automatic brightness
- [ ] Touchscreen and rotation
- [ ] Proximity and ambient-light sensors
- [ ] Accelerometer / gyroscope
- [ ] Vibration
- [ ] Fingerprint sensor
- [ ] USB data and charging
- [ ] Battery reporting and charging stability

## Evidence capture

For a reproducible failure, collect logs immediately after it occurs:

```sh
adb logcat -b all -d > logcat-all.txt
adb shell dmesg > dmesg.txt
adb shell getprop > getprop.txt
adb shell dumpsys -l > dumpsys-services.txt
```

Some Android builds restrict `dmesg`; record command errors rather than assuming the output is complete. Review logs for device identifiers and personal data before sharing publicly.

## Release gate

Do not label a build “stable” merely because it boots. Publish the exact tested build fingerprint, firmware/baseband, test conditions, passed items, failed items, and known limitations.