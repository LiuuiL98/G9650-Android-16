# Baseline capture checklist

The immediate priority is to identify the exact previously tested LineageOS 19.1 baseline for SM-G9650. Do not start by modifying Android 16 sources without a reproducible reference.

## Required identifiers

Fill these in from the phone and the exact ROM ZIP when available:

- ROM ZIP filename:
- ROM download/source URL:
- SHA-256:
- Android version / LineageOS build date:
- Build fingerprint (`adb shell getprop ro.build.fingerprint`):
- Device product / device / board properties:
- Kernel version (`adb shell uname -a`):
- Bootloader/baseband version:
- Current Samsung firmware build:
- Recovery version:
- Encryption state:
- Exact device model shown in Download Mode:

## Safe data collection

With the phone booted and ADB authorized, run:

```sh
adb devices
adb shell getprop ro.build.fingerprint
adb shell getprop ro.build.version.release
adb shell getprop ro.build.version.security_patch
adb shell getprop ro.product.device
adb shell getprop ro.product.vendor.device
adb shell getprop ro.bootloader
adb shell getprop gsm.version.baseband
adb shell uname -a
adb shell getprop | grep -E 'ro\.build\.fingerprint|ro\.product\.device|ro\.bootloader|baseband'
```

Save output locally and remove personally identifying information before sharing. Do not collect or publish IMEI, serial number, phone number, account data, or other secrets.

## Prior reported observations

The user reported a LineageOS 19.1 setup where SIM registration was unreliable in the default mode, while LTE-only mode could register China Unicom; Bluetooth was also unavailable in one build. Exact build and logs are not yet pinned. Reproduce each issue on the exact same image before attributing it to all builds or to a particular source component.

## Backup and recovery gate

Before flashing experimental builds, confirm that a known recovery path exists and make backups of important personal data. Do not flash images from an unverified build. Keep original stock firmware and the exact known-working recovery available.