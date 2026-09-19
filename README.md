# OVERTUNE

**PC Performance & Optimization CLI** — aplikasi console (.NET 8) untuk membersihkan, memantau, dan mengoptimasi performa Windows, dengan fokus khusus ke gaming/performa maksimal.

![Platform](https://img.shields.io/badge/platform-Windows-blue)
![.NET](https://img.shields.io/badge/.NET-8.0-blue)
![License](https://img.shields.io/badge/license-Personal%20Use-lightgrey)

---

## ✨ Fitur

### Pembersihan & Disk
- **Bersihkan Temporary Files** — hapus cache di `%TEMP%`, `Windows\Temp`, `Prefetch`
- **Kosongkan Recycle Bin** — via `shell32.dll`
- **Bersihkan Cache Windows Update** — kosongkan `SoftwareDistribution\Download`
- **Bersihkan Cache Browser** — Chrome & Edge
- **Analisis Ukuran Folder (Disk Usage)** — cari subfolder terbesar
- **Uninstall Program** — daftar & jalankan uninstaller resmi aplikasi terinstal

### Performa & Tuning
- **Trim RAM** — paksa semua proses melepas memori idle (`EmptyWorkingSet`)
- **Optimasi Visual Effects** — toggle animasi/transparansi Windows untuk performa
- **Power Plan Switcher** — ganti Power Saver / Balanced / High Performance / **Ultimate Performance** (unlock otomatis)
- **FPS Booster** — matikan Game DVR, aktifkan Game Mode & Hardware GPU Scheduling
- **Game Mode / Priority Booster** — atur priority proses (Low/Normal/High) per aplikasi
- **Scan & Kill Proses Berat** — cari & hentikan proses paling boros RAM/CPU (dengan proteksi proses sistem)
- **Prefetch & Superfetch Cleaner + Tuner** — bersihkan cache Prefetch, toggle service SysMain, rekomendasi otomatis berdasar SSD/HDD
- **Kelola Background Apps** — matikan aplikasi UWP/Store yang jalan diam-diam di background
- **CPU/RAM Monitor Real-time** — dashboard live usage dengan grafik bar & top proses

### Sistem & Jaringan
- **Info Sistem** — CPU, RAM, GPU, motherboard, BIOS, uptime, semua drive
- **Kelola Startup Programs** — enable/disable aplikasi yang jalan otomatis saat boot
- **DNS Flush & Network Reset** — flush DNS, release/renew IP, reset Winsock & TCP/IP stack
- **Network Speed & Ping Checker** — tes ping ke server publik + speed test download
- **Cek Kesehatan Baterai** — wear level baterai laptop (kapasitas desain vs aktual)
- **Buat System Restore Point** — snapshot sistem sebelum perubahan besar

---

## 🖥️ Requirements

- **Windows 10/11** (sebagian fitur seperti Ultimate Performance butuh edisi Pro/Enterprise/Workstation)
- **Visual Studio 2022** (17.8+) dengan workload **.NET desktop development**
- **.NET 8 SDK**
- Koneksi internet (untuk restore NuGet package saat build pertama kali)

---

## 🚀 Cara Menjalankan

1. Clone/download project ini
2. Buka `PCOptimizer.csproj` di Visual Studio
3. Tunggu NuGet package selesai di-restore otomatis
4. Tekan **F5** (debug) atau **Ctrl+F5** (tanpa debugger)

> Aplikasi otomatis meminta hak **Administrator** (UAC prompt) setiap dijalankan — ini diperlukan karena sebagian besar fitur (bersihkan Windows\Temp, ubah service, ubah registry HKLM, dll) butuh akses admin.

### Build via command line

```bash
dotnet build
dotnet run
```

---

## 📁 Struktur Project

PCOptimizer/
├── Program.cs # Menu CLI utama & orkestrasi semua fitur
├── app.manifest # Manifest UAC (requireAdministrator)
├── PCOptimizer.csproj # Konfigurasi project & NuGet packages
├── Services/
│ ├── TempFileCleaner.cs # Bersihkan temp files
│ ├── RecycleBinCleaner.cs # Kosongkan recycle bin (P/Invoke shell32)
│ ├── WindowsUpdateCleaner.cs # Bersihkan cache Windows Update
│ ├── BrowserCacheCleaner.cs # Bersihkan cache Chrome/Edge
│ ├── DiskAnalyzer.cs # Analisis ukuran folder
│ ├── SystemInfoService.cs # Info sistem lengkap (CPU/RAM/GPU/dll)
│ ├── StartupManager.cs # Kelola startup programs (registry)
│ ├── RamTrimmer.cs # Trim RAM (P/Invoke psapi.dll)
│ ├── BatteryHealthService.cs # Cek kesehatan baterai (WMI)
│ ├── VisualEffectsService.cs # Toggle visual effects Windows
│ ├── NetworkResetService.cs # DNS flush & network reset
│ ├── NetworkSpeedService.cs # Ping & speed test
│ ├── PowerPlanService.cs # Power plan switcher + Ultimate Performance
│ ├── UninstallManager.cs # Uninstall program terinstal
│ ├── PriorityBoosterService.cs # Atur priority proses
│ ├── BackgroundAppsService.cs # Kelola background apps UWP
│ ├── PrefetchSuperfetchService.cs # Prefetch cleaner & SysMain tuner
│ ├── RestorePointService.cs # Buat system restore point
│ ├── ProcessScannerService.cs # Scan & kill proses berat
│ ├── FpsBoosterService.cs # FPS booster (Game DVR/Mode/GPU Scheduling)
│ └── PerformanceMonitorService.cs # CPU/RAM monitor real-time
└── Utils/
├── ConsoleHelper.cs # Helper tampilan console (warna, prompt)
├── ByteSizeFormatter.cs # Format byte → KB/MB/GB
└── ConsoleProgressBar.cs # Progress bar dengan persentase


---

## ⚠️ Catatan Penting

- **Butuh Administrator**: aplikasi selalu minta elevasi UAC lewat `app.manifest`. Beberapa fitur (Uninstall Program, Scan & Kill Proses) tetap aman dipakai walau bukan admin, tapi sebagian besar fitur lain memang membutuhkannya.
- **Ultimate Performance** hanya tersedia di Windows Pro/Enterprise/Workstation — tidak muncul di edisi Home (batasan resmi Microsoft).
- **Trim RAM** dan **Scan & Kill Proses** punya proteksi terhadap proses sistem kritikal (`System`, `csrss`, `lsass`, `explorer`, dll) — tidak bisa di-kill lewat aplikasi ini demi mencegah crash sistem.
- **System Restore Point** dibatasi Windows maksimal 1x per 24 jam secara default.
- Tidak ada dependency eksternal berbahaya — semua fitur pakai WinAPI (P/Invoke) dan WMI bawaan Windows, tidak ada koneksi ke server pihak ketiga kecuali fitur **Network Speed Test** (mengunduh file test dari `thinkbroadband.com`).

---

## 📦 NuGet Dependencies

| Package | Kegunaan |
|---|---|
| `Microsoft.Win32.Registry` | Akses registry (startup, visual effects, background apps) |
| `System.Management` | WMI query (CPU/GPU info, battery, drive media type) |
| `System.ServiceProcess.ServiceController` | Kontrol Windows Service (SysMain) |
| `System.Diagnostics.PerformanceCounter` | CPU usage real-time monitor |

---

## 🛣️ Ide Pengembangan Lanjutan

- Page File (Virtual Memory) Optimizer
- Disk Health Checker (SMART status)
- Export laporan optimasi ke file `.txt`/`.pdf`
- Auto-clean terjadwal via Task Scheduler
- Custom icon `.exe`

---

## 📝 Lisensi

Project personal — dibuat untuk penggunaan pribadi.
