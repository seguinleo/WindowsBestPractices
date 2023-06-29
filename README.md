# WindowsBestPractices

![img](https://repository-images.githubusercontent.com/540623246/8acc5ef0-1f81-4ab5-96f2-3aa5ef0a2c45)

[en](/README.md), [fr](/README-FR.md)

Hello! Here are simple and healthy manipulations for a PC running Windows 10 or 11. This allow you to have a more powerful PC for office and video games. These manipulations are without risk and can solve the slowness and crashes of your PC. These practices are not "magic", I do not promise an incredible gain, the most effective being to buy new and more efficient components. Read everything before doing anything.

## 📖 Table of contents
- [Quick practices](#quick-practices)
- [Advanced practices](#advanced-practices)
- [Optional](#optional)
- [Conclusion](#conclusion)
- [Sources](#sources)

## 🧹Quick practices
In order, to be repeated approximately once a month:
* Delete your browser history, cache and cookies
* Update the BIOS and drivers **via your motherboard's website**. Avoid CCleaner, Driverscloud or DriverBooster, these utilities may install outdated or non-compatible drivers with your components
* Update the drivers for your [Nvidia](https://www.nvidia.com/Download/index.aspx?lang=en-us) or [AMD](https://www.amd.com/en/support) graphics card, use [DDU](https://www.guru3d.com/files-details/display-driver-uninstaller-download.html) to remove old drivers properly. DDU is **essential** because it allows you to fix crashes and performance issues on your games (recommended by Nvidia). Activate the [Message Signaled Interrupts](https://www.mediafire.com/file/ewpy1p0rr132thk/MSI_util_v3.zip) **only for graphics card** (enabled by default on AMD graphics card), it will have to be reactivated after each drivers update
* Update Windows via Windows Update in settings
* Update your softwares

Once everything is up to date and the PC has been restarted:
* Delete Windows Update files (`C:/Windows/SoftwareDistribution/Download/` - Delete all folders inside to avoid errors during future updates)
* Delete all temporary files (`Windows` + `R` - Type "%temp%" - Delete all)
* Repair system files: `sfc /scannow`
* Flush DNS cache: `ipconfig /flushdns`
* Repair Windows image: `Dism /Online /Cleanup-Image /RestoreHealth`
* Reset icon cache with my [script](https://github.com/PouletEnSlip/ResetIconCache) to avoid white icons
* Clean all drives (Type "Disk Cleanup" in the Windows search bar - Run as administrator - Check all)
* Optimize all drives (Right click on a drive - Properties - Tools - Optimize)

> **Note** Remember to turn off your computer at night, do not put it to sleep to avoid bugs. Also regularly clean your PC of dust to avoid components from overheating and therefore losing performance

## 🔧Advanced practices
Uninstall a maximum of Windows applications and software that you do not use via the Control Panel. Do not use tools like Revo Uninstaller or CCleaner which can uninstall system apps like Edge or Store which will make the system unstable

Disable as many programs as possible that launch at Windows startup (`Ctrl` + `Maj` + `Esc` - Startup)

Disable Widgets on Windows 11: `REG ADD "HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Dsh" /v AllowNewsAndInterests /t REG_DWORD /d 00000000 /f` - Restart PC | To cancel: `REG DELETE "HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Dsh" /v AllowNewsAndInterests /f`

Uncheck "Enhance pointer precision" to avoid mouse acceleration (Control Panel - Hardware - Mouse - Pointer Options)

Disable fast boot and hibernation to free up space (~3GB) and avoid bugs, with these **two** commands: `REG ADD "HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Session Manager\Power" /v HiberbootEnabled /t REG_DWORD /d 00000000 /f` + `powercfg -h off` - Restart PC | To cancel: `REG ADD "HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Session Manager\Power" /v HiberbootEnabled /t REG_DWORD /d 00000001 /f` + `powercfg -h on`
> **Note** Disabling fast boot will make your PC start up a bit longer, however your computer will completely shut down when you turn it off, which will make the system more stable and avoid bugs

Uncheck as many boxes as possible in the "Privacy" section in Windows settings to limit the collection of personal data by Microsoft (diagnostic data, location, contacts...)

Install **all** versions of [Visual C++](https://www.techpowerup.com/download/visual-c-redistributable-runtime-package-all-in-one/) to avoid missing DLL errors

Disable Xbox Game Bar if you don't use it, with these **three** commands: `Get-AppxPackage Microsoft.XboxGamingOverlay | Remove-AppxPackage` + `REG ADD "HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows\CurrentVersion\GameDVR" /v AppCaptureEnabled /t REG_DWORD /d 00000000 /f` + `REG ADD "HKEY_CURRENT_USER\System\GameConfigStore" /v GameDVR_Enabled /t REG_DWORD /d 00000000 /f` - Restart PC | To cancel: `Get-AppxPackage -allusers *Microsoft.XboxGamingOverlay* | Foreach {Add-AppxPackage -DisableDevelopmentMode -Register “$($_.InstallLocation)\AppXManifest.xml”}` + `REG ADD "HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows\CurrentVersion\GameDVR" /v AppCaptureEnabled /t REG_DWORD /d 00000001 /f` + `REG ADD "HKEY_CURRENT_USER\System\GameConfigStore" /v GameDVR_Enabled /t REG_DWORD /d 00000001 /f`

Disable Bing results in Windows Search: `REG ADD "HKEY_CURRENT_USER\SOFTWARE\Policies\Microsoft\Windows\Explorer" /v DisableSearchBoxSuggestions /t REG_DWORD /d 00000001 /f` - Restart PC | To cancel: `REG ADD "HKEY_CURRENT_USER\SOFTWARE\Policies\Microsoft\Windows\Explorer" /v DisableSearchBoxSuggestions /t REG_DWORD /d 00000000 /f`

Bring old Windows 10 context menu back on Windows 11: `REG ADD "HKCU\Software\Classes\CLSID\{86ca1aa0-34aa-4e8b-a509-50c905bae2a2}\InprocServer32" /f /ve` - Restart PC | To cancel: `REG DELETE "HKCU\Software\Classes\CLSID\{86ca1aa0-34aa-4e8b-a509-50c905bae2a2}" /f`

For gaming use, Microsoft recommends disabling Memory Integrity and Virtual Machine Platform [here](https://support.microsoft.com/en-us/windows/options-to-optimize-gaming-performance-in-windows-11-a255f612-2949-4373-a566-ff6f3f474613)

**Change power options in control panel:**
* Intel CPU: choose "High performance"
* AMD Ryzen 1000, 2000, 3000 and 4000 CPU: choose "AMD Ryzen Balanced"
* AMD Ryzen 5000 CPU and newer: choose "Balanced"
* In advanced plan settings: turn off hard disk after 0min (never) and disable USB selective suspend

**Nvidia/AMD panel changes:**
* Select the highest possible refresh rate (144Hz, 180Hz...)
* Choose the highest possible color intensity/depth (8bpc, 10bpc...)
* Enable G-SYNC/FreeSync + V-SYNC + limit FPS to 2 below screen refresh rate to avoid frame tearing (144Hz screen → limit to 142FPS)
* Nvidia specific: choose "full" dynamic range in the video colors tab, select "Use advanced 3D image settings", in these settings -> low latency mode on "On", prefer maximum performance
* AMD specific: enable Radeon Anti-Lag and Radeon Chill
> **Warning** If you enable V-SYNC in the Nvidia/AMD panel, you must disable it in the settings of all your games to avoid conflicts!

**Overclock your graphics card:** overclocking allows you to increase the clock frequency of your graphics card and thus have more performance in games. However, the temperature of the card may increase. Video guide [here](https://www.youtube.com/watch?v=6_Me603fnq8). Personally I use [Afterburner](https://www.msi.com/Landing/afterburner/graphics-cards) and [Kombustor](https://msikombustor.com/). Kombustor allows you to scan the number of artifacts (you have to check the box on the welcome screen and choose your screen resolution). I consider an overclock to be stable if the temperature of the graphics card does not exceed 85°C/185°F and Kombustor detects **no** artifacts in at least 10 minutes. Then, try on a very resource-intensive game to verify that the system is stable over time

**Undervolt your graphics card:** undervolting allows you to lower the voltage received by the graphics card and thus have lower temperatures. Video guide [here](https://www.youtube.com/watch?v=eaVp6vcVIts). Just like overclocking, I use Afterburner and Kombustor

## 💡Optional
* Reinstall Windows (preferably Pro) completely (with a USB key) before applying these manipulations to start on a clean system
* Always keep Windows and its programs up-to-date for security and stability reasons
* Use the Windows antivirus which does its job very well. Avoid Avast, Bitdefender...
* Prefer [Firefox](https://www.mozilla.org/en-US/firefox/new/) to Chrome for privacy reasons, configure it to block third-party cookies and use HTTPS only
* Install [uBlock Origin](https://ublockorigin.com/) extension for blocking ads and trackers. Avoid Adblock Plus
* Use a custom DNS (DoH, in Windows settings) like [Quad9](https://www.quad9.net) or [Mullvad](https://mullvad.net/fr/help/dns-over-https-and-dns-over-tls/) rather than that of the local provider for security and privacy reasons
* Enable BitLocker on your laptop to encrypt drive data and secure your files (Right click on a drive - Enable BitLocker)
> **Warning** Be sure to back up the BitLocker recovery key to a cloud or an external drive!
* Use [OpenRGB](https://gitlab.com/CalcProgrammer1/OpenRGB) to control all your RGB components through a single software. Thus, we avoid software like Razer Synapse, ASUS Aura or MSI Dragon Center which use resources in the background

## 🎉Conclusion
That's it! Your PC should be faster and more efficient. I recommend reinstalling Windows every year, taking care to make backups. I advise against other manipulations which could damage the system (custom ISO, PowerShell scripts, Internet connection optimizer... these are scams).

### 🔗Sources
[Microsoft](https://learn.microsoft.com/en-us/windows/security/) | [Discord Entraide Informatique (fr)](https://discord.gg/WMsR7dT) | [Piwi](https://www.youtube.com/c/Piwi_youtube) | [BlurBusters](https://blurbusters.com) | [PrivacyGuides](https://privacyguides.org/)
