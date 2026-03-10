# Changelog

All notable changes to this project.

## [v0.9.11] (unreleased - WIP)

### Upstream base upgraded: MeshCore v1.13 → v1.14

### Added
- Radio activity LED: a small round dot in the top status bar (left of the device name) that flashes green for 250 ms whenever any LoRa signal is received — valid packets and CRC-error receptions alike. The dot shows an idle ring outline when quiet and fills solid green on any radio activity.
- Map last-known-position auto-save: when GPS gets a valid fix, coordinates are persisted to flash immediately so the map restores the true latest position after reboot. Ongoing writes are still throttled (max once every 5 minutes) and only occur when coordinates change.
- Map contact markers now show the node 1-byte ID prefix (2-digit hex, e.g. `1E`) instead of plain squares, using the same existing marker background coloring (hop color / selected highlight) with automatic black-or-white foreground contrast for readability. (still under construction)

### Changed / Improved
- MAP marker tap behavior: tapping a ByteID marker now keeps selection on the map and shows a left-bottom info box with `Name`, `Age`, and `Distance` (km).
- MAP marker double-tap behavior: double-tapping the same ByteID marker opens that contact directly in Contact Detail.
- MAP selected-marker label text now stays at a constant size while zooming, instead of scaling with map zoom. (still under construction)
- Mgmt → Advert → Scan Neighbor Repeaters now shows an Add button only for repeaters not yet in Contacts, and tapping a repeater row opens Contact Detail instead of adding on name/row tap.
- Mgmt → Advert now includes `Path Hash Mode`, and Mgmt → Contacts now includes `AutoAdd Max Hops`, matching the MeshCore v1.14 companion settings on-device. (be careful other repeaters/companions need minumum MeshCore v1.14+ to avoid compatibility issues when these settings are changed)
- Mgmt → Contacts → `AutoAdd Max Hops` now cycles the full MeshCore v1.14 range `0..64` with the correct semantics: `0 = No Limit`, `1 = Direct (0 hops)`, and `N = up to N-1 hops`. (still under construction)
- DM message detail now shows delivery state for sent messages (Pending / Delivered / Not delivered), including ACK-based delivered updates.
- Mgmt → Channels now treats the built-in `Public` channel like any other joined channel, including Share/Delete actions.
- If the default `Public` channel was deleted, Mgmt → Channels → Join now shows a `Public` button that recreates it automatically only when it is currently missing.
- T-Deck text editor now scrolls with the cursor when moving left into long input, so the beginning of the text remains visible while editing.
- Text edit overlay uses smaller message-input text with wrapped visible lines for easier long-message editing.

### Fixed
- Companion-sent messages no longer appear twice.
- GUI-origin channel messages now populate message-detail metadata consistently (route/path hint + timestamp/RSSI baseline), so detail fields are no longer empty compared to companion-origin sends.
- DM detail delivery status now correctly updates for zero-hop/direct sends (no false "Not delivered" when ACK was received), and sent-message hops/RSSI/repeats metadata updates more reliably.
- T-Deck display wake behavior is now consistent: whether the screen turned off automatically or manually, it wakes via knob press only and no longer wakes on knob movement.
- Online AutoUpgrader manifest validation is now more robust (schema compatibility + target-key fallback), reducing false "manifest" errors.
- T-Deck keyboard backlight timeout now reliably turns the keyboard light off even after manual Alt+B keyboard-light activation.
- MAP now starts downloading and caching online tiles even when the SD card tiles folder is initially empty, as long as network tiles are enabled, Wi-Fi is connected, and the SD card is writable.
- Mgmt/Advert: Auto Advert Direct interval setting is now persisted correctly across reboots; values 4 h and 72 h were silently reset to Disabled on load due to a mismatch between the UI option list and the prefs sanitizer.
- Contact/DM/channel sender name colours are now clearly readable on dark backgrounds and far more distinct: the fixed 12-entry palette has been replaced with a full HSV colour-wheel generator (360 distinct hues at full brightness), so up to ~250 contacts each receive a unique, high-contrast colour.
- DM "Del(ete)" (Back long-press in DM view) no longer silently fails for contacts with names ending in "?" due to a parsing error in the delete confirmation dialog.
- AutoLock lock screen clock now counts automatically while the screen is on: the HH:MM:SS display updates once per second when idle (no touch). Clock pauses automatically when the screen turns off (display timeout or hardware button) and resumes on next wake. Display timeout continues to work normally while locked.
- Adding a repeater from Mgmt → Advert → Scan Neighbor Repeaters (and from Contacts → Discovered) now preserves the original seen time instead of resetting `Last heard` to `now`.
- Mgmt → Advert → Scan Neighbor Repeaters and the Repeater Admin `Neighbours` view now show only zero-hop/direct repeaters, excluding flooded ones.
- Msgs list scrolling is now visually consistent with Contacts: partially visible rows at the top edge of the Channels and DMs lists are rendered instead of popping in only once fully visible.
- Room-server `SVR` badges now use a distinct purple informational style instead of looking like blue action buttons.
- Nickname colours are now applied consistently across Contacts, MAP labels/info, discovered/repeater neighbour lists, and contact detail headers so the same name keeps the same colour throughout the UI.
- Mgmt → Contacts → `Purge w/o favs` works again; the final row is now reachable and tappable.

