# HP Tester BLE — Native Android

Aplikasi native Android untuk monitor arus & tegangan HP via boot cable (ESP32 + INA226).

Port dari web app `hp_tester_ble_v5.html` ke Kotlin + Jetpack Compose.

## Fitur

- Koneksi BLE native (lebih stabil daripada Web Bluetooth)
- Live monitoring: tegangan, arus, daya, puncak
- Grafik real-time arus & tegangan
- Mode Diagnosa dengan 12 kandidat (short, bootloop, stuck bootloader/kernel, dll)
- Rekam baseline HP normal → bandingkan saat diagnosa
- Kompensasi drop tegangan karena resistansi kabel
- Dukungan berbagai keluarga chipset (Qualcomm, MediaTek, Exynos, Unisoc, Apple, dll)

## Hardware

- ESP32 + INA226 (firmware: `hp_tester_ble (1).ino`)
- BLE Service UUID: `6e400001-b5a3-f393-e0a9-e50e24dcca9e`
- Characteristic UUID: `6e400003-b5a3-f393-e0a9-e50e24dcca9e`
- Nama perangkat: `HP-Tester`

## Build dengan GitHub Actions

1. Push repo ini ke GitHub
2. Buka tab **Actions** → workflow **Build APK**
3. Setelah selesai, download artifact:
   - `HPTesterBLE-debug` → APK debug (langsung bisa install)
   - `HPTesterBLE-release` → APK release (unsigned)

Atau trigger manual: Actions → Build APK → Run workflow.

## Build lokal

```bash
# Butuh Android SDK + JDK 17
./gradlew assembleDebug
# APK di: app/build/outputs/apk/debug/
```

## Permission

- Android 12+: `BLUETOOTH_SCAN` + `BLUETOOTH_CONNECT`
- Android 11 ke bawah: `ACCESS_FINE_LOCATION` (diperlukan untuk scan BLE)

## Struktur

```
app/src/main/java/com/hptester/ble/
├── MainActivity.kt      # UI Compose
├── AppViewModel.kt      # State + recording logic
├── BleManager.kt        # BLE scan / connect / notify
└── DiagnosisEngine.kt   # Port logika diagnosa dari web app
```

## Catatan

- Min SDK 26 (Android 8.0)
- Target SDK 35
- Hanya portrait
- Data baseline saat ini disimpan di memori (akan hilang saat app ditutup). Bisa ditambah DataStore/Room jika diperlukan.
