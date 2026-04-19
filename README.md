Would be nice if you support me: https://buymeacoffee.com/bmks — thank you very much!!
> Important: Use at your own risk. You, the device owner, are responsible for any damage, data loss, or bricked devices.

# Firmware "MC Term"
Finally there is a name, "MC Term", M(esh)C(ore) Term(inal). We like Retro, you like Retro, we stay retro :-).

# v0.9.12 released for TDECK & HeltecV4 (no online update!)
- bugfixes!
- save messages on SDCard if available!
- adaptions in the GUI (mail symbol now in the Msgs Button).
- DuryCycle can now be configured.
- GPS tracker based on distance with advert flooding
- new two customizeable QuickReply Buttons with dynamic variables support!
- integrated webserver with a powerful WebBrowser UI
- new device support -> Elecrow CrowPanel7
and more...

# MeshCore / additional GUIs for embedded mesh radios

MeshCore is a lightweight hybrid routing mesh protocol for packet radios... this Repo is mainly about the companion firmware for user-friendly UIs and (actually) support for devices such as the LilyGO T-Deck Plus and Seeed Studio SenseCap Indicator-TFT AND you can connect via BLE or WiFi with the typical APPs (WebUI, iOS,...).

## Table of contents
- [Features](#features)
- [Devices](#devices)
- [Installation](#installation)
- [Default radio settings](#default-radio-settings)
- [Why "this" GUI?](#why-this-gui)
- [Notes & reliability](#notes--reliability)
- [Contributing](#contributing)

## Features

The improved companion UI is a full-featured, touch-first interface designed
for small embedded mesh radios. Highlights include:

- Tabs: `Contacts`, `Channels` (groups + Users/DMs), `Map` (slippy tile map), and `Mgmt` (settings & admin).
- Status bar: device name, transport badges (BLE/WiFi), and battery or duty-cycle indicator; temporary left-side overrides for short messages.
- Touch-first interactions: large touch targets, configurable UI zoom, reliable touch scrolling and list handling.
- Hardware navigation: D-pad / joystick / T-Deck trackball support, with configurable debounce and keyboard navigation for non-touch targets.
- On-screen keyboard: overlay keyboard with upper/lower/symbol modes and D-pad focus support; editor buffer sized for channel sends (up to ~125 chars).
- Messaging: messenger-style channels and DM threads, send confirmations, unread counts, IRC-style transcript view, full message detail view, timestamps, and quick-send actions.
- Message metadata: display of path/hops, best-effort RSSI and SNR, overheard repeaters list, and route hints.
- Contacts: sortable/filterable contact list (Azimuth, LastHeard, LastMessage), badges for GPS/reachability/online, favorite/tel flags, and detailed contact view with public key.
- Map: slippy-tile map with zoom, pan/drag, auto-centering, jump-to-me, map selection of contacts, and tile fetching/caching behavior configurable per target. (look here for maps: https://github.com/mattdrum/map-tile-downloader)
- Management features: edit device name, WiFi SSID/password, BLE PIN, screen timeout, radio parameters (frequency, bandwidth, spreading factor, coding rate, TX power), GPS pins/baud, and other device settings.
- Diagnostics & logs: neighbor scan helper, Rx-raw packet log viewer with parsed metadata, parsed packet detail view, telemetry request/pending state and telemetry history, and ping records for reachability tests.
- Telemetry & sensors: manual telemetry requests, telemetry records (voltage, temperature, humidity, CO2, TVOC), and support helpers for CayenneLPP / LPP payloads.
- Alerts & popups: lightweight banner/popups, sticky popups, and power/battery info overlays.
- Persistence: user preferences saved (screen timeout, battery label mode, contacts filter, T-Deck nav debounce, touch tuning) using NVS/Preferences.
- Developer & advanced helpers: public key detail, raw RX log scrolling, parsed packet metadata for debugging, and small utilities (neighbor repeater scan, telemetry probe, ping helper).

## Devices

This firmware includes companion UIs and support for several devices; notable examples:

- LilyGO T-Deck / T-Deck Plus — more details: https://lilygo.cc/en-us/products/t-deck-plus-1
- Seeed Studio SenseCap Indicator (TFT / D1Pro) — more details: https://www.seeedstudio.com/
# SOON
- Heltec V4 with Display! (v0.9.11) (WIP)
- Elecrow with 3.5 Display! (v0.9.11) (WIP) -> https://www.elecrow.com/pub/wiki/CrowPanel_Advance_3.5-HMI_ESP32_AI_Display.html

## Installation

Flash using the MeshCore web flasher:

1. Open https://flasher.meshcore.co.uk/ and choose "Custom Firmware".
2. For first-time installation (use the *-merged version), perform a full flash erase (this removes all settings). *take care*, if you upgrade the version to a newer one, use the non-merged version!
3. Upload the built firmware image and flash to your device.

Notes:

- Full flash erase is only needed for first-time installs or major updates. Subsequent updates can be done without erasing settings.
- First boot after flashing may take 1–2 minutes. The screen can flicker while partitions and settings initialize.

## Default radio settings

These defaults are used when there are no saved preferences on the device (EU/UK):

- Frequency: 869.618 MHz
- Bandwidth: 62.5 kHz
- Spreading Factor: 8
- Coding Rate: 8
- TX Power: 22 dBm

Adjust and persist settings from the Management screen on-device.

## Why "this" GUI?

MeshCore focuses on a compact, reliable mesh stack while adding polished on-device GUIs for a better out-of-the-box experience on small embedded devices. This repository adds companion UIs and platform variants to support hardware (such as the T-Deck Plus and SenseCap Indicator).

## Notes & reliability

- Persistence: key settings are stored on-device and survive reboots and updates.
- Reliability: UI touch mapping and state handling were improved to remain accurate when using zoomed UIs.
- GPS: some device variants (e.g. T-Deck Plus) require specific UART baud rates (38400) or correct antenna orientation;

## Contributing

Contributions welcome. Please open issues for bug reports, improvements, enhancements, or device support (thats more tricky when i do not own one).

## Device pictures (samples)

* see "images" folder for more!

![pic_tdeckplus](/images/IMG_2694.jpeg)
![pic_sensecapindicator](/images/IMG_2717.jpeg)