---

## [v0.9.10]

### Changed / Improved
- Smoothed battery voltage display using averaging to reduce rapid value jumps in the status area.
- Battery/Duty-cycle toggle now reliably shows percentage mode (including while charging) instead of repeating volts.
- Mgmt/GPS now includes an AutoBaud toggle (enabled by default); Baud editing is blocked while AutoBaud is enabled.
- Mgmt/GPS now shows progress popups while switching AutoBaud and while restarting GPS, so long-running actions no longer look stuck.
- In timeout-only mode (autolock disabled), keyboard keys no longer wake the display; wake remains on recessed trackball press/click to reduce accidental pocket wake-ups.
- Internal clipboard is now mirrored to SD at `/clipboard.txt` (where SD is available), allowing out-of-band import/export and clipboard persistence across reboot.
- Text edit fields now support press-and-hold paste in addition to double-tap paste.
- Copy popups now specify what was copied (for example: channel secret, selection, word, PubKey) instead of only showing a generic "Copied" message.
- In chat views, press-and-hold now copies text directly from the touched message/detail line (broader "copy any text" behavior in transcripts/details).
- Press-and-hold copy is also available in Contacts rows and selected Mgmt text rows (RxRaw log + telemetry rows).
- Mgmt/Global rows now support press-and-hold copy as well (name/device ID/public key/constants/admin info/storage).
- Press-and-hold copy now also covers Mgmt/WiFi, Mgmt/BLE, and Mgmt/GPS rows.
- Manual display-off (single recessed trackball/button click) now wakes only on another recessed trackball/button click when autolock is disabled; with autolock enabled, existing wake behavior remains unchanged.
- On T-Deck navigation, trackball movement is now ignored while the display is off/soft-off or while the lockscreen is active.


### Fixed
- Transport switch confirm no longer freezes the UI/device when enabling BLE from transport-off mode; live BLE re-enable from OFF is now handled safely.
- Transport switching no longer freezes on BLE -> WiFi -> BLE round-trips; BLE is kept initialized during live transport toggles.
- Message rendering now respects embedded line breaks (`\n`) from incoming text (for example bot messages), showing each break on a new line.
- Room server session transcript no longer draws the topmost message through the message-window top boundary when scrolled.
- Channel messages now synchronize both ways between device GUI and companion app over WiFi (app->device and device->app).
- BLE PIN edit now accepts valid 6-digit values that start with zero (for example, 012345).
- Mgmt/GPS button row rendering and touch hitboxes are aligned again (including Defaults and Restart GPS rows).
- Mgmt/GPS AutoBaud now keeps scanning past noisy false-positive baud hits, and the displayed Baud value now reflects the detected runtime baud after AutoBaud evaluation.
- Mgmt/Advert touch mapping is corrected: Advert-Direct and Advert-Flood buttons now trigger the intended send mode consistently.
- DM detail "Del" button hit area is corrected; deletion now triggers when tapping inside the visible button.
- Screen timeout now turns the display fully off on touch devices (instead of only dimming to zero brightness).
- Mgmt/UI scrolling now clamps correctly at the bottom and no longer scrolls into empty space.

