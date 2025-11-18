# WindowsBestPractices

[en](/README.md), [fr](/README-FR.md)

Here are simple and safe tweaks for a computer running Windows 10 or 11. They help make your PC more responsive for office work and gaming. This guide is effective for fixing bugs, slowdowns, and crashes. These practices are not "magic"—I don’t promise incredible performance gains; the best way to improve performance is still to buy more powerful components.

*Although this guide is low-risk, read everything carefully before doing anything.*

## 📖 Table of Contents
- [Quick Practices](#quick-practices)
- [Advanced Practices](#advanced-practices)
- [Tips](#tips)
- [Conclusion](#conclusion)
- [Sources](#sources)

## 🧹Quick Practices
In order, repeat about once a month:

* Update your graphics drivers from [Nvidia](https://www.nvidia.com/Download/index.aspx) or [AMD](https://www.amd.com/en/support). Use [DDU](https://www.guru3d.com/files-details/display-driver-uninstaller-download.html) to cleanly remove old drivers
> [!IMPORTANT]
> Using DDU is **essential** as it fixes crashes and performance drops in games

* Update Windows via Windows Update in Settings

Once everything is up to date and the computer has been restarted:

* Repair system files: `sfc /scannow`
* Repair Windows image: `Dism /Online /Cleanup-Image /RestoreHealth`
* If you have white icons on the desktop, reset the icon cache with my [script](https://github.com/seguinleo/ResetIconCache)
* If you encounter errors during an update, delete Windows Update files (`C:\Windows\SoftwareDistribution\Download\` - Delete all folders inside)
* Clean all drives (Type "Disk Cleanup" in the Windows search bar - Run as administrator)

## 🔧Advanced Practices
First, I recommend Windows Pro edition for more system control

Update your BIOS and drivers **via your motherboard’s website**. Avoid CCleaner, DriverCloud, or DriverBooster—these utilities can install outdated or incompatible drivers. In the BIOS, ensure UEFI mode is enabled and, for gaming, that the RAM XMP profile is activated

Install only essential programs to avoid system slowdowns and bugs

Disable as many startup programs as possible (`Ctrl` + `Shift` + `Esc` - Startup)

Disable Widgets on Windows 11: `REG ADD "HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Dsh" /v AllowNewsAndInterests /t REG_DWORD /d 00000000 /f` - Restart PC | To undo: `REG DELETE "HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Dsh" /v AllowNewsAndInterests /f`

Disable Windows Copilot: `REG ADD "HKEY_CURRENT_USER\Software\Policies\Microsoft\Windows\WindowsCopilot" /v TurnOffWindowsCopilot /t REG_DWORD /d 1 /f`

Disable the SysMain service, which can cause disk performance issues: `sc stop "SysMain" & sc config "SysMain" start=disabled` | To undo: `sc config "SysMain" start=auto & sc start "SysMain"`

Uncheck "Enhance pointer precision" to disable mouse acceleration (Control Panel - Hardware and Sound - Mouse - Pointer Options)

Disable Fast Boot and hibernation to free up space (~3GB) and prevent potential bugs, using these **two** commands: `REG ADD "HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Session Manager\Power" /v HiberbootEnabled /t REG_DWORD /d 00000000 /f` + `powercfg -h off` - Restart PC | To undo: `REG ADD "HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Session Manager\Power" /v HiberbootEnabled /t REG_DWORD /d 00000001 /f` + `powercfg -h on`

> [!NOTE]
> Disabling Fast Boot will make your PC boot slightly slower (1-2s), but your computer will fully shut down, making the system more stable.

Uncheck as many options as possible in the "Privacy" section in Windows Settings to limit Microsoft’s data collection (Advertising ID, diagnostic data, location, contacts, etc.)

Install **all** versions of [Visual C++](https://www.techpowerup.com/download/visual-c-redistributable-runtime-package-all-in-one/) to avoid missing DLL errors

Disable permanent Xbox Game Bar recording, which uses resources in the background, with these **two** commands: `REG ADD "HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows\CurrentVersion\GameDVR" /v AppCaptureEnabled /t REG_DWORD /d 00000000 /f` + `REG ADD "HKEY_CURRENT_USER\System\GameConfigStore" /v GameDVR_Enabled /t REG_DWORD /d 00000000 /f` - Restart PC | To undo: `REG ADD "HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows\CurrentVersion\GameDVR" /v AppCaptureEnabled /t REG_DWORD /d 00000001 /f` + `REG ADD "HKEY_CURRENT_USER\System\GameConfigStore" /v GameDVR_Enabled /t REG_DWORD /d 00000001 /f`

Disable Bing results in Windows Search: `REG ADD "HKEY_CURRENT_USER\SOFTWARE\Policies\Microsoft\Windows\Explorer" /v DisableSearchBoxSuggestions /t REG_DWORD /d 00000001 /f` - Restart PC | To undo: `REG ADD "HKEY_CURRENT_USER\SOFTWARE\Policies\Microsoft\Windows\Explorer" /v DisableSearchBoxSuggestions /t REG_DWORD /d 00000000 /f`

Restore the old Windows 10 right-click menu on Windows 11: `REG ADD "HKCU\Software\Classes\CLSID\{86ca1aa0-34aa-4e8b-a509-50c905bae2a2}\InprocServer32" /f /ve` - Restart PC | To undo: `REG DELETE "HKCU\Software\Classes\CLSID\{86ca1aa0-34aa-4e8b-a509-50c905bae2a2}" /f`

For gaming, Microsoft recommends disabling memory integrity and virtual machine platform [here](https://www.microsoft.com/en-us/windows/learning-center/optimize-pc-for-gaming-performance)

**Microsoft Edge:**
* I do not recommend using Microsoft Edge for privacy reasons; prefer Firefox
* Disable startup boost and background running in "System and Performance"
* Disable data sharing with other Windows features in "Profiles"
* Select "Strict" tracking protection in "Privacy, search, and services"
* Block third-party cookies and preloading in "Cookies and site data"

**Power Options in Control Panel:**
* Intel CPU: Choose "High performance"
* AMD Ryzen 1000, 2000, 3000, and 4000: Choose "AMD Ryzen Balanced"
* AMD Ryzen 5000 and newer: Choose "Balanced"
* In advanced power settings: Disable USB selective suspend

**Nvidia/AMD Control Panel:**
* Select the highest possible refresh rate (144Hz, 180Hz, etc.)
* Enable G-SYNC/FreeSync + V-SYNC + cap FPS at 3 below your monitor’s refresh rate to avoid screen tearing (144Hz screen → cap at 141FPS)
* Nvidia specific: Set low latency mode to "On"
* AMD specific: It’s better to cap FPS via [RTSS](https://www.guru3d.com/files-details/rtss-rivatuner-statistics-server-download.html) rather than the AMD panel for lower latency

> [!IMPORTANT]
> If V-SYNC is enabled in the Nvidia/AMD panel, disable it in all game settings to avoid conflicts.

> [!NOTE]
> For games that allow FPS capping, it’s best to do it in the game settings rather than the Nvidia/RTSS panel for lower latency.

Use [MPO-GPU-FIX](https://github.com/RedDot-3ND7355/MPO-GPU-FIX) to disable MPO (Multi-Plane Overlay), which can cause performance and stability issues in games

For advanced gaming use, you may consider overclocking/undervolting your GPU, but do your research and know what you’re doing. Personally, I use [MSI Afterburner](https://www.msi.com/Landing/afterburner/graphics-cards) and [Kombustor](https://msikombustor.com/) to test system stability. I consider a GPU stable if its temperature does not exceed 85°C and Kombustor detects **no** artifacts for at least 10 minutes

## 💡Tips
* Reinstall Windows before applying these tweaks for a clean base:
  * Using a USB drive for a full system reinstall (backup first)
  * Or via [ISO file](https://www.microsoft.com/en-us/software-download/) to keep your documents/settings

> [!NOTE]
> Reinstallation is very effective for fixing bugs/crashes. Once done, do not sign in with your Microsoft account; create a local account to limit data collection.

* Use Windows Defender, which does a great job. Avoid Avast, Bitdefender, etc.
* Always keep Windows and your programs up to date for security and stability, especially your browser.
* Prefer [Firefox](https://www.mozilla.org/en-US/firefox/new/) over Google Chrome for privacy. Configure it to block third-party cookies and use HTTPS only. Install the [uBlock Origin](https://ublockorigin.com/) extension (with [this additional filter](https://raw.githubusercontent.com/DandelionSprout/adfilt/master/LegitimateURLShortener.txt)) for ad and tracker blocking. Avoid other ad blockers and limit the number of extensions. Never save passwords in your browser; use a manager like [Bitwarden](https://bitwarden.com/).
* Use a [custom DNS](https://www.privacyguides.org/dns/) (DoH, in Windows settings) instead of your ISP’s.
* A good free VPN I recommend is [ProtonVPN](https://protonvpn.com/), or a paid one like [Mullvad](https://mullvad.net/).
* Enable BitLocker on your laptop to encrypt your drive and secure your files (Right-click a drive - Turn on BitLocker).
> [!WARNING]
> Make sure to back up your BitLocker recovery key to a cloud or external drive!

* Shut down your computer at night; do not put it to sleep to prevent bugs. Regularly clean your PC from dust to prevent components from overheating and losing performance.
* Enable Windows Night Light to reduce eye strain.
* Use [OpenRGB](https://gitlab.com/CalcProgrammer1/OpenRGB) to control all your RGB components with a single software. This avoids resource-heavy software like Razer Synapse, ASUS Aura, or MSI Dragon Center.

## 🎉Conclusion
That’s it! Your PC should now be faster and more performant. I advise against other tweaks that could damage your system (custom ISOs, PowerShell scripts, internet connection optimizers—these are often scams or placebo).

## 🔗Sources
[Microsoft](https://learn.microsoft.com/en-us/windows/security/) | [Discord Entraide Informatique](https://discord.gg/WMsR7dT) | [Piwi](https://www.youtube.com/c/Piwi_youtube) | [BlurBusters](https://blurbusters.com/) | [PrivacyGuides](https://privacyguides.org/) | [Reddit](https://www.reddit.com/r/Windows11/)
