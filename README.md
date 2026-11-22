# TSS-3 Recorder for Android

Records Yost Labs TSS-3 (3-Space) sensors over Bluetooth on an Android phone. Each sensor records
to its own storage, so a weak or busy Bluetooth link cannot lose samples. After **Stop**, the app
downloads every recording, checks it sample by sample, and saves CSV files that all start and end
together.

## Download

**[⬇ Download TSS-3_Recorder.apk](https://github.com/covdetbaba/tss3-recorder-android/releases/latest/download/TSS-3_Recorder.apk)** (2.1 MB, version 1.0)

Open this page on the phone and tap the link above. It always gives the newest version.

Or scan this with the phone camera:

<img src="docs/download-qr.png" width="220" alt="QR code for the download link">
All versions: [Releases](https://github.com/covdetbaba/tss3-recorder-android/releases).

## Requirements

- Android 10 or later, with Bluetooth LE. Tested on a Samsung Galaxy A26 5G (Android 16).
- TSS-3 sensors with onboard storage, advertising as `YL-TSS-...`.

## Install

1. Tap the download link above on the phone and open the downloaded file.
2. If Android asks, allow **Install unknown apps** for your browser or file manager. This
   happens once.
3. If Google Play Protect says the app is unknown, tap **More details** and then
   **Install anyway**. The app is not from the Play Store, so Play Protect has not seen it before.
4. Open **TSS-3 Recorder** and allow **Nearby devices** when it asks. That permission is what
   lets it find the sensors.

To update, download and install the new version over the old one. Your recordings stay.

## Record

1. Switch the sensors on near the phone and tap **Scan**. It takes about 8 seconds.
   Untick any sensor you do not want. An orange signal value (-88 dBm or weaker) marks a weak link.
2. Optional: tap **Check sensors** to connect to each sensor once and show its battery.
3. Tap **Start**. When the status line says **Recording**, all sensors are recording.
4. Tap **Stop**. The status line shows **Downloading NN%** until the files are saved.
   Keep the app open and the phone near the sensors until then. The screen stays on.
5. Tap **Share last recording** to send the files by email, Drive, and so on.

<img src="docs/screenshot.png" width="300" alt="The app after a recording">

## Files

Recordings are saved in **My Files > Documents > TSS_logs**:

```
Documents/TSS_logs/<date_time>/<sensor>/<sensor>.csv
Documents/TSS_logs/<date_time>/log.txt
```

The CSV columns are `Time (s), time64, qx, qy, qz, qw, cAx, cAy, cAz, cGx, cGy, cGz`:
time in seconds and in microseconds on the shared clock, then tared orientation (quaternion),
primary corrected accelerometer and primary corrected gyroscope. These are the same files the
fixed desktop TSS-3 Suite writes. Every sensor's clock is set to one shared time at Start, and
all files are cut to the stretch every sensor recorded, so they have the same number of rows.

## Settings on the screen

- **Rate (Hz):** 100 by default.
- **Max links:** how many sensors the phone talks to at once (default 7). Phones can only keep
  a handful of Bluetooth connections open. With more sensors than this, the app starts them in
  groups and lets them go while they record, then reconnects each one at Stop. Lower the number
  if a phone refuses connections.

The app puts each sensor's own log settings back afterwards. It saves nothing to the sensor's
flash. Each recording also stays on its sensor (`/DATA`), as it does with the desktop suite.

## Measured

| Test (Galaxy A26 5G) | Result |
|---|---|
| 13 sensors, 60 s at 100 Hz, 7 links at a time | 13 files of 6,200 rows each, starts and ends within 10 ms, no missing samples |
| 3 sensors, 60 s, 2 links at a time (groups) | 3 files of 6,230 rows each, within 4 ms |
| Download speed | up to 11 kB/s per sensor; 13 sensors × 60 s saved 3 minutes after Stop |

## Troubleshooting

- **No sensors found.** Check that Bluetooth is on and that the app has the *Nearby devices*
  permission (Settings > Apps > TSS-3 Recorder > Permissions). Make sure the sensors are switched on.
- **"did not connect on the first try".** Android sometimes refuses a connection (status 133).
  The app retries up to 6 times on its own, and this message is only information.
- **A sensor is left out at Start.** It could not be reached. Charge it, bring it closer, or
  lower *Max links*. The other sensors still record.
- **Slow download.** The sensor with the weakest signal sets the pace. Once you tap Stop, the data
  is already on the sensors, so bringing them closer to the phone speeds up the download.

## Verify the file (optional)

SHA-256 of `TSS-3_Recorder.apk` 1.0:
`aa388c6bda0c731424711bbb946fff41a9cd26dec999a5e4e6ad89e33f408279`

---

This app is independent and is not made by or affiliated with Yost Labs. TSS-3, 3-Space and
YL-TSS are names of Yost Labs products and are used here only to say which sensors the app works
with. It is provided as is, without warranty.