---

## [v0.9.9]

### Changed / Improved
- updated to mescore v1.13.0
- Initialize T-Deck I2S buzzer at boot (keeps it quiet when disabled) to avoid driver install from touch handlers and prevent UI lockups on LilyGo T-Deck.
- Accept login-OK responses from alternate sender identities when the response matches expected formats (fixes repeater login stuck cases).
- Adjusted conservative repeater request/timeout values and improved local CLI reply handling.
- Repeater + RoomServer pending waits: Flood min 30s, Direct min 15s (applies to login and other admin requests).
- Reduce idle CPU usage by yielding in the main loop (adaptive delay when screen is soft-off/no companion app connected) to improve power draw without changing functionality.
- Add separate keyboard backlight timeout setting in Mgmt/UI (T-Deck Plus), independent of screen timeout and autolock.
- Smooth fade when display/keyboard backlight turns off (soft-off / keyboard timeout) instead of snapping to black.
- Persist Map "info bar" (altitude bar) toggle (T key) across reboots.
- Moved "Custom QuickSend" configuration from Mgmt/UI to Mgmt/Messages.
- reduce power use in display off/soft-off state by increasing idle sleep and skipping non-essential touch/gesture processing until wake.

### Fixed
- Prevent UI freeze when toggling "All Sounds" after reboot on LilyGo T-Deck builds by ensuring I2S/buzzer hardware is initialized safely at boot.
- Fix repeater login hanging when the OK response arrives from a different sender identity.
- Harden Mgmt/Global "Reboot" to always restart on ESP32 (adds esp_restart() fallback and avoids sticky UI states).

---

## [v0.9.8]

### Added
- Contacts / Room Server; added room login + Room Console (transcript + send + logout) under Contact Detail.
- UI; added horizontal drag in text input fields to move the cursor (caret).
- Contacts: added small `RSTPath` button in Contact Detail to reset a contact's route/path.
- Power: added `PowerStatus` struct and `MainBoard::getPowerStatus()` helper; UI now reads consolidated power state for battery/charging/usb.
- Power (T-Deck Plus): added configurable ADC multiplier / VBAT divider ratio support (`adc.multiplier`) to allow per-device calibration.
- Mgmt / Contacts; added "Purge w/o favs" (purge all contacts except favourites).
- Mgmt / GPS; show UBX accuracy estimate (hAcc) when available.
- MAP (T-Deck Plus); added keyboard controls: W/A/S/D for panning, O/I for zoom in/out, R to recenter on self, T to toggle GPS/Zoom info window.
- MAP; added zoom level indicator overlay that displays briefly when zoom changes (keyboard or touch).
- MAP; added GPS info window (top-left) showing altitude, speed (km/h), and current zoom level in a compact 3-line display (toggleable with T key on T-Deck Plus).
- UI; added autolock feature - can be configured in Mgmt / UI -> LOCK to automatically lock the UI after a period of inactivity. Unlock by holding the unlock button or touching and holding the screen for 2 seconds.
- Mgmt / UI; added LOCK section with configurable Autolock toggle and Autolock Timer (seconds).

### Changed / Improved
- Contacts; service contacts are no longer treated like normal DM targets:
	- Repeaters now route to Repeater Admin.
	- Room Servers now route to the Room Console.
