# Software Version History — ATS Triton Receiver Interface

**Current release:** v1.28 (August 2026)

**Last updated:** October 2026

The Triton Receiver Interface is the Windows application used to configure ATS
SR3xxx acoustic receivers, record detections in real time, download files from
the receiver's SD card, filter detection data, and flash receiver firmware.

This document lists what changed in each release. Changes to the detection
filters are summarized here and described in full in the companion document,
**Detection Filter History**.

> **A note on version numbers.** Releases use a two-digit minor number: v1.16
> is "one point sixteen", v1.28 is "one point twenty-eight". There has never
> been a v1.6 or v1.06.

---

## v1.28 — August 2026

- Fixed a USB driver check that showed a "contact ATS" message during
  installation even when the driver had installed correctly.
- Renamed the firmware download list's "Last Modified" column to
  **Release date**.
- No changes to filter behavior.

## v1.27 — June–July 2026

A correctness release. Several changes fix real data and communication errors.
**We recommend all users update to v1.27 or later.**

**Detection filters**

- Corrected timestamp reconstruction in files containing more than one receiver
  time marker, in files recorded across a receiver restart, and in files where
  the SD card was pulled mid-deployment. Earlier versions could place
  reconstructed timestamps days to months away from the true time. If you
  produced corrected timestamps with v1.23–v1.26, reprocess those files.
- Fixed corrected dates printing with the year 0025.
- Added `Delta_Value` and `Internal_Value` diagnostic columns so you can see
  what the receiver logged next to what the filter reconstructed.
- Reorganized the Filter Options dialog into "Filter Methods" and
  "Data Quality / Verification", and relabeled the checkboxes. Behavior was
  unchanged by the renaming — see the Detection Filter History for the old and
  new names.

**Receiver communication**

- **Set Time** now also writes the receiver's timezone to match the time being
  set. Previously the clock and the timezone could disagree.
- Fixed GPS On/Off leaving the receiver half-on. The receiver has two separate
  GPS systems — the modem and sentence printing — and the buttons now track
  both.
- Fixed commands colliding in the receiver's buffer immediately after
  connecting, which could leave configuration fields blank.
- USB commands are now null-terminated, fixing occasional unreliable command
  delivery.

**Other**

- Added an **Auto** checkbox for USB connection, so automatic connection can be
  turned on and off instead of happening once per session. Automatic connection
  is suppressed during firmware flashing.
- The firmware download list now shows each file's release date.
- Firmware can be flashed without a connected receiver (settings validation is
  skipped and defaults are used).
- Fixed "Access is denied" errors writing to `Generated_Files` when the
  application was launched from a shortcut or an installed location.

## v1.26 — May 2026

- Firmware download list reorganized into a grouped, sorted tree, with the most
  recent firmware in each folder marked. Legacy firmware folders start
  collapsed so they are only opened deliberately.
- Added a **Release Notes** button to the firmware download dialog, which opens
  the firmware version tracking document.
- Fixed firmware download failing on networks that inspect secure traffic
  (corporate proxy, VPN, or antivirus). Network errors now explain the actual
  cause — no connection, proxy, firewall, or GitHub rate limit — instead of
  always reporting "no internet".
- Added an **Auto** checkbox beside the Configuration tab's receiver time field
  that keeps the displayed receiver clock ticking once per second.
- Firmware flashing reliability: firmware generation is now detected and
  validated before any dialogs appear, so you are told what was found before
  choosing a file, rather than hitting an error after selecting one. Fixed
  several cases where flashing failed or crashed on both current and legacy
  firmware.

## v1.25 — May 2026

- **Filtering extended to real-time logs** recorded by the interface, not just
  files downloaded from the SD card.
- Added the **Flag Out-of-Order Rows** filter.
- Added **Save Filtered Detections**: during a live session, a second CSV is
  written containing only detections matching your active filter.
- Downloaded SD card files are now saved exactly as the receiver wrote them,
  preserving the original headers and structure.
- Remote (TRC) downloads: added **Download all new files**, which queues every
  completed file not yet saved locally and downloads them oldest-first; added
  **Sync Files** / **Stop Syncing**; added per-station **Hourly** and **Daily**
  schedule checkboxes and a configurable gap between downloads. Unattended
  downloads skip files already saved, and now correctly target the most
  recently *completed* file rather than the file still being written.
- Added the **Download Firmwares** button on the Configuration tab, which
  fetches firmware directly from the ATS public repository.
- Added a progress indicator during file transfers, a resizable About window
  with adjustable text size, and tooltips on every filter, graph, and file menu
  option.

## v1.24 — March 2026

- Remote (TRC) command delivery reworked for reliability through the cellular
  modems used in field deployments.
- Cleaned up the remote file listing so SD card directory entries display
  correctly.

## v1.23 — February 2026

- **Pressure sensor tag decoding finished** and usable on acoustically coded
  pressure tags.
- **Added time verification**, which cross-checks receiver timestamps against
  the receiver's internal counter and can optionally correct them.
- Time verification can be run on its own, without the code or false-positive
  filters.

## v1.22 — November 2025

- Remote (TRC) file download completed for the most recent completed file.
- Added a diagnostic log file to help ATS troubleshoot reported issues.

## v1.21 — November 2025

First production-ready release of the remote (TRC) tab, now visible to all
users.

- Real-time detection logging from remote stations to a per-station CSV named
  by site and serial number.
- Fixed command reliability across different modem types.
- The detection table shows local time, sorts by detection count, and trims
  itself to stay responsive during long sessions.
- Log files roll over to a new file at 1 GB.

## v1.18 — October 2025

- Firmware flashing cleaned up: clearer errors and warnings during a flash, and
  more reliable reconnection afterwards.
- The application now connects to an attached USB receiver on startup and pulls
  its configuration automatically.
- Added a visual indicator showing when the receiver is processing a command.
- Receiver settings and calibration offsets are restored after flashing newer
  firmware over legacy firmware.

## v1.16 — October 2025

First release with post-processing filters.

- Added the **Code** filter and the **False Positive** (PRI) filter.
- Added the default `filter_output` folder.
- Added an interactive map of recorded GPS fixes.
- The USB driver installer is bundled with the application, and the correct
  driver is checked for on startup.
- Executables are digitally signed.

## April–November 2025 — Initial development

First releases of the interface: USB and RS232 connection to a receiver, the
live detection table with tag code summarizing, the command terminal, GPS fix
parsing, SD card file download, firmware flashing, and the remote (TRC) tab for
managing field-deployed receivers over cellular or Wi-Fi.

---

## Questions

Contact Advanced Telemetry Systems — <https://atstrack.com>

*Advanced Telemetry Systems, Isanti, Minnesota.*
