# Detection Filter History — ATS Triton Receiver Interface

**Applies to:** Triton Receiver Interface v1.16 through v1.27

**Last updated:** October 2026

This document describes the post-processing filters in the Triton Receiver
Interface and records how they have changed from version to version.

If you are reprocessing older data, or comparing results produced by two
different versions of the software, use the version table in Part 2 to see
whether a filter changed in between.

---

## Part 1 — What each filter does

The filters are reached through **File → Filter Files** (Ctrl+F), which opens
the **Filter Options** dialog. Every option in that dialog has a tooltip:
hover your mouse over a checkbox or field to see what it does.

The dialog is divided into two groups.

### Filter Methods — choose which detections to keep

| Filter | Default | What it does |
|---|---|---|
| **False Positive** | On | Removes multipath echoes and spurious detections by checking the interval between detections against the tag's pulse repetition interval (PRI). A detection is kept when it falls inside the expected PRI window. |
| **Code** | Off | Keeps only detections whose tag code appears in the code list file you select. Use this to pull your own study tags out of a file that also contains other projects' tags. |
| **Press Sensor Decoding** | Off | Decodes pressure (depth) sensor tags and keeps only readings inside the depth range you specify. Cannot be combined with Temp/Press Sensor Decoding — the two target different tag types. |
| **Temp/Press Sensor Decoding** | Off | Decodes three-pulse tags that report both temperature and pressure, and keeps only readings inside both the temperature and the depth range you specify. |

### Data Quality / Verification — check the data file

These options inspect the file rather than deciding which fish detections to
keep. They are useful when a receiver's clock is suspect.

| Check | Default | What it does |
|---|---|---|
| **Verify Tag Times** | Off | Diagnostic only. Cross-checks each timestamp against the receiver's internal DSP counter and adds an `Adjusted_Date_Time` column alongside the original, so you can compare the two. Your data is not altered. |
| **Correct Tag Times** | Off | Replaces the `DateTime` column with the reconstructed timestamp before the other filters run. Requires Verify Tag Times to be enabled. Use this when a deployment's clock is known to be wrong. |
| **Flag Out-of-Order Rows** | Off | Reports rows where the receiver's internal counter runs backwards, which indicates detections logged out of chronological order. GPS rows are excluded from this check. |

### Output options

The same dialog controls which data columns are carried into the filtered
output (tilt, pressure, temperature, voltage, signal strength and others), and
whether a summary file and pressure graph are produced alongside the filtered
CSV.

Filtered output is written to the `filter_output` folder unless you choose
another location.

### Supported input files

Filters accept both file types the system produces:

- Detection logs downloaded from the receiver's SD card.
- Real-time logs recorded by the interface during a live session
  (`..._dat.csv`).

---

## Part 2 — What changed, by version

> **A note on version numbers.** Releases use a two-digit minor number:
> v1.16 is "one point sixteen", v1.27 is "one point twenty-seven". There has
> never been a v1.6 or v1.06.

### v1.16 — October 2025

First release with filtering.

- Added the **Code** filter (filter a detection file against a tag code list).
- Added the **False Positive** filter with a configurable PRI window.
- Added the default `filter_output` folder for filtered results.
- Pressure sensor tag decoding started but was not yet usable.

### v1.23 — February 2026

- **Press Sensor Decoding finished** and usable on real acoustically coded
  pressure tags.
- **Added time verification.** Receiver timestamps could occasionally be logged
  out of sequence or at implausible values; this check reconstructs the
  expected time from the receiver's internal counter so the two can be
  compared.
- Time verification can be run on its own, without also applying the Code or
  False Positive filters.

### v1.25 — May 2026

- **Filtering extended to real-time logs.** Logs recorded live by the interface
  (`..._dat.csv`) can now be filtered, not only files downloaded from the SD
  card.
- Added **Flag Out-of-Order Rows**.
- Added **Save Filtered Detections**, which writes a second live CSV during a
  real-time session containing only detections matching your active filter.

### v1.27 — June–July 2026

A correctness release. Several of these were genuine data errors, not cosmetic
changes. **If you produced corrected timestamps with v1.23–v1.26, reprocess
those files with v1.27.**

- **Files with more than one time marker were reconstructed incorrectly.** In a
  file containing several receiver time markers, the software could attach
  every detection to a single reference time, placing reconstructed timestamps
  days to months away from the true time. Files with one marker were
  unaffected, which is why this went unnoticed. Timestamps are now anchored to
  the correct marker.
- **Receiver restarts were mistaken for counter rollovers.** The internal
  counter can run backwards for two reasons: it wrapped around its maximum, or
  the receiver restarted it. Treating a restart as a wrap added a spurious jump
  of roughly eleven days. The two cases are now told apart.
- **Pulling the SD card mid-deployment shifted later data backwards.** Data
  recorded after a log resume could be pinned to a stale reference time — in
  one case data recorded on 28 January was reported near 8 December. Fixed.
- **Corrected dates could print with the year 0025.** A two-digit year was
  re-read as a four-digit year in the output column. Filtering decisions were
  unaffected; only the printed date was wrong.
- **Filter Options dialog regrouped and relabeled.** The checkboxes were split
  into "Filter Methods" and "Data Quality / Verification" so it is clear which
  options discard detections and which only inspect the file. Wording was made
  tag-centric. Behavior was unchanged by the renaming.

  | Old label (v1.26 and earlier) | New label (v1.27 and later) |
  |---|---|
  | Apply Filter | False Positive |
  | Apply Code Filter | Code |
  | Apply Press Sensor Filter | Press Sensor Decoding |
  | Verify Detection Times | Verify Tag Times |
  | Run Through filters after verifying | Correct Tag Times |

- Added the **About** button in the Filter Options dialog.

---

## Questions

Contact Advanced Telemetry Systems — <https://atstrack.com>

*Advanced Telemetry Systems, Isanti, Minnesota.*