- UI; horizontal swipes that switch higher-level frames now require an edge swipe (keeps left/right scrolling available for the focused element).
- UI (T-Deck Plus); trackball left/right no longer switches the bottom menu tabs.
- UI (T-Deck Plus); when editing a text field, trackball left/right moves the text cursor (caret).
- MAP; scroll/pan inputs now operate on the map view itself (instead of page-level scrolling).
- Mgmt / UI Zoom can now be changed with "^" or "v" buttons.
- Mgmt / Contacts; Auto Add Types can now be set with freely combinable toggles (USR/RPT/SRV/SNS/OW) when Auto Add is disabled.
- Mgmt / Contacts; Auto Add Types now shows clearer labels (e.g., "USR (Users)").
- Contacts / Repeater Admin; reorganized Login screen layout to prevent status text overlap and improve readability.
- Contacts / Repeater Admin; added live login status lines (Direct/Flood send mode, wait countdown, result, role).
- GNSS; u-blox M10 nav tuning (portable dynModel + auto fixMode) and 1Hz rate for weak-signal stability.
- Mgmt / UI -> UI Zoom; improved zoom step granularity for finer control with extra buttons for more/less zoom.
- MAP; zoom level now automatically persists when changed (via keyboard or touch), eliminating the need for manual default zoom configuration.
- MAP; removed "Def. Zoom Lvl" setting from Mgmt / UI as zoom now auto-saves and restores on startup.
- MAP; moved zoom level indicator to top-left position (stacks under GPS info window when active).
- MAP; GPS accuracy circle now renders as an unfilled light-blue ring instead of a solid fill for better map visibility.
- UI; autolock is now disabled by default and only engages when enabled in Mgmt / UI -> LOCK.

### Fixed
- Contacts / Room Console; fixed transcript drawing over the "Room Console" title (partial refresh artifacts) and added a bordered transcript viewport.
- MAP (T-Deck Plus); fixed reversed zoom hotkeys: I now zooms in and O zooms out.
- DM editor; fixed being able to send DMs to Repeaters/Room Servers via existing DM threads (now blocked and redirected to the proper flow).
- Mgmt / Log: touch scroll release no longer triggers a tap on the last touched point (prevents click-through when stopping a scroll).
- Mgmt / Log: require a tap gesture before activating list actions to avoid scroll-release click-through.
- Boards (T-Deck Plus): repaired corrupted header and fixed VBAT conversion to use the new configurable multiplier; prevents miscalibrated battery percentage readings.
- Contacts / Repeater Admin: clear session and cached repeater data when leaving admin or switching repeaters (prevents stale values).
- Contacts / Repeater Admin: direct routes now send direct logins/requests; flood is used only when no path is known.
- UI: hide battery percentage while charging; avoid duplicated charging indicator.
- Mgmt / Channels; fixed an issue where adding a new #hashtag channel could show "Channel exists" and could lead to duplicate message display.
- Mgmt / UI; fixed Tiles Folder picker showing empty after reboot until Map was opened once.
- MAP; fixed an issue while moving the map out of touch and after returning, where the map would jump back to the original position.
- Contacts/Repeater Admin; fixed Login button hit-test offset in Contact Detail overlay.
- Contacts/Repeater Admin; fixed repeater password NVS persistence detection (saved state) and empty-password save/load handling.
- MSGS / Message details view; fixed the overscrolling of the top button bar.
  
---

## [v0.9.7]

### Added
- Sync settings to SDCard and load from SDCard on boot (if present) for easy backup/restore of settings.
- MAP; GPS Accuracy Circle Implementation - when GPS accuracy is available, a light blue circle is drawn around the GPS position indicating the accuracy radius (HDOP-based accuracy estimation (HDOP × 2.0 meters)).

### Changed / Improved
- Contacts / Repeater Admin Menu; Modified the contact selection logic to only reset the repeater admin state when actually switching to a different repeater.
- MAP; improved GPS accuracy circle rendering (smoother edges).
- Contacts / Repeater Admin Menu; improved to be allowed to login with empty password.
- Discovered (Contacts) - Discovered Contacts list is now only sorted by "last heard" (newest on top) to make it easier to find recently discovered nodes for adding them as Contacts.

### Fixed
- Mgmt / UI - MAP; "Tiles Folder", fixed cosmetic issues fixed.
- Mgmt / Telemetry; when opening the Graphs overlay, now you can close it with a "back" button!
- Mgmt / GPS; fixed an issue where GPS could not be enabled/disabled properly.
- Mgmt / GPS; cosmetic issues fixed.
- MAP; Fix map scrolling bug: restrict map dragging to content area only
- I2C initialisation was broken - fixed!
- Indicator/RP2040; optimized the communication between RP2040 coprocessor and ESP32
- Indicator; fixed an issue where the bootsound was not working.
- Add SD persistence for UI preferences; Implement optional SD mirroring of UI prefs to /MCTerm/prefs.txt; NVS remains source of truth, SD provides backup across device erases; Add sync versioning to prevent stale data issues; Support both direct SD (T-Deck) and remote SD (SenseCap via RP2040); Live SD presence detection with automatic sync every 10 seconds; Include prefs: UI timeout, navigation, battery, contacts, NTP, sounds, map settings; Keyboard blink prefs only for T-Deck (conditional compilation)
- Cyrillic letters updated/fixed.

