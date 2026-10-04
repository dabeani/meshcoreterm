# Changelog

All notable changes to this project.

## [v0.9.14]

### What's new

- **Airview — a live radio waterfall in Mgmt → Stats:** Tap **Airview** for a full-screen view of the signal level on your node's current frequency/SF/BW: time runs down, columns are signal level (2 dB steps), coloured from quiet (blue) to strong (red), plus a level histogram, noise floor, estimated sensitivity, packets marked at their own RSSI, and stats (peak, busy %, packets/min, last packet RSSI/SNR, symbol time, bit rate). It shows the radio's frequency, bandwidth, SF, CR and TX power. A colour lane, a faint stripe across the row and a packet feed tell advert (ADV), channel message (CH, with channel name or `#hash`), direct message (DM), PATH, TRACE, ACK, requests (REQ) and your own transmissions (TX) apart, with hops, RSSI, SNR and length. The type comes from the packet header, not the encrypted content; a power streak without a packet entry was not decoded (noise, other LoRa settings or a CRC error). A **?** button opens a scrollable help that explains every element (waterfall colours, cyan noise-floor and orange sensitivity lines, histogram, packet-type colours, each number and the packet feed); Back or **X** closes it. On short wide panels (Heltec V4 TFT) the stats, packet counts and legend move into a column beside the plot. The chip measures only the channel it is tuned to, so this is signal level over time, not a frequency sweep, and it never retunes the radio — the node keeps sending and receiving mesh traffic normally the whole time the view is open. Tap the device header's Back button (device) or anywhere on the view (WebUI) to exit. On the WebUI the device only samples while someone actually has the view open. Also fixed: tapping the Mgmt tab while Airview or a graph was open left the Mgmt overview without touch/swipe; the Airview legend text no longer truncates on small screens.
- **WebUI contacts list now updates when a contact is just heard again:** Hearing a known contact (for example a zero-hop advert that doesn't change its name, type, hop count, path or GPS) used to never reach the WebUI at all — the live-update check didn't look at last-heard time, only at fields that describe the contact itself. It now does, coalesced to at most once every 1.5 s so a burst of adverts from several contacts doesn't resend the list once per contact.
- **OpenStreetMap attribution on the Map:** The Map tab now shows the required "Map data from OpenStreetMap" credit in the bottom-right corner, matching Leaflet's own placement in the WebUI, small and legible on both. On the device it steps above the zoom/D-pad/ME buttons when those are on and reach that far down. In the WebUI it's on the main map and on the small map on a contact's detail page (which had no credit before; its "Open in OpenStreetMap ↗" link went to that location, not the attribution).
- **Resend a message you sent (#299):** Open the detail view of a channel message or DM you sent, on the device or in the WebUI, and a new **Resend** button sends the same text again as a brand-new message — the original stays untouched. On DMs it's the natural fix for one that shows "Not delivered".
- **Tap a shared contact/channel link to add it:** If someone pastes a MeshCore "biz card" share link into a channel, DM or room (`meshcore://contact/add?...` or `meshcore://channel/add?...`, the same format as the phone app's QR codes), it no longer sits there as dead text. In the WebUI it becomes an "Add contact" / "Join channel" button; on the device, tapping that message offers the same action, including inside a room server's Room Console. Either way you get a confirmation with the name (and contact type or channel name) before anything is added.
- **Mgmt → Sensors replaces Mgmt → Tele:** One page for everything about the node's own sensors, on the device and in the WebUI. **Live** shows every reading with a graph of its last 96 samples (recorded in the background, so the graphs are already filled when you open the page; tap a reading for a full-screen graph). **Status** shows GPS, location and, on the SenseCAP Indicator, the RP2040 link. **Calibration** lets you set an offset for every reading — temperature, humidity, pressure, distance, voltage, current, power, CO₂, VOC/IAQ index, light and altitude — saved per physical sensor so it survives reboots. **Telemetry** holds the refresh interval and the Base/Loc/Env telemetry permissions (moved here from the WebUI's Contacts page).
- **Chip temperatures are sensors:** The ESP32 die temperature (all boards) and the RP2040 temperature (SenseCAP Indicator) now appear as their own sensors, with a live graph, a calibration offset, and in the telemetry sent to other nodes.
- **All MeshCore I2C sensors on every board:** SenseCAP Indicator, CrowPanel 3.5 and T-Display P4 now look for the full sensor set (AHT10/20, BME280/BMP280/BME680, BMP085, SHTC3, SHT4x, LPS22HB, INA219/INA226/INA260/INA3221, MLX90614, VL53L0X), like the other boards already did. Only sensors that answer are initialised.
- **Alarm trigger match modes:** Mgmt → Alarm has a **Match** setting for the DM and the channel trigger: **Starts with** (default, as before), **Contains**, **Ends with**, **Exact**, or **Whole word** (the phrase anywhere, but not inside a longer word — `SOS` fires on "help SOS!" but not on "SOSO"). Device and WebUI.
- **Screen rotation on the SenseCAP Indicator:** Mgmt → UI → **Rotation** turns the whole screen in 90° steps (clockwise), so the device can be mounted any way round. Touch follows the rotation, it is saved, and it is also on the WebUI's UI page. Only the square SenseCAP screen can do this.
- **Sharper UI zoom:** Zoom now has four steps, **100 / 133 / 167 / 200 %**. 100 % and 200 % are exact pixel copies, so letters stay perfectly sharp; the two steps in between are scaled so every stroke of every letter keeps the same width (before, some strokes came out 1 px and others 2 px wide). On the SenseCAP Indicator, T-Deck Plus, CrowPanel 3.5 and Heltec V4, zoomed screens are also sent to the panel faster. A zoom saved by older firmware is rounded to the nearest step.
- **MCTerm logo:** The boot screen shows the MCTerm logo and the homepage (mcterm.net); the WebUI shows the logo in its loading screen, login box, status bar and browser tab.
- **SD card map tile cache:** Map tiles you download are cached on the SD card (on the SenseCAP Indicator: the RP2040's card) whenever the board has one, instead of the small internal flash. When the card fills up, the oldest tiles are removed automatically. Boards without SD (Heltec V4) still cache in flash.
- **GPS page without a receiver:** Mgmt → GPS (device and WebUI) now also shows when no receiver has been recognised, with the reason (disabled, UART failed, no data, no NMEA) and the pin/baud settings, so a wrong setup can be fixed from the UI.
- **User guide:** Every Mgmt page is now described from the firmware itself, including the new Alarm page, and several wrong statements were corrected (for example, channel sharing shows the secret as text — there is no QR code).

- **Emoji on the device (#38):** Emoji in names and messages now show as small colour pictures on all devices, instead of `?`. All single emoji of Unicode 18.0 and 259 country flags are built in (Google Noto Emoji). Combined emoji (like a family) show their first part, skin tones show the default colour, and number keycaps show the number. The on-screen keyboard has a new **EMO** page (after SYM2) with the 40 most used emoji. On the T-Deck, **Alt+E** opens it (Alt+S also cycles to it). The WebUI message boxes have an emoji button with the same 40. Emoji are only offered where they make sense (messages, names, quick texts, search), not in passwords, keys or settings. The emoji are sent exactly as the phone apps send them.
- **On-screen keyboard fixes (all touch devices):** Taps now always hit the key that is drawn there. Before, the keys and the action bar were placed slightly differently for touch than on screen, so on small screens, at higher UI zoom or with extra rows (password or reply buttons) a tap could type the key below. Swiping to change the keyboard page after touching a key no longer deletes an extra character. OK now behaves the same from the touch keyboard, the T-Deck button bar, the trackball and the Enter key; for example, sending a reply from message details now always returns to the chat.
- **T-Deck keyboard:** The Cancel / # / SYM1 / SYM2 / OK bar now fits the screen at every UI zoom (before, it ran off the screen edges above about 110 %) and has an **EMO** button where emoji are allowed. In the SYM1/SYM2/EMO pages, clicking the trackball now types the key it is on (before, the trackball only moved the highlight). The text cursor moves over whole emoji and characters.
- **Text is no longer cut in the middle of a character:** Long names shortened with "...", wrapped message lines and Backspace now always work on whole characters and whole emoji. Before, umlauts, Cyrillic letters or emoji could be split and show as `?`. The WebUI's message length limit also no longer cuts an emoji in half.
- **MeshCore 1.17.1:** The mesh/radio protocol is updated to upstream MeshCore v1.17.1 (About shows **v1.17.1**; the MC-Term interface itself stays **v0.9.14**). Your saved settings move to a new file automatically on first boot after updating — no action needed.
- **Multiple identities:** You can now keep more than one node identity (key) saved on the device at once — up to 4 on Internal storage, 8 on an SD card. Open **Mgmt → Identities** on the device, or **WebUI → Global → Identities**, to see every saved identity with its storage location, name, and short public key, and filter the list by All / Internal / SD. Tap a row to select it, then: **Use** switches to that identity and reboots; **New** creates a brand new identity (choose a name and Internal or SD); **Copy** duplicates the selected identity onto the other storage; **Del** removes it from the storage you're viewing. You must switch away from an identity before you can delete it, and the very last remaining identity can never be deleted. Switching only changes the active key and node name — your contacts and settings are untouched. SD backup slots also save and restore any extra identities, including on SenseCAP.
- **Settings are stored separately now:** Your device-only preferences (theme, map tiles, lock screen, etc.) and your radio/mesh settings are now saved to two separate places. This means connecting the phone app and saving mesh settings can no longer accidentally overwrite your device UI preferences.
- **More sensors supported:** An HC-SR04 distance sensor and the CrowTail Light Sensor 2.0 can now be read and shown as telemetry, on-device and in the WebUI (the I2C sensors are listed under *All MeshCore I2C sensors* above).
- **Two of the same sensor at once:** If you wire up two boards of the same sensor model (for example two BME280s at different addresses), both now show up as separate readings instead of one silently overwriting the other.
- **Sensor discovery on the boot screen:** The boot screen now shows exactly which sensors were found (name and address) as the device starts up, instead of nothing at all.
- **Replies now keep the original message's scope:** When you reply to a channel message, the reply is sent using the same region scope the original message was posted under, instead of switching to the channel's current default scope — so your reply reaches the same audience the original post did, even if the channel's scope has changed since. A **[Reply: scope]** tag (device) or **Reply: scope** label (WebUI) appears on the message box while this applies, so you can see it before sending.

- **Alarm in the WebUI:** Mgmt → Alarm is now its own WebUI page with every option from the device, including picking specific DM senders, channels and channel senders from your contact and channel lists.
- **Map tile source picker:** Mgmt → Map shows which tile server is in use and lets you switch between OpenStreetMap, OSM Germany, OpenTopoMap, Humanitarian and CyclOSM, or enter your own HTTPS URL. The WebUI map now uses the same source as the device. Tiles from different servers are cached separately, so switching servers doesn't show mixed or stale tiles.
- **Contacts search Clear button:** After searching, a **Clear** button on the right of the search box empties the search and shows the full list again.
- **T-Deck Plus keyboard symbols:** Alt+key now types the characters the keyboard has no key for (`= % ^ & < > [ ] \ | ~` and the backtick), and the editor lists them on screen. The old "Alt+3=#" hint never worked (`#` is Sym+Q); it has been removed.

- **INA219 power monitor:** An INA219 at I2C address 0x40 is now supported on all devices (newly enabled on CrowPanel 3.5, T-Display P4 and SenseCAP Indicator). Mgmt → Sensors (device and WebUI) shows its voltage, current and power, each with a calibration offset, and a **Range** choice (32V 2A / 32V 1A / 16V 400mA, for the usual 0.1 Ω shunt) that is saved with your settings. Voltage, current and power offsets work for the INA3221, INA226 and INA260 too. Note: telemetry carries power in whole watts, so loads under 1 W show 0 W. Another chip sitting at 0x40 (e.g. a Si7021 humidity sensor) is no longer mistaken for an INA219.
- **Keyboard settings only on keyboard devices:** The keyboard settings in the WebUI are only shown on devices with a hardware keyboard.
- **Keyboard blink settings in the WebUI:** Keyboard Blink, KB Blink Dur and KB Timeout from Mgmt → Light can now also be changed in the WebUI.

### Fixes and improvements

- **WebUI sync audit (contacts/messages incomplete):** reproduced on a real device with a new audit tool (`tools/webui_sync_audit.js`) — reloading while a sync was in flight, a rejected frame or a gap made the browser close the socket (`resync all`) and loop, leaving contacts empty/half-filled, and a message resync replaced the whole history by its first page (3–10 messages, i.e. "one message per channel"). Fixed in the browser: frames that do not belong to a section's active sync are dropped instead of closing the connection, a page gap restarts only that section, the contacts resync really restarts (it used to keep its old cursor and never complete), the old complete contact/message list stays visible while a resync pages in (swapped in at the end), a server-initiated first page without sync is turned into a normal sync, and the final contacts page now carries the device's contact count so a short list is detected and re-synced automatically. Messages are attributed to their channel by slot number (duplicate or renamed channel names no longer lose messages). Device: late/duplicate/stale acknowledgements no longer terminate the session, a page that could not be built completely is never sent as if it were complete, skipped contact slots no longer stall the sync, and sections held back by the heap guard or an overflow are retried. The same fix covers a session-killing race: a live update for a section (for example a new repeater on the Map or a contact being heard) that arrived while that section's own startup sync was still loading used to drop the whole WebUI connection so everything reloaded from scratch, repeatedly on an active mesh; a live update and a startup-sync reply are no longer treated as the same thing just because they are for the same section.
- **Full-screen firmware update progress:** while a firmware .bin is uploaded through the WebUI, the device wakes the display and shows a full-screen "FIRMWARE UPDATE" page with a progress bar, percent and KB counter ("Do not power off" → "Verifying image…" → "Saved - rebooting…"), swallows touches, and shows the error in red for 6 s if the update fails or the upload stalls for 30 s. The WebUI shows the real upload percentage instead of a static message.
- **Duty cycle is entered in percent (Mgmt → Radio and WebUI):** type **10** for a 10 % duty cycle instead of the airtime factor 9.000. The row shows the percent (10, 33.33 …), the airtime factor is shown read-only below it; allowed 10–100 % (the firmware's airtime-factor limit of 9 is a 10 % floor). Stored values are unchanged (the settings file and the WebUI key `airtime_factor` still work; `duty_cycle_pct` is the new WebUI key).
- **WebUI sync speed on all boards:** contact and message pages were paced at a fixed 200 ms per page whatever the page size, so boards with small pages (SenseCAP 6, CrowPanel 7 / T-Display P4 / T-Deck 3 contacts per page) were 2–6× slower than the CrowPanel 3.5 (10 per page). The pause now scales with the page size to the same ~20 ms per item whenever the internal heap has room; it stays at 200 ms (400 ms when very low) otherwise.
- **GPS off means no GPS at boot:** with GPS disabled in Mgmt / GPS the firmware no longer powers the receiver, opens its UART or probes baud/NMEA during boot (it used to do all of that and only then switch the receiver off, including a 1 s wait on some boards). The first time you enable GPS the full initialisation runs then. Until a receiver has been seen once, the GPS page shows "GPS not recognized: Disabled".
- **SD card writes/reads cut down (SenseCAP and other SD-mirror setups):** when the SD card mirrors the internal flash, every contact/prefs/channel save re-read all ~250 cached advert files and every state file from the card just to confirm nothing had changed (up to ~20 s and a 90 s stall of the main loop in a capture). The mirror now remembers size and checksum of what is already on the card, copies only the advert files that were actually written or removed, and skips the manifest and settings export when unchanged; a full compare runs only after a boot, a storage switch or an SD error. The UI-state snapshot is also no longer rewritten when its content did not change, and advert-only changes wait 30 s. Each mirror prints one `[MIRROR] … skip/write` summary line.
- **WebUI GPS page is live:** the GPS status block (status, link age, satellites, fix quality, HDOP, accuracy, C/N0, byte and message counts, PVT/UBX diagnostics, last byte, fix age, coordinates, module/chip/talker) now updates every second while the page is open, with the same rows and formatting as the device page, instead of only when the page was reloaded.
- **SenseCAP Indicator boot errors:** the I2C sensor scan I added for the Grove port saw ACKs at 0x40/0x5A/0x5C/0x77, then every driver failed with `ESP_ERR_INVALID_STATE` (about 180 error lines and ~2 s boot delay). It is disabled on this board again; the board's own sensors on the RP2040 are unaffected.
- **Lock screen:** Lock Background can now be a **Channel** (the newest messages of the channel you pick, always scrolled to the latest, without marking them read) or **Sensors** (the live readings from Mgmt / Sensors). A new **Preview** button on Mgmt / Lock (device and WebUI) shows the lock screen before you lock; the WebUI also has "Show on device".
- **Mgmt / UI in the WebUI:** UI Zoom, Color Scheme and Battery 100 % are now there as on the device (they were missing).
- **Storage switch:** switching the primary storage now asks Yes/No on the device and in the WebUI and says the device will reboot when finished.
- **WebUI on phones and tablets:** Dialogs and the login card taller than the screen (landscape, small iPhones, address bar showing) now scroll instead of being cut off, and respect the iPhone notch and home bar in landscape. When the on-screen keyboard opens, the app resizes to the visible area and hides the tab bar so the message box stays above the keyboard. Larger touch targets on touch screens, a compact login in landscape, tighter header on very narrow phones, and a roomier layout on iPad/Android tablets.
- **SD card → internal storage switch:** Switching back from SD card to internal storage could fail with "Blobs failed …/…" when some cached advert files could not be read from the card. The switch now waits for the SD link and checks again before giving up, skips advert files that are empty or missing on the card, and only aborts on a real copy failure. Every skipped or failed file is now logged with its reason, and the SenseCAP Indicator logs why an SD file check failed. The boot log now always shows the I2C sensor scan: which addresses answered and how long each sensor took to initialise.
- **WebUI messages sync:** Messages that arrived while the WebUI was still loading the message history were never shown until a reload. Changes to messages already on screen (delivery/ACK state, SNR/RSSI, repeaters, read) now update live instead of only after a reload. Sending the same text to the same contact more than once (for example "ok") no longer hides the earlier copies from the history and the counters. A flood message on a busy channel with several repeaters in range was resending the whole newest-messages page once per repeater that relayed it — several times within a couple of seconds for one message — which could be seen as the message list flickering or repeatedly resyncing; those updates are now coalesced into at most one resend per 1.5 s.
- **SenseCAP Indicator: new ESP32 ↔ RP2040 link.** The link between the two chips was rewritten with checked, numbered messages, retries and a faster negotiated speed, which fixes SD card timeouts and freezes. It also gives the RP2040 a clock (for the tile cache) and removes a ~6 s freeze after boot. **Install the matching RP2040 firmware from this release** — the new ESP32 firmware needs it.
- **SD-primary storage lost settings on reboot:** With Primary Disk → SD Card, the node settings were read from the wrong place at boot and replaced by defaults. Fixed.
- **Smoother screen updates:** The screen is only redrawn when something changed, lists move the already-drawn rows while you scroll, and only the changed lines are sent to the panel. Scrolling is noticeably smoother, most of all on the CrowPanel 3.5.
- **Phone app link no longer stalls:** A slow WiFi (TCP) connection could freeze the device for seconds while it waited to send; sending is now non-blocking. Storage usage is cached instead of being recounted on every app poll (up to ~1 s each on the T-Deck), and the keyboard and buzzer no longer wake the processor hundreds of times per second while idle.
- **WiFi power saving off:** WiFi modem sleep is kept off on all boards, so pings, the WebUI and the phone app get their packets promptly instead of late or lost.
- **Zoom could blank the screen:** If there was not enough memory for the new zoom size, the screen went blank and all touches landed in one corner until a reboot. Now the previous zoom is kept and **UI zoom failed** is shown.
- **Button labels no longer spill over their buttons:** Labels too wide for a Mgmt button are shortened to fit, and the value-button column is sized for the longest label on each board.
- **Mgmt identity menu:** "Tap again to confirm" messages were hidden behind the identity overlay and the next tap hit whatever was underneath. Use/New/Del now ask with a proper confirm dialog.
- **CrowPanel 7:** Switching between WiFi and BLE no longer disables the microSD card, and the board uses its correct pin definitions.
- **T-Display P4 build fixed.**
- **Contact list uses its full capacity:** The contact table now reliably allocates all 250 visible contact slots (plus 8 reserved for internal use) on boards with PSRAM, instead of potentially running short.
- **Quiet hours for sound and vibration:** On devices with sound/haptic support, you can now set a time range (including overnight, e.g. 22:00–07:00) during which notification alerts are silenced. Devices that don't support sound no longer show a setting that couldn't actually do anything. (#323)
- **Post directly to a room from Messages:** If you're already logged into a room server, you can now post to it right from its WebUI Messages thread — no need to go through the room admin screen. If you're not logged in yet, it prompts you to log in first. (#237)
- **Delete a chat without deleting the contact:** You can now clear a DM or room conversation from the device or WebUI without removing that person/room from your contacts. (#227)
- **Better repeater matching on short hashes:** When a short hash could ambiguously match more than one contact, the app now prefers an actual repeater match when there's exactly one, instead of essentially guessing. (#250)
- **More in-app help:** Added explanations for QuickSend/QuickReply 1/2, region scopes, and what each direct-message delivery status means.
- **Map zoom feels smoother:** Zooming the map no longer flashes to black or shows "Loading map…" placeholders — the previous view stays visible while new tiles load in over it, and all visible tiles now load together instead of trickling in one at a time.
- **Fixed a map-zoom crash:** Zooming the map while a tile was still downloading over the network could crash and reboot the device. Fixed.
- **Map tiles appear sooner:** The very first downloaded map tile now appears as soon as it's ready, without needing to drag the map first. Writing tiles to storage is also delayed until you leave the Map screen (or the screen turns off), so it no longer causes a brief freeze while you're using the map.
- **WebUI loads more reliably on memory-constrained boards:** Adjusted how the WebUI's network buffers are allocated so the web page and its assets load more reliably without exhausting the device's working memory.
- **WebUI reconnects correctly after an update:** If your browser had an old cached copy of the web page open, it now detects the mismatch and reconnects cleanly instead of silently failing to connect.
- **Fixed: WebUI Contacts/Messages could get stuck loading after login.** If a data update to the browser hit a temporary snag, it could retry too aggressively and get stuck, blocking Contacts, Messages, and other tabs from ever loading — sometimes also affecting the overall connection. All web data now retries with a backoff (waiting a bit longer each time) instead of hammering the connection.
- **Clearer WebUI error logging** for developers/support diagnosing a stuck connection — no user-visible change.
- **WebUI Storage browser is safer on large folders:** Browsing a large folder of files (Internal or SD) in the WebUI's Storage tab no longer risks crashing the connection.
- **Fixed: SD card status could be wrong in the WebUI.** On some boards (notably SenseCAP Indicator), the WebUI's Storage tab could report the SD card as unavailable even though it was working fine and other parts of the app were using it successfully. Fixed by having all parts of the firmware share one true "is the SD card mounted" status instead of each tracking it separately.
- **Quieter boot log:** Removed a harmless but noisy error that appeared during boot cleanup on some setups.
- **Slightly less memory used** for the on-device contact list sorting, with no visible change in behavior.
- **Phone app contact sync:** T-Deck Plus and Heltec V4 TFT now send contacts to a connected phone one at a time as it asks for them, matching the reference MeshCore app behavior.
- **Clearer wording:** The WebUI's "Manual add" contacts setting is now labeled **Manual add (auto-add off)** so it's clear this only matters when auto-add is off — it doesn't mean "never add."
- **Contact auto-add type filters fixed:** After the 1.17.1 update, the per-type contact filters (User/Repeater/Server/Sensor) were only reachable while auto-add was already on — exactly when they had no effect. The **Set** button now opens in both modes; the filters themselves still only apply to manual-add.
- **Phone app protocol updated for MeshCore 1.17.1 compatibility:** with auto-add on, any named device type can become a contact; a DM you type on the phone now shows up as sent on the device screen too; contact "last heard" now always uses the device's own clock; and several smaller protocol edge cases were aligned with the reference implementation. Note: DMs you type on the device itself still don't push to the phone app (only the reverse works so far).
- **Fixed a T-Deck Plus reboot loop after connecting a phone/WebUI.** A specific serial-connection code path could crash and continuously reboot the device (your contacts were safe throughout — they were never wiped). Also fixed a related issue where checking the WiFi signal strength too often could starve the WiFi driver of memory and crash it.
- **On-screen keyboard feels faster:** Letters, space, and backspace now register the instant you touch them instead of waiting until you lift your finger. The **#** key is now on the main keyboard page instead of a hidden layer. T-Deck Plus's physical keyboard also gained a dedicated **#** (Sym+Q).
- **More consistent touch response** across all touchscreen boards — screens no longer get asked to redraw faster than they can actually keep up with.
- **Waking the screen is smoother:** Turning the display back on no longer redraws everything in a visible loop. On boards that only dim rather than fully power off the screen, your last view reappears instantly with the backlight.
- **Fewer freezes while using the screen:** Background saves (contacts, settings, etc.) now wait until the screen is off before writing to storage on every touchscreen board, so they don't cause a visible stutter while you're actively using the device. Manual reboots and backups still save immediately, as before.
- **Fixed: storage space showed 0 KB** on boards without an SD card (Heltec V4).
- **Fixed: brief screen glitches during saves** on CrowPanel and other panel-driven boards, where a background storage write could briefly show garbage on screen mid-save.
- **Fixed: a disconnected GPS module could be reported as present.** The device now correctly notices when nothing is actually responding on the GPS port.
- **Clearer GPS status on the map:** The map now distinguishes **GPS off** / **GPS disabled** (you turned it off) from **No GPS fix** (GPS is on but hasn't found a position yet). (#310) The **GPS off** / **No GPS fix** bar is also drawn on top of the map tiles, so it is no longer covered by them or smeared when you drag the map.
- **Smoother map performance:** Map tile loading (both from storage and over the network) now happens in the background instead of on the same thread that draws the screen, so the map stays responsive while tiles load, and leaving the Map screen properly cancels any tiles still loading.
- **Contacts update more completely when heard again (#335):** Hearing a contact's advert again — even one that's just passing through your region scope — now also refreshes their last-seen time and location, not just their signal path. This happens only after the advert's signature is checked and only if it is not older than the last accepted advert, so a forged or replayed advert can no longer move a contact on the map.
- **GPS AutoBaud setting now actually sticks:** Previously it could silently reset to "Enabled" after a GPS restart or reboot. It's now saved immediately and stays as set. (#326)
- **Factory reset is more thorough and predictable:** After a factory reset (from the phone app or the `erase` command), the device now consistently starts with no contacts, only the default Public channel, no region scopes, and no saved WiFi profiles — this now also works correctly when your Primary Disk is an SD card. Your identity and any SD backup slots are kept. Restoring an SD backup slot brings back contacts, channels, region scopes, WiFi profiles, and any extra identities from that slot.
- **Reboot button always works:** Mgmt → Global/Admin → Reboot no longer refuses to restart (with a confusing "Save failed" message) just because a channel save was still pending in the background.
- **Fixed: couldn't re-add a WiFi profile after a wipe/reinstall** if the saved profile file was missing or corrupted — it's now automatically rebuilt. (#312)
- **Repeater admin "Owner" field fixed:** Now correctly shows the repeater's own owner info text instead of repeating its device name. (#331) In the WebUI (Repeater Admin → Device) the owner text was already requested but the answer never reached the browser; that is fixed too (#331).
- **CrowPanel 3.5 Adv:** This variant has no battery sensor, so the battery icon now consistently shows as an empty outline instead of a misleading reading. (#336)
- **Contact hop-count history is more reliable:** The last-known hop count for a contact now survives a reboot and stays correct even after hearing many other nodes, instead of resetting to unknown. (#325, #224) A contact first heard over several hops and later heard directly now shows **Direct** instead of the old hop count; the map ID badge keeps its size.
- **Fixed: new contacts showed a bogus "last heard" date.** Manually added contacts (including adding yourself) now correctly show **Last heard: --** until they actually send something, instead of a date from 1970 or an absurdly large number of days. (#247)
- **Flood-routed DMs labeled correctly:** These now show **Path: Flood** instead of incorrectly showing Direct. (#211)
- **Repeater/relay counts on sent messages no longer disappear:** Seeing how many nodes relayed your DM or room post now survives the message's delivery status changing afterward. (#228)
- **Clearer "unknown value" display:** Fields with no known hop count, SNR, or RSSI now consistently show `--` instead of a stray `?`. (#308)
- **AutoAdd Max Hops setting explained clearly:** Now shows in plain terms what the number means — **No Limit**, **Direct (0 hops)**, **1 hop max**, **2 hops max**, and so on — instead of just a raw number. (#169)
- **Chat display fixed:** Messages no longer occasionally overlap each other or leave odd extra blank lines. (#301, #181)
- **#226 #255:** A DM with no contact name shows the public-key hash (for example `-- [AB12]`), not **User**. In the WebUI, a DM chat whose contact was removed or has no name now shows its messages too (before, the chat opened empty): the WebUI matches messages to the chat by the contact's key, not its name, and shows the same name as the device — the contact name, else the saved chat name, else `-- [AB12]`. The chat header on the device now uses that same name as the DM list.
- **#131:** Chat no longer shows `<-->` as the sender. A region name at the start of a channel message is not used as the nick, and a sender name of 24-31 characters (allowed by the 32-byte node-name limit) is now parsed correctly too — previously it silently fell back to `--` and left the raw `Name: text` prefix stuck in the message body instead of showing it as the nick.
- **Repeater Admin: Default region row wasn't marked on-device for longer region names.** A repeater's default region name of 24-30 characters (allowed by the firmware's own 31-byte region-name limit) was silently cut short before being compared against the region list, so that row's button always showed **SetD** instead of **Def** even though it already was the default — a device-only bug (the WebUI compared the untruncated name and was already correct). Fixed by storing the full name.
- **#232 #231 #282:** Copy no longer freezes the screen. Tap opens the message; hold or drag to copy. The DM list no longer stays in copy mode after you leave a message. The clipboard is also saved in the background once you stop using the screen (or when it turns off), like other saves, so copying no longer causes a short stall a moment later; Reboot still saves it right away.
- **#281 #101:** Room and repeater login stays when you change tabs, go back, or open the same contact from the map. Opening a different contact still starts a new login.
- **#180:** Room and repeater login screens show that contact’s name.
- **#137:** Scrolling Msgs, DMs, Rooms, or Discovered now shows the partly visible row at the top of the list (same as Contacts). Turning the screen off fades the display (~0.3s). On T-Deck Plus the keyboard backlight fades with it. T-Display P4 still turns off immediately (no brightness fade).
- **#104:** Hold on a room-console message copies it.
- **#139:** When Public is missing, Mgmt → Channels has a **Restore Public** row of its own (not on the Join row). Join stays Join-only.
- **#279:** An old compact channel file is rewritten once to the slotted format so channel slots and scopes stay aligned after reboot.
- **#158:** Display Blink on a new message wakes a timed-off screen and paints, then returns to off if you do not touch. A screen you turned off yourself stays off. SenseCAP Indicator uses the same blink as the other boards.
- **#83:** **Battery 100%** (now at the end of Mgmt → UI, on the device and in the WebUI) sets the voltage that means full (3500–4500 mV, default 4200). Percent and the battery icon use that value. Saved with device UI prefs. Tap still cycles percent / volts / duty.
- **Repeater Admin Status:** After login, saving the password no longer rewrites the whole contact store. Status opens without freezing the device for minutes.
- **Mgmt Backup:** A new slot copies the identity and settings of the **selected** Primary Disk (Internal or SD). The suggested name is that identity’s first 3 public-key bytes plus the local date/time (`AABBCC-DDMMYYYY-HHMMSS`). Message/map extras (quick-send, auto-advert, network tiles, tracking, …) and the lock-screen PNG are stored in the slot and restored into Internal NVS or the SD file, matching the current disk. Slots stay on the SD card only.
- **#121:** Some T-Deck units need the LilyGO touch-fix image at address `0x0`, then the firmware again.
- **#328:** Heltec V4 Expansion 10-second power-off depends on the board revision. There is no extra software power-off pin.
- **#314 #311 #283:** The last successful clock set (manual, GPS, NTP, or message) is kept and used after reboot. Contact last-seen times no longer pull the clock back to an older or once-wrong date. GPS Sync, message sync, NTP, and Set Time all use the same apply path.
- **#217:** Turning GPS on no longer overwrites **Manual Lat/Lon**. Those stay until you tap **Capture GPS fix**, **Use map center**, **Clear Loc**, or set them from the phone app. Turning GPS off no longer resets them to `0.000000`.
- **#216:** Disabling GPS now powers the module down (u-blox sleep / EN pin) and closes the GPS UART so the chip cannot stay awake. The sleep command is flushed before the UART is torn down. Boot no longer pulses GPS reset after disable (Heltec V4). Re-enable reopens the UART. Map / ME / follow-me still show the live fix while GPS is on.
- **Primary disk switch:** Copying MC-Term settings to Internal no longer fails with NVS `KEY_TOO_LONG` (`settings_migrated` was 18 characters; ESP32 allows 15). SD → Internal can finish after contacts copy.
- **#329:** SenseCAP Indicator: tapping the screen no longer freezes the display for minutes while Backup, Map, or WebUI Storage use the SD card. The screen keeps updating. **#277** SD/map/WebUI lock-up is the same fix; connecting a phone over BLE is unchanged.
- **Elecrow CrowPanel 7 sensors:** Sensors were silently compiled out for this board entirely — none of the environment sensors could ever be detected no matter what was wired up. All sensor types are now genuinely active.
- **T-Deck Plus and T-Watch 2019 sensors:** AHT20/AHT10, BME280, and BMP280 were explicitly disabled on these boards; re-enabled, plus BME680 added on both.
- **CrowPanel 7 WebUI — Contacts/Messages not loading:** After logging into the WebUI, Contacts and Messages could stay stuck loading forever once a WiFi profile was configured, because a "settings" data packet was silently failing to send and blocking everything queued behind it. Fixed.
- **CrowPanel 7 WiFi/Bluetooth co-processor reliability:** The link between the main chip and its WiFi/Bluetooth co-processor now uses the same speed setting as Elecrow's own official firmware, instead of an untested default that could cause dropped or stalled connections.
- **CrowPanel 7 crash during WiFi recovery:** If the WiFi link needed to reset itself to recover, the device could crash immediately afterward. Fixed.
- **CrowPanel 7 save speed:** Saving contacts and settings to internal storage is now noticeably faster (previously could pause the UI for several seconds).
- **CrowPanel 7 automatic recovery:** If available memory becomes too fragmented (which could previously make WiFi/WebUI silently stop responding until you manually toggled WiFi off and on), the device now detects this and recovers on its own.
- **Lock-screen picture vs. WiFi DNS setting conflict:** Editing the on-device "WiFi Static DNS1" field could silently erase your custom lock-screen picture, and editing the lock-screen picture could corrupt the DNS1 setting — the two fields were accidentally sharing the same internal slot. Fixed; each is now independent.
- **TX Power field accepted the wrong range on-device:** The on-device Radio screen rejected negative power values and displayed them incorrectly, while the WebUI already allowed down to -9 dBm. Both now accept and display the same range consistently.
- **Map tile URL template not refreshing:** If you changed the custom map tile URL from one place (device, CLI, or another browser tab), an already-open WebUI tab wouldn't pick up the change until manually reloaded. Fixed.
- **WiFi could crash/freeze the device:** Turning WiFi on while the device's memory was fragmented could crash the WiFi radio driver and hang the device, requiring a hard reset. The device now detects this condition and safely falls back to Bluetooth instead.
- **Bluetooth could stay unusable after a WiFi/WebUI session until a full power cycle:** Even with the crash above fixed, Bluetooth could still refuse to start afterward, on every retry, because two internal data tables were permanently reserving a large chunk of memory they didn't need to. Freed that memory (~78 KB on affected boards) so Bluetooth can start normally again right after using WiFi/WebUI, without needing a reboot.
- **Mgmt pages could hide their last row(s) on scroll:** On Mgmt → BLE, UI, Map, Sound, GPS and Stats the scroll length of the page was undercounted (by 1 row, by 3 on GPS: e.g. the Quiet Start/End rows on Sound, the tile source rows on Map, the Airview row on Stats), so scrolling to the bottom could stop just short of the last setting and the bottom row could be unreachable on small screens. Fixed, and an internal check (serial warning `[MGMT_ROWCOUNT] frame=N undercounted`) makes sure a future added setting can't quietly repeat this on any page.
- **A new channel could silently inherit an old, unrelated region scope:** In Mgmt → Channels, tapping "Scope" on an empty channel slot let you assign a region scope to it even though no channel occupied it. If you later added or joined a new channel that landed in that same slot, it would silently start using that leftover scope instead of the mesh-wide default — without you ever choosing it for that channel. Fixed: the Scope action is no longer available on empty channel slots, at every level (button, tap handler, and the underlying save function).
- **Heltec V4 (TFT/modern): GPS took nearly a minute to give up at boot, and never actually worked.** Two compounding bugs: the GPS UART's RX/TX pins were swapped specifically in the TFT/modern build (not the plain Heltec V4 build), so the GPS module could never be heard from at all; and the firmware assumed the wrong GNSS chip family (CASIC/AT6558 instead of the Quectel L76K that Heltec's own product page for this exact expansion kit specifies), which made it run a slow, exhaustive multi-baud/multi-protocol scan that could never succeed anyway. Both fixed — GPS should now be found quickly and actually work on Heltec V4 boards with the expansion GNSS module attached.
- **Opening Mgmt for the first time after boot could freeze the screen for over a second:** Reading how much internal storage is used (for the Storage row on the Global page) has to scan the whole filesystem's allocation table, which is slow — and it was doing that scan on the same thread that draws the screen. Moved it to a background task so it can never block a frame; the Storage and Sketch Size rows now show the last known value for a moment while the fresh one loads instead of freezing the whole page.
- **Opening Mgmt → Backup was slower than it needed to be:** Listing your backup slots was reading each slot's info file from the SD card twice per slot (once to sort the list, once to actually show it). Now reads it once.
- **Mgmt → Global is much smoother:** (1) The Identities row used to re-read every saved key from storage on every redraw; it now reads the list once and reuses it. (2) Most rows (Name, Public Key, Identities, Save/Load key, Set private key, Primary Disk, Reboot/OTA/C6-update buttons) were fully redrawn every frame even while scrolled out of view, although only 6-8 of ~20 are visible; only a background rectangle used to be skipped, now the text and buttons are too (the same applies to the brightness sliders on Mgmt → UI/Light). (3) Holding a tap still on Global forced a full repaint and display refresh on every redraw tick (roughly 200-260 ms each, 400 ms+ stalls on SenseCAP Indicator); a stationary hold is now skipped entirely, while any real action (scrolling, a button press, a popup) still redraws immediately.
- **Tracking never actually reported where you'd moved to.** With Tracking enabled and Advert Location set to GPS, the node correctly noticed when you'd moved past the configured distance and sent an update on schedule — but that update always contained your old, one-time "Capture GPS fix" position, never your current one, because Advert Location = GPS and Advert Location = Manual were sending the exact same (non-live) coordinate. GPS mode now genuinely uses your live position when sending any self-advert (auto, manual "Advert" buttons, or Tracking-triggered), falling back to the last known position only if GPS momentarily has no fix; Manual mode is unchanged. Also added a persistent warning (device and WebUI) when Tracking is on but Advert Location is Off, since that combination silently has nothing to send.
- **Repeater Admin: adding a region scope never actually worked.** Both the on-device UI and the WebUI sent a "region add" command that doesn't exist in the repeater's firmware, so it always silently failed with a generic error reply; the real command is "region put". On top of that, the on-device "Add" button in Regions wasn't actually drawn (only Refresh/Save were), even though tapping near them could accidentally trigger it anyway due to a layout mismatch — it's now a real, visible, correctly-placed button. The WebUI also no longer mistakes that error reply for a bogus region named "Err" in the list.
- **Repeater Admin: "Default region" was completely missing.** Home region has always worked, but the separate "default region" (the repeater's fallback flood scope) couldn't be viewed or changed from either UI. Both the on-device UI and WebUI now show the current default region on the Regions screen and let you set it per-region, alongside the existing Home control.
- **Repeater Admin: Status screen didn't show whether the repeater was actually repeating.** The "Repeat" on/off setting was only visible on the Device tab; it's now also shown on the Status tab of both UIs, since that's the more natural place to look for it.
- **Repeater Admin: on-device Status screen was missing "Recv Errors."** The WebUI's Status screen already showed this counter; the on-device version now shows it too.
- **Spurious "does not exist" storage errors in the debug log on every save.** Saving the advert-path table (and other atomically-committed files sharing the same save routine) always deletes its `.tmp`/`.bak` helper files right after a successful save — so the very next save always finds them absent, and simply checking for that produced a scary-looking `open(): ... does not exist, no permits for creation` error line from the underlying storage layer, even though nothing was actually wrong. These checks are now silenced (existing debug builds with storage I/O logging explicitly enabled still see them).
- **WebUI Contacts could stop loading after just the first handful of entries.** On devices with enough contacts that the list needed more than one page to load, the browser could silently give up after the very first page, showing only a handful of contacts instead of your full list — with no error shown anywhere. The page-numbering the device used to tell the browser "here's the next batch" could repeat the same number across two genuinely different batches whenever a batch came back smaller than usual (which happens under memory pressure, or simply near a size limit), and the browser treated that repeat as "I already have this" and quietly stopped asking for more. Fixed so every batch is numbered correctly no matter its size; Contacts should now always load in full.
- **Harsh/buzzy sound on T-Deck Plus, T-Display P4 and CrowPanel 7:** Notification tones and the Alarm siren sounded distorted because part of every generated audio block was thrown away whenever the speaker's playback buffer was full, splicing the waveform many times per second. Every sample now reaches the speaker, which also makes the Alarm siren sweep at its intended speed.
- **T-Deck Plus could crash when pressing Reboot in Mgmt:** The state save that runs before a reboot (and in the background) put a ~10 KB table on the main program's working memory (stack), overflowing it and crashing instead of rebooting cleanly. Those tables, and the settings-save buffer, now live in separate memory, freeing roughly 14 KB of headroom.
- **No black map after dragging:** Letting go after moving the map no longer blanks it for about half a second while tiles reload; the map stays visible and only missing tiles are filled in.
- **Smoother map dragging:** Contact markers no longer recalculate every contact's map position on every frame (only when a contact's location changes), and moving the map image while dragging now copies it once instead of three times. On T-Deck Plus the screen is also updated at twice the previous SPI speed.
- **WebUI security: firmware could be uploaded without logging in.** The WebUI checked the login only after an uploaded firmware file had already been written, so anyone on the same WiFi could flash the device. Firmware, map-tile and lock-screen uploads now require a valid login before any data is accepted.
- **WebUI login hardened:** After 5 wrong PINs, further attempts are delayed (up to 1 minute between tries) so the 6-digit PIN can no longer be guessed quickly, and the PIN is no longer sent in the page address.
- **WebUI: two browsers at once.** Logging in from a second browser (e.g. phone and laptop) no longer logs the first one out.
- **Faster screen updates on Heltec V4:** The display is now driven at twice the previous SPI speed, like T-Deck Plus.
- **CrowPanel 7 layout:** The Contacts list's Up/Down buttons, the contact type badge (e.g. RPT) and the RSTPath/ZeroPing buttons on a contact's detail page were drawn at small-screen sizes while their text uses the panel's larger font, so labels overflowed or were cut off. They now size themselves to their text. The RSTPath/ZeroPing tap areas also now line up exactly with the drawn buttons (on all devices).
- **Alarm keyboard blink:** Mgmt → Alarm → Keyboard blink was invisible while the screen was on (it blinked between two equal brightness levels). It now really blinks, and the keyboard-light timeout no longer cuts in during a blink.
- **Alarm display brightness:** On some devices the alarm popup set the screen to a fixed low brightness instead of your Mgmt → UI brightness, and a message blink running at the same time could darken it again. The popup now uses your brightness and stays on until the alarm is silenced.
- **Log shows all packets:** Mgmt → Log (device and WebUI) now always includes the packet bytes, even for packets received while the Log page was closed. Copies of your own packets heard back through repeaters are shown and marked **ECHO** instead of being hidden. This also stops some packets from other nodes being hidden by mistake. The WebUI log also marks **TX** / **ECHO** entries.
- **Heltec V4 GPS:** The GPS receiver's standby pin (GPIO40) was never driven, so the receiver could stay asleep, and it was probed only 50 ms after power-on. The pin is now held awake and the first probe waits 1 s.
- **SenseCAP Indicator sensors in telemetry:** The built-in temperature, humidity, CO₂ and VOC readings from the RP2040 coprocessor were only shown on the device's Tele page and never sent to the app or other nodes. They are now normal telemetry (sensor "Indicator") and can be calibrated on Mgmt → Sensors.
- **Log: scoped messages you send:** Packets you transmitted with a region scope never appeared in Mgmt → Log (device and WebUI). They are now logged like all other transmissions.
- **Sensors: no more hangs or bogus values from failed reads.** A sensor that dropped off the bus (e.g. a loose cable) could freeze the device during a telemetry read (AHT10/AHT20, SHTC3, BME280, BMP280 drivers wait forever for it), and failed reads were sent as random numbers. Unreachable sensors are now skipped, failed or out-of-range readings (including VL53L0X/HC-SR04 "0 m") are left out, and an SHT4x at 0x44 is no longer also reported as a fake INA226 current sensor.
- **More accurate BMP280 and SHT4x readings:** The BMP280 now measures on request instead of continuously (less self-heating, like the BME280), and the SHT4x uses its high-precision mode.
- **Repeater Admin: ReNeigh/DelNeigh buttons on the Neigh screen never worked.** Tapping either button did nothing, on every device — a coordinate bug made the button's tap area start to the right of wherever you actually touched, so it could never register a hit. Fixed; also aligned the drawn buttons with their tap area exactly (previously slightly mismatched, though not enough to cause missed taps).
- **#303: Map nodes are easy to open again, and the map no longer leaves old markers behind.**
  - **Opening nodes (device):** Tap a node to show its popup, then tap the same node again (or the popup) to open it. A tap now counts anywhere on the node's ID badge plus a finger-sized margin around it (larger at higher UI zoom), and the nearest node wins. A tap with a little finger movement no longer turns into a tiny map drag, and the yellow touch dot is no longer drawn on top of the node you tapped.
  - **Wrong contact opened (device):** The map picked contacts from a list that was shifted by 8 places, so opening a node from its popup showed a different contact or did nothing, and **Contacts → Map** could highlight the wrong node. The map now always uses the right contact.
  - **Old dots and popups stayed on screen (device):** When only the markers changed (your GPS position moved, a popup opened or closed, a node was heard), the new markers were drawn over the old picture without erasing it. Your own GPS dot showed up several times, and a closed popup stayed visible. The map now repaints the area under anything that moved or disappeared, and only there, so it stays fast. Dragging the map with a popup open no longer smears the popup.
  - **Map kept redrawing after it was done (device):** A frame that took slightly too long always scheduled another full frame, over and over, which kept the screen busy and could swallow taps. The map now stops redrawing once everything is on screen.
  - **Nodes on top of each other:** On the device, tapping the same spot again cycles through all nodes there and the popup shows **(1/3)**. In the WebUI the popup shows **(1/3) nodes here** and a **Next ›** button that opens the next node at that spot.
- **Repeater Admin → Device showed values in the wrong fields.** A repeater's reply to a command doesn't say which command it answers. If a reply came in late, the next field on the page took it. For example, **FW-MC** could show the radio settings (`869.61…,62.5,8,5`) while Freq/BW/SF/CR stayed `-`, and **SetRepeat** stayed `-` after opening Device from Status. Now each field only takes a reply that looks right for it: radio, version and home-region replies always go to their own field, even when they come in late.
  - **Missed Power value:** When a reply took too long, the page skipped the next value (e.g. **Power** after a slow radio reply). It no longer skips anything. The wait time now depends on the route (longer on flood or multi-hop routes) instead of a fixed 4.5 s.
  - **Refresh / reopening Device or Advert** no longer drops a reply that is still on its way, which let the flood advert interval land in the other advert field.
  - The loading indicator now stays on until all values are loaded, including the regions.
  - **WebUI:** The same fixes apply. Before, any reply the WebUI didn't recognise was shown as the firmware version, and any stray number as TX power.
- **Mgmt → Channels: default region, channel regions and new channels were not saved reliably (device and WebUI).**
  - **Default region (and other radio/mesh settings) went back to an old value after every restart (SD storage):** An old settings file from earlier versions (`/MCTerm/mesh_prefs.txt`) was still read at every start, and it overwrote the saved default region, radio settings and node name. This file hasn't been updated since the switch to the new settings file. It is no longer read and is deleted automatically at the next start (also if an old backup brings it back). After updating, set your default region once more.
  - **New channels got a region they were never given:** Channel slots without a saved region were loaded as region #1 of your list (e.g. `at`) instead of **None**. A channel you added into such a slot (e.g. `#graz`) then sent with that region right away. Empty slots now load as **None**. Channels already saved with an unwanted region keep it; set them to **None** once in Mgmt → Channels.
  - **Changes were only written when you left Mgmt:** A new or deleted channel, a channel's region, the region list and other Mgmt settings stayed in memory as long as the Mgmt page was open. A reset or power loss before leaving Mgmt (or before using Reboot in the menu) lost them. Channel and region changes are now saved right away. Other Mgmt settings (including radio settings such as the Airtime Factor) are saved within about 8 seconds, even while you stay in Mgmt, so a power cut or reset-button press in that short window no longer discards them. Saving never happens while your finger is on the screen. (SenseCAP Indicator keeps its old behaviour, because of how its screen and storage share hardware.)
- **#201:** Tapping the Route line in message details opens the map with the hop path again. The tap position was calculated wrong when a message had a "Sent" or "Scope" line, so the tap did nothing.
- **#229:** "Show on map" from a contact now shows that contact even when "follow me" (R) was on; before, the map jumped back to your own position.
- **#223:** T-Deck: Alt+S now cycles lowercase → UPPERCASE → SYM1 → SYM2, as the 0.9.12 notes said. The editor shows "abc" or "ABC", and letters typed on the hardware keyboard follow it.
- **#110:** Mgmt → Advert: **Auto Advert Flood** now goes up to 72 h, 1 week (168 h) and 255 h (about 10.6 days, the maximum the setting can store). **Auto Advert 0-Hop** can now also be set to 1 h. A value set in the WebUI that isn't in the list now steps to the next larger option instead of jumping back to the start.
- **#300:** The boot screen now shows the real build date of the firmware instead of a fixed date.
- **#200 (WebUI):** Mgmt → GPS in the WebUI now has a **Use Map Center** button, like the device. It uses the open map, or the last map view.

---

## [v0.9.13]

### What's new

- **Serial Logging:** Activated Serial Console Logging for extended debugging. If you know how to deal with Serial Logging, and you want to report more details when you experience bugs, send me the output from serial. Try to shorten it to the magic moment :)
- **MeshCore update:** Firmware is now based on upstream MeshCore **v1.16.0**.
- **Unscoped flood:** You can turn unscoped flood on or off from Mgmt → Channels and in the WebUI (advanced mesh option).
- **WebUI:** More device settings match the on-screen menus (multi-ACKs, RX delay, RX boost, auto-add contacts). About screen shows base **v1.16.0**.
- **LilyGo T-Display P4:** Larger, sharper UI (clearer 2× text); SDIO WiFi/BLE module and SD card support; improved boot and touch in portrait mode (including screen edges); serial debug over USB; SDIO WiFi/BLE reconnect and SD mount fixes.
- **Clock sync:** Mgmt → Date/Time uses **Sync Prio 1 / 2 / 3** (each: Off, NTP, GPS, or Message). Set all three to **Disabled** to stop automatic clock updates; manual set and GPS sync still work. **NTP Sync every** chooses 1h–48h or one-shot. **Clock Status** shows which source is active. WebUI matches.
- **Contacts search:** Find contacts by name or public key (8+ hex digits). Pull down from the top or tap the search row; list stays visible above the keyboard while contacts scroll underneath.
- **Keyboard:** Starts in lowercase; **^** = shift, **Sym** = symbol pages.
- **Compose:** First letter is capitalized by default when you start typing a DM, channel, or room message.
- **Discovered contacts:** Swipe left on **Contacts** (or long-press the tab ~1s) for unsaved adverts; swipe right to return.
- **New installs:** **Auto Add Contacts** and all **Auto Add Types** are on by default.
- **Coprocessor info:** CrowPanel 7 shows ESP32-C6 versions and update status; SenseCAP Indicator shows RP2040 link and sensor/SD role.

### Fixes and improvements

- **WebUI ping / connection:** During large Contacts/Messages sync, map tile sends, and Storage loads, a busy socket no longer forces a disconnect; ping recovers when the link has room; faster dead-socket detect and reconnect. On T-Deck Plus, large transfers no longer stall ping while the screen is redrawing.
- **T-Deck Plus boot:** Fixed a crash right after boot caused by reading touch from a background task; touch and keyboard stay on the main UI loop.
- **Repeater admin:** Re-login after logout uses the correct route again (Flood when the repeater is multi-hop); login status shows Flood / Direct / hop count.
- **SD-primary concurrency:** Large contact saves no longer corrupt the SD card or crash while channels/prefs also write.
- **Companion WiFi sync:** Large contact sync no longer overruns the WiFi queue; prefs/channels saves wait while the phone link is busy.
- **Companion BLE (hosted C6):** Large BLE updates send in smaller chunks instead of dropping mid-transfer.
- **Companion offline queue:** Offline BLE/WiFi sync depth is the same across ui-modern boards (larger shared queue).
- **WebUI contacts/messages:** Large contact tables and the Messages tab load in pages so they no longer crash or truncate under memory pressure; route badges and hash labels match the device.
- **WebUI Mgmt → Storage:** Opening Storage no longer kills WiFi ping or falsely reports "failed to load" (paced loads, capped folder listings); Mgmt prev/next wraps last→first and first→last consistently.
- **Map network-only tiles:** Fixed MAP tab hang when local tiles are off and network tiles are on (tiles no longer stuck as permanent misses while downloads are still running).
- **Map progressive load:** Pending tiles keep the map visible while more load in (including Heltec V4); a full queue waits instead of showing a false “miss” after ~12s.
- **Map tab re-entry:** Returning to MAP after another tab repaints cached tiles immediately instead of leaving blank map holes.
- **Map tab leave:** Leaving MAP clears only the map content area so the next tab paints faster.
- **WebUI map tiles:** Tile requests no longer block the network task (ping stays responsive while the map loads); network tile fetches no longer freeze the map UI for the whole transfer.
- **Message detail:** Radio info uses a clearer two-column layout; **Path** (Direct / Flood / hop count) is shown in the grid instead of a separate line.
- **Channel privacy:** Lock icon rules are shared between device UI and WebUI — public channels (`#tag`, default Public, hashtag-joined plain names) no longer show a lock.
- **Mgmt:** Horizontal page swipe on the overview and sub-pages is more reliable.
- **Companion WiFi hardiness:** Bad or oversized companion frames no longer freeze the device; sends continue with partial progress and a short timeout.
- **Companion BLE hardiness:** BLE sync backs off when the queue is full; hosted BLE recovers the link without a full device reboot.
- **Long-uptime timers (ui-modern):** Ping, telemetry, room/repeater timeouts, map frame budget, and similar timers keep working after ~49 days of continuous uptime.
- **WebUI + device UI together:** Browser commands no longer race the on-device UI when both are used at once (fewer freezes/crashes during WebUI use).
- **WebUI firmware upload:** A stalled device loop during upload aborts cleanly within ~10s instead of tripping a network watchdog.
- **Backup state machine:** Backup/restore/delete/repair use one clear operation state; work finishes even if you leave the screen (no endless "Working…"), Back recovers from a stuck Working screen, and local-SD backups list newest-first like remote-SD.
- **Device/WebUI parity:** Hop-count decoding, RX-log payload type labels, battery voltage formatting (two decimals + charging `+`), and map tiles folder normalization now use the same logic on the device screen and in the WebUI.
- **Repeater credentials:** "Save cred." stores repeater login credentials (and mirrors them to SD when SD is primary); saving no longer blocks the UI — primary mirror writes are deferred and flushed in the background.
- **Room/repeater login state parity:** Starting a new room or repeater login fully clears the previous session’s permissions/roles so a failed re-login cannot show leftover access levels (device UI and WebUI).
- **WebUI map updates while loading:** New map markers arriving while the marker list is still paging in are kept instead of dropped.
- **Map info-box tap accuracy:** Taps on the selected-contact info box match the visible box again (hit area had drifted from the painted size).
- **Map "ME" button parity:** Jump-to-me appears in both tile and points map modes; without a GPS fix it shows "No GPS fix".
- **Shared map/route math:** Distance, hop-count, and reply-timeout rules are consistent across map, contact detail, and related screens.
- **T-Display P4 touch/I2C recovery:** After a stuck I2C bus (touch/LoRa control/battery), the device recovers touch and related sensors automatically without endless error spam.
- **WebUI live sync:** New and deleted messages update the list in place when possible; reconnect can skip re-downloading unchanged Contacts/Messages/Channels; gaps recover from recent updates first and fall back to a full refresh only when required.
- **Identity/contacts (SD-primary):** Boot recovers identity/contacts from SD, internal copy, mirror, or latest backup before creating a new identity; node identity is no longer silently replaced by an old key from prefs (#304).
- **Mgmt channel scopes:** Scope popups dismiss cleanly; scope/channel changes no longer stall touch handling and are flushed so they survive reboot (#280 / #279).
- **Reboot latency:** Faster reboot when contacts were not dirty (skips a full rewrite).
- **Transport boot responsiveness (Heltec v4 modern):** UI comes up sooner; WiFi readiness no longer blocks the whole boot setup.
- **Sensor init boot delay (Heltec v4 modern):** Faster boot by probing only known sensor addresses (no full I2C bus sweep).
- **CrowPanel 3.5 compile hardening:** Stable builds across SD/internal storage paths for CrowPanel 3.5.
- **Hashtag/channel ingest:** Channel text from hashtags stays aligned with MeshCore base behavior.
- **Message route parity (DM/channel/WebUI):** Channel zero-hop sends show as **Flood** (not **Direct**); WebUI matches on-device Flood / Direct / hops.
- **WiFi #316:** Station and AP SSID fields now keep punctuation such as `!` (and other printable characters); only auto-generated AP names from the node name are still simplified. Max length 32 characters on the keyboard.
- **Clock #311:** **GPS Time → Sync** sets the clock when GPS has a fix but only sends time (common on T-Deck); date can come from the RTC when GPS date is missing. Retries for 30s; clearer errors when sync cannot run.
- **Contacts #319:** Route on the contact detail screen shows each relay as its own bracket (e.g. `Route: 3 hops [1A71] [213B] [3F0E]`) instead of one long hex string; correct **hop** vs **hops** wording. Hash width (1B/2B/3B) on the list and map follows the heard/stored route.
- **Map #303:** Tapping overlapping or very close map nodes now cycles through them and shows `(n/x)` in the info box.
- **Messages #303 parity:** DM/channel/room sends match companion-app behavior (including flood when the path is unknown); WebUI room send is treated as room traffic.
- **Messages capacity + parity:** Device UI and WebUI share the same composer limits; older DM threads drop least-recent first; Mgmt/WebUI show when message rings are full.
- **Sound #320 (T-Deck Plus):** Fixed false "New DM" alerts by stopping DM tone/haptic on contact-discovery adverts, and removed duplicate channel-message notify calls that could trigger repeated sounds.
- **Sound #305 (T-Deck Plus):** Fixed intermittent partial/missing sound by correcting buzzer playback buffering; tones play cleanly without ticks or gaps.
- **Backup #321 (all SD targets):** Hardened backup finalize so backup metadata completes reliably on both remote-SD and local-SD.
- **WiFi #312:** After wipe/reinstall, WiFi profiles save again (corrupt profile storage is rebuilt automatically).
- **BLE #313:** Tapping **BLE** in the status bar turns Bluetooth off again (same idea as the WiFi badge). Mgmt → BLE has **Restart** and **Radio Off** where supported.
- **WebUI:** Debug/clock JSON page works again (build fix).
- **Build cleanup:** Board-guard cleanup and various build fixes for Heltec V4, Xiao, and non–ui-modern companion targets; removed leftover unreachable UI pieces from earlier refactors (no user-visible behavior change).
- **Clock #283:** **Sync from Msg** updates the RTC from channel and DM timestamps when enabled (with sane limits for bad or very old senders).
- **CrowPanel 7:** BLE/WiFi switch without reboot; fixed boot loop when a phone connects over BLE; ESP32-C6 OTA uses the bundled coprocessor image (update offered when the bundle is stale).
- **CrowPanel 7 map:** Less tile flicker; smoother loading over WiFi while panning or downloading tiles; cache writes no longer blank the screen.
- **WebUI (CrowPanel 7):** Login page loads reliably; hosted WiFi checks no longer block the page for minutes.
- **WebUI (SenseCAP):** Map tiles and contact list work with normal memory; page loads much faster.
- **SenseCAP contacts:** Less list flicker during saves; WiFi moved off the UI core to reduce tearing.
- **Lock screen:** Waking the display no longer shows a black background after timeout.
- **T-Deck map:** Switching tabs is not blocked by long tile cache writes on internal storage.
- **Adverts:** Repeat adverts from known contacts cause less UI lag.
- **Discovered #296:** **Del** removes an entry; tapping the row alone no longer drops it.
- **WebUI #289 / #291:** Contact route badges and **Send DM** behave like on the device.
- **Messages #293:** Copy/paste no longer cuts off at 127 characters.
- **Msgs:** Swipe anywhere in the list to switch Msgs / DMs / Rooms.
- **Mgmt WiFi #285:** Touch targets align with scrolled WiFi rows.

---

## [v0.9.12.3]

### Fixed

- Fixed Mgmt → Backup bottom bar layout and touch.
- Switch between BLE / WiFi working again. BLE was stuck!
- Web UI stops on BLE and starts again on WiFi when enabled (boot and transport switch).
- Status bar: BLE badge no longer triggers WiFi; hit boxes match on-screen badges.
- **Discovered** (Contacts long-press): shows companions (USR) and other advert types, not only repeaters.
- **Discovered**: tap row left of **Add** removes entry; **Add** still adds to contacts; discover row requests all node types.
- **Mgmt / Contacts — Auto Add Types**: type filter works when Auto Add is enabled (was inverted: showed **All**, auto-added every type, **Set** only when disabled).
- Mgmt / Backup; Text-Adjustments
- **NTP Timezone:** accepts short forms (`CET`, `UTC+2`, `UTC-5`, `GMT`, `BST`) — converted to POSIX automatically.

---

## [v0.9.12.2]

### Added

- **Read** / **Newest** on channel and DM transcript screens. Message threads header now has **Read** (mark all in thread), **Newest** (jump to bottom), plus quick-send and compose — easier after reading on the phone app.
- While Wi‑Fi/BLE companion is connected, incoming messages are stored as already read on the device (fewer false unread badges).

### Fixed

- **Channels lost after reboot** when primary storage is the SD card (channels were written to internal flash but loaded from SD).
- **Backup** screen: title centred in header; **New**, **Refresh**, **Restore**, **Delete**, and **Deselect** share one bottom action row (confirm shows only action + **Cancel**).
- **Contact missing** detail view shows a visible **Back** button instead of a blank screen. Stale contact detail auto-closes when the contact was removed while you were on another tab.

---

## [v0.9.12.1]

### Changed / Improved

- Repeater Admin Menu (WebUI): Improved Device, Status, ACL, Neighbors, Regions.
- Mgmt / Global: Firmware update uploader implemented. Take care about the Device-Type.
- **#184** — Light color scheme: clearer outdoor readability across the on-device UI. 
- WebUI WebSocket sync: Contacts and Mgmt / Log now stay subscribed in the browser background after the first full sync, so switching tabs does not repeatedly reload the full contacts list. Log and contacts updates use sequence checks with resync on gaps, and normal contact changes are sent as bounded deltas instead of whole-list broadcasts unless a full resync is required.
- Mgmt / Lock and WebUI: added lock-screen PNG upload to `/MCTerm/lockscreen.png` with a visible `?` limits hint. Uploads accept PNG only, require exact current display dimensions, reject files over 512 KiB, enable the picture background after a successful upload, and persist the selected path/color settings through the existing UI prefs flow.
- Mgmt / Lock: lock-screen background selection is now `None` or a bounded PNG picture path, following the SenseCAP photo-demo model without keeping a large image buffer in RAM. The selected PNG must match the active lock-screen dimensions, is capped at 512 KiB, defaults to `/MCTerm/lockscreen.png`, and persists through Internal and SD prefs with WebUI parity. Lock-screen text color selection remains available (White, Blue, Green, Yellow, Orange, Red). Sample Pictures -> [https://github.com/Seeed-Solution/SenseCAP_Indicator_ESP32/tree/main/examples/photo_demo/spiffs](https://github.com/Seeed-Solution/SenseCAP_Indicator_ESP32/tree/main/examples/photo_demo/spiffs)
- **#22** — SD Card support implemented for Backup/Data Storage (needs to be selected in Mgmt / Global -> Primary Disk).
- **#176** — Store all data primarily on SD Card. Mgmt / Backup
- **#177** — Backup and restore to/from SD Card.
- **#238** — Msgs / DM room separation: split room-server chats out of the normal DM list into a dedicated Rooms frame in both the on-device ui-modern Msgs screen and the WebUI, so user DMs and room-server conversations are no longer mixed together (#238).
- **#252** — Mgmt/Channels/Regions: show a distinct "Max regions reached" popup (instead of the misleading "Invalid or duplicate") when the user attempts to add a region beyond the maximum allowed count, both on-device and in the WebUI (#252). Mgmt/Channels/Regions: raise the maximum number of region scope definitions from 12 to 15 (#252).
- **#125** — Map tab: use each device marker border color for hop count while keeping the marker fill tied to the device type (#125).
- **#199** — Map tab: render 2-byte and 3-byte hash badges on two compact lines so multibyte node markers stay readable without widening as much (#199).
- **#196** — T-Deck Plus sounds: prioritized T-Deck I2S audio refills ahead of WebUI/SD-store work and deferred SD journal flushes until playback ends, eliminating the remaining preview stutter on hardware (#196).
- **#191** — Mgmt/Global: restore the Load key action to the default blue button styling for consistent readability (#191).
- **#192** — Mgmt/Global: make the SD key filename editable for Save/Load from the on-device UI while keeping the current path visible in Identity details and related copy flows (#192).
- Mgmt/Channels: label scope lists **Scopes** (not Regions), without leading hash; message detail uses **Scope**.
- WebUI Repeater Admin: some improvements are done, but WIP!
- Msgs: channel scope, unread, and total shown as list badges.
- Map tiles: Local Tiles and Network Tiles toggles (four combinations, device + WebUI); WebUI device search, map layers, popups, and local tile API.
- Mgmt / DateTime: readjusted Texts!
- Mgmt overview: **UI**, **Light**, **Lock**, **Map**, and **Sound** are separate pages (20 tiles); UI keeps brightness, display timeout, and zoom; Light and Lock hold blink/backlight and autolock settings formerly under UI.
- Mgmt / UI and WebUI: display brightness is a **0–100%** slider (not a 0–255 numeric field).
- Mgmt / Light (T-Deck+): **Keyboard Light** slider (0–100%, default **50%**) is independent of display brightness; persisted in Internal, SD prefs, and backup.
- Mgmt / Light and WebUI: **Blink Bright** uses the same percent slider as display brightness (replaces numeric Set on device).

### Added

- Mgmt/WebUI WiFi **Access Point** mode per **WiFi profile** (MWP2): each profile stores AP/STA mode, STA SSID/password, and AP SSID/password; profile picker shows AP/STA tag; DHCP `192.168.4.1/24`; max 3 AP clients.
- BootConsole: New boot "console style" screen implemented.
- **#264** — implemented Lockscreen Image with selectable color of Text. More to come later!

### Fixed

- SenseCap Indicator TFT WebUI improvements.
- Mgmt / Lock: corrected lock-screen text color interpretation so the `Blue` selection uses the actual blue display color instead of light blue, and added a matching WebUI color swatch/select background for the lock text color picker.
- **#141** — would be fixed with #203, #213.
- **#144** — MeshCore app / direct messages: keep app-side pending delivery waits conservative for ACK-tracked sends so flood-routed DMs are no longer reported failed before their ACK can return (#144).
- **#174** — T-Deck Plus UI blink: when Keyboard Blink or Display Blink is disabled, pending blink state is now cancelled both at notification time and in the runtime blink drivers, preventing random short display/keyboard flashes from queued message activity (#174).
- **#186** — T-Deck Plus keyboard: keep the hardware keyboard in byte-stream mode while draining input, using only idle-time modifier sampling for Alt+S and global Alt+L lock/unlock, restoring responsive backspace/text entry (after 0.9.12 introduced key/event lag) (#186).
- **#187** — Messages/WebUI: derive DM and channel composer counters from MeshCore's 160-byte message limit, the current node-name prefix, and the local mirrored-message buffers so device UI and WebUI match the real send/storage cap (#187).
- **#190** — T-Deck Plus keyboard: only cycle the on-screen SYM picker on a pure SYM tap/release, so held sym compose chords no longer retrigger the picker on each keypress (#190).
- **#194** — Mgmt/UI: remove the stray separator line above the Color Scheme row (#194).
- **#197** — Mgmt/CLI: stop truncating long local command replies in the on-device CLI transcript by wrapping them across multiple output rows (#197).
- **#198** — Mgmt/Channels: restore the visible Default Scope row so channel scope and delete taps target the intended channel instead of the row above (#198).
- **#202** — Messages: force an immediate redraw when entering message detail so hidden detail state cannot keep acting on a stale Msgs list frame (#202).
- **#203** — Clock jumps when receiving room or repeater-admin traffic (duplicate of #213; closed). See **#213**
- **#206** — Ack sound now plays when a pending DM becomes Delivered if its activated in Mgmt / Sound -> All Sounds + Ack.
- **#207** — Contacts/Discovered: unified node-type badge rendering so colors, borders, labels, and sizing match across both screens (#207).
- **#208** — Identity view: fixed public-key detail hold-to-copy so it no longer copies the private key when the public key is selected, and fixed detail scrolling so the SD key file rows remain reachable (#208).
- **#209** — Manual add contact: fixed the name-entry OK flow so a successful add closes cleanly instead of leaving a second OK press to trigger a spurious pubkey-first error (#209).
- **#210** — Mgmt/Global copy flow: fixed stale DM detail state leaking across touch tab switches and hijacking later long-press copy actions on other screens (#210).
- **#211** — Messages/DM detail: fix sent and received direct messages (#211).
- **#213** — Time sync: stop trusting room-server and repeater-admin traffic as RTC authorities, so room/repeater replies can no longer trigger large local clock jumps (#203, narrowed scope, see #213). Time sync (ui-modern): added a Sync from Msg toggle in Mgmt -> Date/Time (default Disabled) that gates whether received message timestamps are allowed to update the on-device RTC at all, and made the NTP Status row report No response whenever the clock is changed by manual edit or by a non-NTP message-driven sync, so the indicator no longer keeps claiming Synced after such overrides. WebUI mirrors the toggle in Settings -> Date/Time (#213).
- **#214** — Direct messages: honor Auto Retry and Auto Reset Path on ACK timeout, including resetting known routed paths before the final retry/failure instead of leaving stale multi-hop routes in place (#214).
- **#215** — In v0.9.12.1 the misplaced allowf/denyf flood controls are moved out of Mgmt/Channels/Regions; Repeater Admin got now the implementation for Regions (#215).
- **#219** — T-Deck Plus: make the trackball click consistently switch the display off from edit fields instead of activating the on-screen key cursor and inserting 1 (#219).
- **#222** — Repeater Admin: keep GUI-generated command replies out of the visible CLI transcript (#222).
- **#230** — Messages: align on-device channel transcript tap/hold hit-testing with the timestamp-sorted rendered rows and unread separator so Quick Reply echoes stay selectable as the visible message instead of opening a different hidden row (#230).
- **#233** — Discovered: shorten the status-bar sort hint so it fits (#233).
- **#242** — Map / advert path cache: receiving a zero-hop advert from a repeater no longer resets its map icon to a 1-byte ID. A zero-hop packet carries no hash-size information; the cached path_len (and therefore the hash badge) is now preserved whenever the existing entry already has hop info, in both the raw-RX and the decoded-advert update paths (#242).
- **#243** — Message detail / Sent timestamp: add a "Sent:" line to the message detail view showing the sender's embedded timestamp alongside the existing "Received:" line (device time when the packet arrived), for both the on-device UI and the WebUI. Allows users to compare the sender's claimed send time against the local receive time and diagnose clock discrepancies (#243).
- **#248** — Room server messages: fix the message body being rendered in the sender-name field when the room server uses signed-plain delivery (i.e., the actual sender was resolved from the signing key prefix) (#248).
- **#251** — Map: stop map badge text from wrapping to the opposite screen edge or overflowing outside the badge border when a node marker is partially scrolled off-screen; badges whose box clips the map viewport boundary are now suppressed cleanly (#251).
- **#256** — Mgmt/Messages quick replies: clearing Custom QuickR1 or Custom QuickR2 now persists across reboot instead of being treated as missing and restored to the factory default; legacy prefs that do not contain quick-reply fields still keep the defaults (#256).
- **#257** — DM and room threads no longer disappear from the Msgs list while the contact still exists (#257).
- Mgmt/Channels: channel scope assignment Mgmt / Channels -> Scopes.
- Mgmt / Log: RX and TX raw packet log (48-entry ring); path/hash metadata on device and WebUI.
- Mgmt / UI (device): brightness slider uses the full row width;
- Mgmt / UI (device): changing display brightness no longer changes T-Deck keyboard backlight; keyboard level follows Keyboard Light on Mgmt / Light only.
- Mgmt / Light and UI (device): percent sliders no longer change while vertically scrolling the list.
- Mgmt / Global Device ID and map hash badges: width follows Path Hash Mode (1/2/3 bytes); remote markers prefer advert path metadata (device + WebUI).

### Questions

- **#205** — Contact Colors: contact and sender name colors are random identity colors, not presence or delivery-state indicators (#205).

---

## [v0.9.12]

## Elecrow CrowPanel 7" companion radio support added!
## Upstream base upgraded: MeshCore v1.14.0 → v1.14.1
## Upstream base upgraded: MeshCore v1.14.1 → v1.15.0
## WebUI interface implemented, WIFI needs to be active (http://IP), usr: admin / pass: BLE-code

### Changed / Improved
- **WebUI message navigation and thread behavior refined**
  - Bottom tab presses now always return to the selected tab's root view, reducing stale nested-state confusion
  - Message list navigation arrows are now visually consistent
  - Channel/DM reply action labels now use "RPL" and signal badges display compact RSSI/SNR formatting
- **WebUI message detail page redesigned for faster diagnostics**
  - Message detail now surfaces key delivery and route context first, including clearer hops vs repeats emphasis based on message direction
  - Metadata and raw transport details remain available in a more structured layout
- **WebUI channel management cleanup**
  - The extra key-format helper line was removed from the Regions section to reduce visual noise
- **On-screen light color scheme readability overhaul**
  - Light mode now uses a bright white base with stronger contrast treatment for controls, improving legibility in heavy sunlight
  - Buttons across the on-device UI were visually unified for outdoor use with clearer borders and higher-contrast label rendering
  - Badge and accent colors were adjusted in light mode so critical status indicators remain easy to distinguish on bright backgrounds
  - Contact list rows were tuned for light mode readability with clearer row separation and easier-to-read text treatment
- **MAP marker and badge consistency improvements**
  - Map device hash badges now follow each device's advertised hash width (1-byte, 2-byte, or 3-byte) instead of the local multi-byte hash preference
  - Map device badge colors now match device type coloring used in contact badges (for example SVR and RPT), improving consistency across the UI
- **Management log readability improved** — long entries wrap to available width
- **More consistent device colors across the interface**
- **Channel and DM views stay responsive while browsing larger histories**
- **CrowPanel 7 list readability, alignment, and text size improved across all screens**
- **Build stability follow-up** — stale platform overrides removed; all targets revalidated
- **Contact route badges correctly decode 1-, 2-, and 3-byte multihash paths**
- **DM route hints now survive device restart**
- **Manual location setting now available in MCTerm via GPS menu**
  - WebUI displays 3-state mode selector: Off (no location), GPS (real-time), Manual (fixed coordinates)
  - Set manual location via text input fields for latitude, longitude, altitude
  - Set location from map view using "Use Map Center" button
  - Clear location via "Clear Loc" button
  - Uses existing firmware protocol (CMD_SET_ADVERT_LATLON) — no companion app protocol changes
  - Matches official companion app behavior for manual location configuration
  - HOWTO: Open Mgmt -> GPS. - Go to Manual Location. - Tap Edit on Manual Lat, enter latitude, confirm. - Tap Edit on Manual Lon, enter longitude, confirm. - Tap Edit on Manual Alt, enter altitude in meters, confirm. - Set Advert Location to Manual (cycle button until it shows Manual), so adverts use your manual coordinates.

### Added
- **Message detail can now jump directly to map route visualization**
  - Selecting the hop count in message detail opens the map and overlays the known hop path as connected lines between devices
- **T-Deck Plus: Alt+S and SYM key now cycle input modes**
  - Pressing Alt+S on the hardware keyboard cycles through all four input modes: uppercase, lowercase, SYM1, and SYM2
  - Pressing the standalone SYM key jumps directly to SYM1; pressing again advances to SYM2 and then back to normal
  - The modifier detection was unreliable due to a hardware quirk; it now reads directly from the keyboard matrix for accurate results
- **T-Deck Plus: SYM1 and SYM2 touch buttons added to the message send bar**
  - While composing a message, two buttons labelled SYM1 and SYM2 are now visible at the bottom of the input panel, allowing direct access to symbol input modes without using the hardware keyboard shortcut
  - In symbol mode the full on-screen symbol picker is shown; the trackball navigates the grid and pressing the trackball button inserts the selected character
  - Alt+S now jumps straight to SYM1 from any normal input mode and stepping it again moves to SYM2 then back
- **WebUI: message hash codes replaced with contact names**
  - Hop-path hash codes (the short bracketed hex codes like [A3], [B2F1] etc.) in channel and DM messages are now automatically replaced by the matching contact name from your contact list, shown in that contact's color
  - If no contact matches a hash code, it is shown dimmed so it remains readable but visually de-emphasised
  - Can be turned off in Mgmt → Messages → Replace HashCodes
  - On-device and WebUI naming behavior now stays aligned through the shared "Replace HashCodes" setting
- **Default flood scope: persistent per-device send scope**
  - A default flood scope can now be stored in device preferences and applied automatically to all outgoing flood messages when no other scope is active
  - Configurable from the browser node settings panel and the command-line interface
- **Map: network tile load reliability improvements for hosted-WiFi targets**
  - Map tile loading is more reliable on devices that use a separate co-processor for WiFi bridging; timeouts are tighter and tile requests no longer collide with background state updates
- **WebUI: full-featured browser interface via WebSocket**
  - Browser-hosted companion UI served directly from the device over WiFi — no app install required
  - All screens (contacts, channels, DMs, map, management, repeater admin) update in real time via WebSocket without page reloads
  - Repeater admin includes live device status readout (battery, uptime, RSSI/SNR, airtime, packet counters) bridged from the binary protocol -> WIP
  - Transient status messages (e.g. sent confirmations, error notices) appear briefly in the header, matching the on-device display behavior
  - Repeater status and clock refresh operations run quietly behind a single progress indicator instead of spamming the channel with individual commands
- **Mgmt/Channels: region scope support with per-channel assignment**
  - Region scope definitions added using MeshCore's flood-scope system, with one scope assignable per channel
- **Mgmt/Channels: new Regions and Scope overlay pickers**
  - The Regions section opens a dedicated overlay listing all defined scopes; each region can be toggled between allow-flood and deny-flood modes
  - Channel rows open a scope picker directly; the currently assigned scope name is shown on the channel row button
- **Mgmt/Date-Time: NTP timezone and server labels fully visible on CrowPanel 7**
- **Mgmt/WiFi: extended network diagnostics and static IP support**
  - WiFi management shows subnet, gateway, and both DNS server addresses; a new mode control switches between DHCP and static addressing
- **GUI: contact list reachability badges condensed**
  - Direct-link badge is now "D" (green); flood-route badge is now "F" (blue)
- **GUI: status-bar clock with quick Date/Time access**
  - A live clock is always shown in the status bar; tapping it jumps to Mgmt → Date/Time
- **GUI: contact list badge reordering and new address-width badge**
  - Badge order from right: address-width, type, route, last-heard, favourite, GPS, unread mail
- **GUI: unread message indicators moved to Msgs tab button**
  - The Msgs tab shows a white envelope for unread channel messages and a red envelope for unread DMs
- **Duty-cycle display shows configured budget instead of lifetime airtime ratio**
- **Msgs: QuickR1 and QuickR2 custom reply presets**
  - Quick reply templates accept both `(VAR)` and `[VAR]` syntax on touch and physical keyboards
  - Supported variables are `HP` (hop path), `HC` (hop count), `SNR` (signal-to-noise ratio), and `RSSI` (received signal strength)
  - Both quick reply slots are editable in Mgmt → Messages; pre-filled with useful defaults on first boot
  - Reply buttons expand hop path, hop count, SNR, and RSSI from the selected message
- **Mgmt/Global: MCTerm firmware information section added**
- **Identity key management added across device UI and WebUI**
  - The management interface now exposes identity details directly on-device, including private-key visibility and guided save/load operations with SD card files
  - Private keys can now be set manually, imported from SD card identity files, and applied without companion app tooling
  - The browser interface now includes a full import/export flow with direct key export, identity bundle export, file-based import, and SD card save/load actions

### Fixed
- **WebUI "Load older messages" indicator now appears only when older pages are actually available**
  - The action no longer appears in states where there is nothing older to fetch, making history availability clearer
- **Message history pruning now retains valid messages when the device clock is far ahead of message timestamps**
  - A faulty or unsynced real-time clock could cause all stored messages to appear expired and get pruned on boot; messages are now preserved whenever the clock has not yet been corrected
- **Message ring counters stay accurate after a history prune pass**
  - Stored message counts shown in the UI and diagnostic views could drift from the actual ring contents after pruning; they now update correctly
- **DM details no longer show fake hop paths for unreachable flood-routed messages**
  - Repeater forwards are now shown in the repeats section, and unknown routes no longer display an invalid high hop count
- **DM delivery status now clearly follows the Mark Delivered Faster setting**
  - If Mark Delivered Faster is enabled, your reported behavior still happens by design.
  - If Mark Delivered Faster is disabled, behavior matches your expected ACK-only Delivered semantics.
- **Room server messages now show the original poster name**
  - Posts relayed through a room server are now attributed to the participant who sent them instead of showing the room server name as the sender
- **WebUI: character counter no longer retains the previous count after a message is sent**
- **WebUI: opening a channel with unread messages now marks them as read immediately**
- **WebUI: management screen fields and dropdowns no longer lose their value mid-edit when a background status update arrives**
- **WebUI: saving a management setting now shows the confirmation status briefly instead of being immediately overwritten by a loading indicator**
- **WebUI: GPS status badge added to the header — shows green when a fix is acquired, gray when searching or disabled, and hidden when GPS is not available on the device; hovering shows the exact GPS status**
- **WebUI: route, signal, and action buttons in message rows now show descriptive tooltips on hover**
- **WebUI: channel view header buttons (back, scope, delete) now show descriptive tooltips on hover**
- **Channel region scope was never actually applied when sending**
  - A protocol framing mistake caused the flood-scope command to silently fail on every attempt; fixed so the scope is reliably applied before each send
- **SD message store: all messages except the last were lost on reboot before compaction**
  - Journal was opened in truncate mode instead of append mode; fixed so all messages accumulate correctly between compaction cycles
- **SD message store: history older than the last ~48 messages was silently discarded on compaction**
  - Idle compaction wrote only the in-memory ring back to the snapshot then cleared the journal, permanently losing older messages regardless of the retention setting; compaction now merges the full journal and existing snapshot so all retained messages survive
- **SD message store: configured message retention days reverted to 15 after every reboot**
  - A byte-order mismatch between the save and load paths caused the setting to be read back from an unrelated field; value now survives reboots correctly
- **SD message store: history could fail to compact after leaving a per-channel filtered view**
  - The pending-compaction flag was cleared when the channel filter blocked compaction; the retry was never attempted after returning to the combined view
- **Region and scope definitions now survive device reboot**
  - Definitions and per-channel assignments were RAM-only; now saved to flash immediately on any change
- **CrowPanel 7 hosted map downloads recover after connection errors**
- **CrowPanel 7 time sync no longer reports false NTP success from the boot clock**
- **Peer and GPS time sync rejects implausible timestamps**
- **Mgmt/WiFi: switching from static IP back to DHCP could keep stale lease values**
- **Mgmt/WiFi profiles now show saved password state before SSID is set**
  - Entering a password before SSID no longer shows "(not set)" in the management overview; both on-device and browser views now show that a password is stored
- **GUI: Date/Time set dialog used wrong timezone**
- **GUI: contact data could become corrupted after malformed updates or bad stored names**
- **Contacts list Last Heard badge now avoids unitless large values**
  - Invalid or ambiguous timestamps that previously showed confusing numbers now display as N/A
- **GUI: long-tap copy in message detail could paste only a partial line**
- **Management log truncation regression fixed**
- **Mgmt/Log timestamps no longer oscillate between adjacent minutes**
  - Log entry timestamps now remain stable once captured instead of flipping between two minute values
- **Repeater login set the repeater clock to a near-zero timestamp**
- **Mgmt/Contacts: AutoAdd Max Hops display was off by one**
- **Map: repeater info popup did not open when tapping markers at higher zoom levels**
  - Marker tap detection now stays reliable across zoom levels, so repeater details open correctly
- **Map marker contact opening no longer depends on tap timing**
  - Tapping a marker shows its info box, and tapping the info box opens contact detail for a more reliable interaction
- **Mgmt/Advert scan: single-attempt scans missed nearby repeaters**
- **Mgmt/Advert Scan Rpts now handles prefix-only repeater matches correctly**
  - Known repeaters discovered by scan now show their saved contact names and open contact detail on row tap
  - When only a short prefix is known and multiple contacts share it, the UI now explains the ambiguity instead of showing a generic key-missing message
  - Repeaters that are not yet in Contacts can now be added directly when a unique full identity can be resolved from recent scan results
- **Device: message read state now saved to storage when scrolling through messages on-device**
  - Previously read state was lost on reboot; now persisted immediately and browser unread counters stay in sync
- **Elecrow CrowPanel 3.5: soft-SPI SD shim build fixed**
- **WebUI: "Jump to latest" button now stays visible while scrolling up in message history**
  - The button was placed at the end of the message list and scrolled out of view when reading older messages; it now sticks to the bottom of the visible conversation area so it is always reachable without scrolling back down first

---

## [v0.9.11]

### Upstream base upgraded: MeshCore v1.13 → v1.14
### Implemented device support for Heltec V4 with TFT +GPS
### Implemented device support for Elecrow CrowPanel 3.5 TFT +SDCard

### unfinished
- MAP is still WIP!

### Added
- Radio activity LED: a small round dot in the top status bar (left of the device name) that flashes green for 250 ms on LoRa RX activity (including CRC-error receptions) and flashes red for 250 ms on LoRa TX activity. The dot shows an idle ring outline when quiet.
- Map last-known-position auto-save: when GPS gets a valid fix, coordinates are persisted to flash immediately so the map restores the true latest position after reboot. Ongoing writes are still throttled (max once every 5 minutes) and only occur when coordinates change.
- Map contact markers now show the node 1-byte ID prefix (2-digit hex, e.g. `1E`) instead of plain squares, using the same existing marker background coloring (hop color / selected highlight) with automatic black-or-white foreground contrast for readability. (still under construction)

### Changed / Improved
- Mgmt → WiFi now supports saved WiFi Profiles: you can create multiple named profiles, keep separate SSID/password pairs per profile, switch the active profile from a dedicated saved-profile picker, delete saved profiles directly in that picker.
- Mgmt → Global is leaner: compile-time rows for `MAX_GROUP_CHANNELS` and `OFFLINE_QUEUE_SIZE` are gone, `MAX_CONTACTS` moved to Mgmt → Contacts as `ACT/MAX Contacts`, and the Global `Reboot` / `Update & Upgrade` actions now use the compact side-by-side button style.
- Mgmt → Contacts now includes `Manual add contact`, using the existing edit overlay for a two-step `Public Key` then `Name` flow with clipboard paste support in both fields.
- MAP tile cache handling is quieter and more efficient: missing SD tiles are negatively cached briefly, repeated open attempts are avoided while offline, and tiles are retried automatically once Wi-Fi connectivity returns.
- MAP marker tap behavior: tapping a ByteID marker now keeps selection on the map and shows a left-bottom info box with `Name`, `Age`, and `Distance` (km), without a separate floating name box.
- MAP marker double-tap behavior: double-tapping the same ByteID marker opens that contact directly in Contact Detail.
- Mgmt → Advert → Scan Neighbor Repeaters now shows an Add button only for repeaters not yet in Contacts, and tapping a repeater row opens Contact Detail instead of adding on name/row tap.
- Mgmt → Advert now includes `Path Hash Mode`, and Mgmt → Contacts now includes `AutoAdd Max Hops`, matching the MeshCore v1.14 companion settings on-device. (be careful other repeaters/companions need minumum MeshCore v1.14+ to avoid compatibility issues when these settings are changed)
- Mgmt → Contacts → `AutoAdd Max Hops` now opens an Edit field for manual numeric entry with validation across the full MeshCore v1.14 range `0..64`: `0 = No Limit`, `1 = Direct (0 hops)`, and `N = up to N-1 hops`.
- DM message detail now shows delivery state for sent messages (Pending / Delivered / Not delivered), including ACK-based delivered updates.
- Mgmt → Channels now treats the built-in `Public` channel like any other joined channel, including Share/Delete actions.
- If the default `Public` channel was deleted, Mgmt → Channels → Join now shows a `Public` button that recreates it automatically only when it is currently missing.
- T-Deck Text editor now scrolls with the cursor when moving left into long input, so the beginning of the text remains visible while editing.
- Text edit overlay uses smaller message-input text with wrapped visible lines for easier long-message editing.
- Touch text-edit overlays now use a tighter full-screen keyboard layout with larger key labels, smaller action-bar labels, and consistent title/field sizing across Wi-Fi, BLE, repeater, and other edit targets.
- The on-screen keyboard now supports two symbol pages (`SYM1` / `SYM2`) and horizontal swipe gestures across the key area to switch symbol pages faster on touch-only devices.

### Fixed
- Elecrow CrowPanel 3.5 SD card initialization now uses the dedicated TF SPI wiring and shared mount helper, so `/MCTerm` prefs, clipboard storage, and map tile cache writes work on the board.
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
- Heltec V4 TFT modern GPS bring-up now handles Quectel L76K modules correctly.
- UI-modern companion radios with onboard GPS now keep the fast boot path while restoring full module-specific GPS init: Heltec V4 TFT preserves persistent L76K warm-start state and RTC sync, and LilyGo T-Deck Plus again applies the richer u-blox UBX runtime configuration even with GPS detect skipped.
- Scroll gestures are now confined to the actual scrollable viewport across Msgs, DMs, contact detail, room console, repeater admin, and Mgmt pages with fixed headers or action rows, and top-edge list drag now clamps immediately so screens cannot be pulled into blank overscroll from buttons, title bars, or the first row.
- Mgmt → Advert action buttons (Advert Direct / Advert Flood / Scan Rpts) are now rendered as three large side-by-side tiles matching the Mgmt overview style — no white border, easier to tap on touch screens.
- Every button press across the entire GUI now shows an instant yellow ring at the touch point for 200 ms, giving clear visual confirmation that (where) the tap was registered.
- Mgmt → GPS now includes a yellow Tracking section with flood-advert movement tracking, a persisted meter threshold, a lock-screen Tracking badge, and an orange tracking icon in the top status bar while active.

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
