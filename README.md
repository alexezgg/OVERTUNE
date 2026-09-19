'''
      ::::::::  :::     ::: :::::::::: ::::::::: ::::::::::: :::    ::: ::::    ::: :::::::::: 
    :+:    :+: :+:     :+: :+:        :+:    :+:    :+:     :+:    :+: :+:+:   :+: :+:         
   +:+    +:+ +:+     +:+ +:+        +:+    +:+    +:+     +:+    +:+ :+:+:+  +:+ +:+          
  +#+    +:+ +#+     +:+ +#++:++#   +#++:++#:     +#+     +#+    +:+ +#+ +:+ +#+ +#++:++#      
 +#+    +#+  +#+   +#+  +#+        +#+    +#+    +#+     +#+    +#+ +#+  +#+#+# +#+            
#+#    #+#   #+#+#+#   #+#        #+#    #+#    #+#     #+#    #+# #+#   #+#+# #+#             
########      ###     ########## ###    ###    ###      ########  ###    #### ##########       
'''

**PC Performance & Optimization CLI** untuk Windows — bersihkan, pantau, dan optimasi performa PC langsung dari terminal.

## Fitur

- Bersihkan temp files, cache browser, cache Windows Update, Recycle Bin
- Trim RAM & scan/kill proses berat
- Power Plan Switcher (termasuk Ultimate Performance)
- FPS Booster untuk gaming
- Kelola startup programs & background apps
- Info sistem lengkap (CPU/RAM/GPU/disk)
- Network tools (DNS flush, ping test, speed test)
- Uninstall program, buat System Restore Point, dan lainnya

## Requirements

- Windows 10/11
- Visual Studio 2022 + .NET 8 SDK

## Cara Menjalankan

1.Buka aplikasi nya doang wok

Aplikasi otomatis minta akses **Administrator** karena sebagian besar fitur butuh itu.

```bash
dotnet build
dotnet run
```

## Catatan

- Beberapa fitur (Ultimate Performance) hanya tersedia di Windows Pro/Enterprise
- Proses sistem kritikal (explorer, lsass, dll) diproteksi dari fitur kill process
- Semua fitur pakai WinAPI/WMI bawaan Windows, tanpa koneksi server pihak ketiga (kecuali speed test)