---

## [v0.9.6] prerelease for testers

 * Updated MeshCore Base to latest v1.12.0!

### Added
- Mgmt / UI -> Map; Added Map Show Zoom Buttons toggle (On/Off).
- Mgmt / UI -> Map; Added Map Show Navigation Buttons toggle (On/Off).
- Mgmt / UI -> Map; Added Map Default Follow Me toggle (On/Off).
- Mgmt / UI -> Map; Added Map Default Zoom Level setting (1-20).
- long Tap on Contacts Tab button; toggles between normal Contacts view and Discovered Nodes view to be able to add nodes when you do not activate in Mgmt / Channels the Auto Add Contacts option.
- Repeater Admin Menu; Implemented role-based gating (Guest/Admin) across Repeater Admin, Overview now visually disables admin-only buttons (ACL, CLI, Reboot, Passwords) when role isn’t Admin, and taps show a short popup instead of entering the screen. Device/Status/Telemetry overlays allow read-only viewing for Guest/RO, but write actions (edits/toggles/off/sync/reboot) are disabled visually and blocked on tap with a “RW/Admin required” popup.
- Indicator; (NEW IMAGE for RP2040 needed) Sound is now Supported & can be configured in Mgmt / UI -> Sound settings and will be processesd on the RP2040 coprocessor.
- Indicator; (NEW IMAGE for RP2040 needed) Added SenseCAP RP2040 SDCARD Support for remote SD access (read/write/list) does not yet offer direct services but will be used in future updates.
- Indicator; (NEW IMAGE for RP2040 needed) Added SenseCAP RP2040 Sensor Support for Temperature, Humidity, CO2, TVOC telemetry values (when RP2040 firmware is flashed with the new coprocessor image).
- MAP; tiles now can be donwloaded while you are connected to WiFi (when enabled in Mgmt / UI -> Map). If there is a SDCard present, it will automatically cache the tiles on the SDCard in the correct folder structure for offline use! (Indicator not yet supported!)
- Mgmt / UI -> Map; added info text about tile caching when WiFi is connected.
- Mgmt / UI -> Map; changed Map Tile folder structure from SD-Card.
- Mgmt / Messages; Addeed Auto Retry / Auto Reset Path / Direct Message Acks / Mark Delivered faster (like you know from the companion APP) but need to be setup separately because not connected to companion!
- MSGs / new button in Message "Reply" to quickly reply to the last DM or Channel message sender.

### Changed / Improved
- MAP; improved hop-count display in Contact detail view (shows "Direct" for 0 hops now and "1 hop", "2 hops", ... for others).
- Mgmt / Advert; neighbor adverts refactored to show more relevant information, and added the possibility to add discovered nodes as Contacts directly from the neighbor adverts list.
- Mgmt / Advert; changed button text "Advert Zero Hop" to "Advert - Direct"
- Mgmt / UI - moved Sound settings to Mgmt / UI for better grouping.
- Mgmt / Global - moved Mgmt / Admin into this menu for better grouping.
- Mgmt; changed some Texts&Buttons for better clarity (toggle, enabled/disabled)
- GUI; finally you can switch between BLE / WiFi or turn both completely off (without reboot!).
- Repeater Admin Menu / Advert; when disable both advert types, a warning popup (yes/no) is shown to inform the user that no adverts will be sent out anymore when accepted.
- MSGS; changed the top statusbar text from "msgs" to "Channels" and from "users" to "DMs" for better clarity.
- Contacts / soting; when you tap the contacts tab to cycle through sort orders, the view will automatically scroll to the top of the newly sorted list, making it much more user-friendly!
- Mgmt / CLI; fixed an issue while switching away from CLI breaks the touch.
- Mgmt / Stats; added the [>] buttons for the Radio section's Noise Floor, Last RSSI, and Last SNR lines in the Management > Stats view to see that this is clickable to open the Radio Stats overlay.

