# WindowsBestPractices

![Last-Commit](https://img.shields.io/github/last-commit/PouletEnSlip/WindowsBestPractices)

[en](/README.md), [fr](/README-FR.md)

Hello! Here are simple and healthy manipulations for a PC running Windows 10 or 11. This allow you to have a more powerful PC for office and video games. These manipulations are without risk and can solve the slowness and crashes of your PC. These practices are not "magic", I do not promise an incredible gain, the most effective being to buy new and more efficient components. Read everything before doing anything.

## 📖 Table of contents
- [Quick practices](#quick-practices)
- [Advanced practices](#advanced-practices)
- [Tips](#tips)
- [Conclusion](#conclusion)
- [Sources](#sources)

## 🧹Quick practices
In order, to be repeated approximately once a month:
* Update the drivers for your [Nvidia](https://www.nvidia.com/Download/index.aspx?lang=en-us) or [AMD](https://www.amd.com/en/support) graphics card, use [DDU](https://www.guru3d.com/files-details/display-driver-uninstaller-download.html) to remove old drivers properly. DDU is **essential** because it allows you to fix crashes and performance issues on your games
* Update Windows via Windows Update in settings

Once everything is up to date and the PC has been restarted:
* Delete your browser history, cache and cookies
* Delete Windows Update files (`C:/Windows/SoftwareDistribution/Download/` - Delete all folders inside to avoid errors during future updates)
* Delete all temporary files (`Windows` + `R` - Type "%temp%" - Delete all)
* Repair system files: `sfc /scannow`
* Flush DNS cache: `ipconfig /flushdns`
* Repair Windows image: `Dism /Online /Cleanup-Image /RestoreHealth`
* Reset icon cache with my [script](https://github.com/PouletEnSlip/ResetIconCache) to avoid white icons
* Clean all drives (Type "Disk Cleanup" in the Windows search bar - Run as administrator - Check all)
* Optimize all drives (Right click on a drive - Properties - Tools - Optimize)

## 🔧Advanced practices
Update the BIOS and drivers **via your motherboard's website**. Avoid CCleaner, Driverscloud or DriverBooster, these utilities may install outdated or non-compatible drivers with your components

Uninstall as many unused software as possible through the Control Panel

> [!CAUTION]
> Never uninstall system applications like Microsoft Edge or Microsoft Store, this could cause huge damage to your system

Disable as many programs as possible that launch at Windows startup (`Ctrl` + `Maj` + `Esc` - Startup)

Disable Widgets on Windows 11: `REG ADD "HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Dsh" /v AllowNewsAndInterests /t REG_DWORD /d 00000000 /f` - Restart PC | To cancel: `REG DELETE "HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Dsh" /v AllowNewsAndInterests /f`

Disable Sysmain service which can cause drive performance issues: `sc stop "SysMain" & sc config "SysMain" start=disabled` | To cancel: `sc config "SysMain" start=auto & sc start "SysMain"`

Uncheck "Enhance pointer precision" to disable mouse acceleration (Control Panel - Hardware - Mouse - Pointer Options)

Disable fast boot and hibernation to free up space (~3GB) and avoid bugs, with these **two** commands: `REG ADD "HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Session Manager\Power" /v HiberbootEnabled /t REG_DWORD /d 00000000 /f` + `powercfg -h off` - Restart PC | To cancel: `REG ADD "HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Session Manager\Power" /v HiberbootEnabled /t REG_DWORD /d 00000001 /f` + `powercfg -h on`

> [!NOTE]
> Disabling fast boot will make your PC start up a bit longer (1-2s), however your computer will completely shut down when you turn it off, which will make the system more stable

Uncheck as many options as possible in the "Privacy" section in Windows settings to limit the collection of personal data by Microsoft (diagnostic data, location, contacts...)

Install **all** versions of [Visual C++](https://www.techpowerup.com/download/visual-c-redistributable-runtime-package-all-in-one/) to avoid missing DLL errors

Disable persistent Xbox Game Bar recording that takes up resources in the background, with these **two** commands: `REG ADD "HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows\CurrentVersion\GameDVR" /v AppCaptureEnabled /t REG_DWORD /d 00000000 /f` + `REG ADD "HKEY_CURRENT_USER\System\GameConfigStore" /v GameDVR_Enabled /t REG_DWORD /d 00000000 /f` - Restart PC | To cancel: `REG ADD "HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows\CurrentVersion\GameDVR" /v AppCaptureEnabled /t REG_DWORD /d 00000001 /f` + `REG ADD "HKEY_CURRENT_USER\System\GameConfigStore" /v GameDVR_Enabled /t REG_DWORD /d 00000001 /f`

Disable Bing results in Windows Search: `REG ADD "HKEY_CURRENT_USER\SOFTWARE\Policies\Microsoft\Windows\Explorer" /v DisableSearchBoxSuggestions /t REG_DWORD /d 00000001 /f` - Restart PC | To cancel: `REG ADD "HKEY_CURRENT_USER\SOFTWARE\Policies\Microsoft\Windows\Explorer" /v DisableSearchBoxSuggestions /t REG_DWORD /d 00000000 /f`

Bring old Windows 10 context menu back on Windows 11: `REG ADD "HKCU\Software\Classes\CLSID\{86ca1aa0-34aa-4e8b-a509-50c905bae2a2}\InprocServer32" /f /ve` - Restart PC | To cancel: `REG DELETE "HKCU\Software\Classes\CLSID\{86ca1aa0-34aa-4e8b-a509-50c905bae2a2}" /f`

For gaming use, Microsoft recommends disabling Memory Integrity and Virtual Machine Platform [here](https://support.microsoft.com/en-us/windows/options-to-optimize-gaming-performance-in-windows-11-a255f612-2949-4373-a566-ff6f3f474613)

**Microsoft Edge:**
* Disable fast startup and background running in "System and performance" tab
* Disable data sharing with other Windows features in the "Profiles" tab
* Select "Strict" Tracking Protection in the "Privacy, search & services" tab
* Block third-party cookies and preloading in the "Cookies and site permissions" tab

**Power plan in Control Panel:**
* Intel CPU: choose "High performance"
* AMD Ryzen 1000, 2000, 3000 and 4000 CPU: choose "AMD Ryzen Balanced"
* AMD Ryzen 5000 CPU and newer: choose "Balanced"
* In advanced plan settings: disable USB selective suspend

**Nvidia/AMD panel:**
* Select the highest possible refresh rate (144Hz, 180Hz...)
* Choose the highest possible color intensity/depth (8bpc, 10bpc...)
* Enable G-SYNC/FreeSync + V-SYNC + lock FPS to 3 below screen refresh rate to avoid frame tearing (144Hz screen → lock to 141FPS)
* Nvidia specific: choose "full" dynamic range in the video colors tab, choose low latency mode on "On" and prefer maximum performance in advanced 3D image settings
* AMD specific: it is better to lock FPS via [RTSS](https://www.guru3d.com/files-details/rtss-rivatuner-statistics-server-download.html) rather than AMD panel for lower latency

> [!IMPORTANT]
> If you enable V-SYNC in the Nvidia/AMD panel, you must disable it in-game to avoid conflicts

> [!NOTE]
> For games that allow FPS locking, it's better to lock the FPS in-game rather than the Nvidia/RTSS panel for lower latency

Use [MPO-GPU-FIX](https://github.com/RedDot-3ND7355/MPO-GPU-FIX) to disable MPO (Multi-Plane Overlay) which can cause performance and stability issues in games

## 💡Tips
* Reinstall Windows (preferably Pro) completely (using a USB key, not through the settings) before applying these manipulations to start with a clean system. During the Windows installation, do not sign in with your Microsoft account, create a local account to limit data collection
* If you suspect you have a virus, install [Malwarebytes](https://downloads.malwarebytes.com/file/mb4_offline) and perform a scan to remove threats. However, the most effective solution is to reinstall Windows as mentioned above
* Use the Windows antivirus which does its job very well. Avoid Avast, Bitdefender...
* Always keep Windows and its programs up-to-date for security and stability reasons, especially the browser
* Prefer [Firefox](https://www.mozilla.org/en-US/firefox/new/) to Chrome for privacy reasons, configure it to block third-party cookies and use HTTPS only
* Install [uBlock Origin](https://ublockorigin.com/) extension for blocking ads and trackers. Avoid any other adblockers and try to limit the number of installed extensions
* Use a custom DNS (DoH, in Windows settings) like [Quad9](https://www.quad9.net) or [Mullvad](https://mullvad.net/fr/help/dns-over-https-and-dns-over-tls/) rather than that of the local provider for security and privacy reasons
* A good free VPN that I recommend is [ProtonVPN](https://protonvpn.com/) for privacy reasons. Or a paid VPN like [Mullvad](https://mullvad.net/en) for the same reasons
* Enable BitLocker on your laptop to encrypt drive data and secure your files (Right click on a drive - Enable BitLocker)
* Turn off the computer at night, do not put it to sleep to prevent bugs. Also regularly clean the PC of dust to prevent the components from heating up too much and therefore losing performance
* Activate night lighting in the evening to avoid eye fatigue

> [!WARNING]
> Be sure to back up the BitLocker recovery key to a cloud or an external drive!

* Use [OpenRGB](https://gitlab.com/CalcProgrammer1/OpenRGB) to control all your RGB components through a single software. Thus, we avoid software like Razer Synapse, ASUS Aura or MSI Dragon Center which use resources in the background
* Going further, you can think about overclocking and undervolting your GPU, but be sure of what you are doing. Personally I use [MSI Afterburner](https://www.msi.com/Landing/afterburner/graphics-cards) and [Kombustor](https://msikombustor.com/) to test the stability of my system. I consider that a GPU seems stable if its temperature does not exceed 85°C/185°F and that Kombustor does not detect **any** artifacts in at least 10 minutes

## 🎉Conclusion
That's it! Your PC should be faster and more efficient. I recommend reinstalling Windows every year, taking care to make backups. I advise against other manipulations which could damage the system (custom ISO, PowerShell scripts, Internet connection optimizer... these are scams).

### 🔗Sources
[Microsoft](https://learn.microsoft.com/en-us/windows/security/) | [Discord Entraide Informatique (fr)](https://discord.gg/WMsR7dT) | [Piwi](https://www.youtube.com/c/Piwi_youtube) | [BlurBusters](https://blurbusters.com) | [PrivacyGuides](https://privacyguides.org/)