### Fixed
- Mgmt / CLI; fixed an issue after switching away from Mgmt / CLI to not be able to go back to the Mgmt / Overview due to a bug!
- Mgmt / GPS; GPS could not be disabled properly — fixed.
- MAP; fixed an issue where the MAP would not center on own GPS position when opening the MAP page.

### Removed
- Mgmt / UI -> Map; Removed overlay when there are no GPS Contacts with GPS coordinates available.

---

## [v8] — 2026-01-14

### Added
- Channels: Custom Command — configure via Mgmt → UI.
- Mgmt: Tele page — compact self-telemetry panel (VBAT, GPS, env values).
- Admin: System section — uptime, heap, PSRAM, flash/sketch usage, CPU, chip model/rev/cores, reset reason.
- Radio presets — predefined presets in Mgmt → Radio; selecting a preset applies settings and reboots.

### Changed / Improved
- Channels UI — redesigned layout; scroll-to-newest now marks all messages read.
- Mgmt / Log — layout updated to Channels-style with extended information.
- Mgmt / GPS — added RX/TX/Baud custom values.
- Battery press — short press toggles Volts ↔ Percent; long press still opens power popup; realtime duty-cycle display added.
- Contacts tab button — short press cycles filters (A–Z → Last heard → Last msg) and shows a 5s statusbar overlay; long press jumps to top.
- SenseCap — default text size increased (adjustable via ZOOM).
- Nick coloring — colored device names across UI (resets on reboot).

### Fixed
- Neighbor discovery — refactored and working.

---

## [v7] — 2026-01-11 (changes #2), [v6] — 2026-01-11 (changes #1)

### Fixed
- WiFi: connects but did not sync with App — fixed.
- Overscrolling could overdraw Back/Send buttons — fixed.

### Changed / Improved
- Top status bar: WiFi / BLE switching now triggers a reboot; setting is remembered.
- Page "Channels": unified message detail view — improved and fixed.
- Mgmt → Global: now shows the PublicKey and the first 2 letters of your PublicKey.
- Mgmt → BLE: added more information.
- Mgmt → WiFi: added more information (IP address, etc.).
- Mgmt → Log: completely renewed with more information.

### Added
- Contacts page: Tperm button — request telemetry from viewed contact.
- Contacts page: RT (RangeTest) button — sends "RangeTest - ACK?" automatically (customizable later).

---

## [v5] — 2026-01-11

### Added
- Firmware release v5 — merged firmware build for SenseCap Indicator D1Pro/D1L and LilyGO T-Deck Plus.
- Mail icon (unread indicator) in status bar: hidden = none; white = unread Channel; red = unread User.
- Battery icon popup — press the icon to show battery details.
- New RxLog — receive log screen.
- New Stats — on-device radio statistics screen.

### Changed / Improved
- Mgmt page: swipeable layout — more space and better overview.
- Channels page: swipeable views — Channel view vs User view (DMs).
- Contacts page: extra info/menus (WIP).
- Touch/display reaction: faster/more consistent UI response (WIP).
- Message details view: improved layout/behavior (WIP).
- WiFi password field: show/hide option.
- WiFi initialization + optimizations — improved stability.
- GPS settings visible (WIP).
- Map improvements.

---

## pre-version [2026-01-08]

### Fixed
- SenseCap RP2040 sensor telemetry → MeshCore telemetry — corrected LPP sensor types to avoid overflow; CO2 + TVOC index (plus temp/humidity) now map into proper MeshCore telemetry fields.

### Added
- ui-modern: Telemetry mini panel (Contact detail) — compact telemetry line in contact detail (voltage / temperature / humidity / CO2 / TVOC when available).
- Map: Tile view (T-Deck Plus) — tile view when SD card contains expected tile folder structure (if available).

### Changed / Improved
- ui-modern: Telemetry request behavior — changed to one-shot request when opening a contact; clear state shown: OK, NO ANSWER, or NO PERMISSION.
- Ping 0-hop improved — TRACE-based ping shows SNR there / SNR back and duration, or NO ANSWER after timeout.
