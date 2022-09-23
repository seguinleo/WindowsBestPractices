# Optimize Windows

[en](/README.md), [fr](/README-FR.md)

Hello! Here are simple and healthy optimizations for a PC running Windows 10 and 11. These manipulations allow you to have a more fluid and efficient computer for office automation, video montage or video games. These manipulations are without risk and can solve the slowness and crashes of your PC. Your computer will also be able to boot faster and launch tasks faster. These optimizations are not "magical", I do not promise an incredible gain, the main optimization is to buy more efficient components. Read everything before doing anything.

## Table of contents
- [Quick optimizations](#quick-optimizations)
- [Advanced optimizations](#advanced-optimizations)
- [Optional](#optional)
- [Conclusion](#conclusion)

## Quick optimizations
In order, to be repeated approximately once a month:
* Check for viruses/malware with [Malwarebytes](https://malwarebytes.com/)
* Delete browser history, cache and cookies (will need to reconnect to sites)
* Update the BIOS and drivers via your motherboard's website. Avoid CCleaner, Driverscloud, DriverBooster… these utilities can install obsolete drivers or drivers that are not compatible with your components
* Update the drivers for your [Nvidia](https://www.nvidia.com/Download/index.aspx?lang=en-us) or [AMD](https://www.amd.com/en/support) graphics card , use [DDU](https://www.guru3d.com/files-details/display-driver-uninstaller-download.html) and [NVCleanstall](https://www.techpowerup.com/download/techpowerup-nvcleanstall/) to update the drivers properly. DDU is essential because it allows you to fix many bugs/crashes on your games, utility recommended by Nvidia. NVCleanstall allows minimal installation of Nvidia drivers, you can also check the Message Signaled Interrupts box **only**, in expert tweaks, which offers more efficient GPU communication (setting enabled by default on AMD GPUs)
* Update Windows via Windows Update in settings. Don't use Windows 7 anymore, too many security flaws and compatibility issues

Once everything is up to date and the PC has been restarted:
* Delete Windows Update files after each update (C:/Windows/SoftwareDistribution/Download/ - Delete all folders to avoid errors in future updates)
* Delete all temporary files (`Windows` + `R` - Type %temp% - Delete all)
* Repair system files: `sfc /scannow`
* Flush DNS cache: `ipconfig /flushdns`
* Repair Windows Image: `Dism /Online /Cleanup-Image /RestoreHealth`
* Reset icon cache with my [script](https://github.com/PouletEnSlip/ResetIconCache)
* Clean all drives (Right click on a drive - Properties - Cleanup - Clean up system files - Check all)
* Optimize all drives (Right click on a drive - Properties - Tools - Optimize)

Remember to turn off your computer at night, do not put it to sleep to prevent bugs. Also regularly clean your PC of dust to prevent components from overheating and therefore losing performance.

## Advanced optimizations
Uninstall as many Windows applications and software as possible that you don't use (Start menu - Right click on a program - Uninstall)

Disable as many programs as possible that launch at Windows startup (`Ctrl` + `Maj` + `Esc` - Startup)

Disable Cortana: `REG ADD "HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\Windows Search" /v AllowCortana /t REG_DWORD /d 00000000 /f` - Restart PC | To cancel: `REG ADD "HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\Windows Search" /v AllowCortana /t REG_DWORD /d 00000001 /f`

Disable Widgets on Windows 11: `REG ADD "HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Dsh" /v AllowNewsAndInterests /t REG_DWORD /d 00000000 /f` - Restart PC | To cancel: `REG DELETE "HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Dsh" /v AllowNewsAndInterests /f`

Disable enhance pointer precision to be more precise (Control Panel - Hardware - Mouse - Pointer Options)

Disable hibernation to free up space (~3GB) and make the PC shut down completely when you turn it off, with these two commands: `REG ADD "HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Session Manager\Power" /v HiberbootEnabled /t REG_DWORD /d 00000000 /f` + `powercfg -h off` - Restart PC | To cancel: `REG ADD "HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Session Manager\Power" /v HiberbootEnabled /t REG_DWORD /d 00000001 /f` + `powercfg -h on`

Reduce CPU resources reserved for certain Windows processes: `REG ADD "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\WindowsNT\CurrentVersion\Multimedia\SystemProfile" /v SystemResponsiveness /t REG_DWORD /d 00000010 /f` - Restart PC | To cancel: `REG ADD "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\WindowsNT\CurrentVersion\Multimedia\SystemProfile" /v SystemResponsiveness /t REG_DWORD /d 00000020 /f`

Uncheck all the boxes on each tab of the “Privacy” section in Windows settings to limit the collection of personal data by Microsoft (location, contacts, etc.)

Install **all** versions of [Visual C++](https://www.techpowerup.com/download/visual-c-redistributable-runtime-package-all-in-one/) to avoid missing DLL errors

Disable Xbox Game Bar, with these three commands: `Get-AppxPackage Microsoft.XboxGamingOverlay | Remove-AppxPackage` + `REG ADD "HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows\CurrentVersion\GameDVR" /v AppCaptureEnabled /t REG_DWORD /d 00000000 /f` + `REG ADD "HKEY_CURRENT_USER\System\GameConfigStore" /v GameDVR_Enabled /t REG_DWORD /d 00000000 /f` - Restart PC | To cancel: `Get-AppxPackage -allusers *Microsoft.XboxGamingOverlay* | Foreach {Add-AppxPackage -DisableDevelopmentMode -Register “$($_.InstallLocation)\AppXManifest.xml”}` + `REG ADD "HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows\CurrentVersion\GameDVR" /v AppCaptureEnabled /t REG_DWORD /d 00000001 /f` + `REG ADD "HKEY_CURRENT_USER\System\GameConfigStore" /v GameDVR_Enabled /t REG_DWORD /d 00000001 /f`

Disable Bing results in Windows Search: `REG ADD "HKEY_CURRENT_USER\SOFTWARE\Policies\Microsoft\Windows\Explorer" /v DisableSearchBoxSuggestions /t REG_DWORD /d 00000001 /f` - Restart PC | To cancel: `REG ADD "HKEY_CURRENT_USER\SOFTWARE\Policies\Microsoft\Windows\Explorer" /v DisableSearchBoxSuggestions /t REG_DWORD /d 00000000 /f`

Bring old Windows 10 context menu back on Windows 11: `REG ADD "HKCU\Software\Classes\CLSID\{86ca1aa0-34aa-4e8b-a509-50c905bae2a2}\InprocServer32" /f /ve` - Restart PC | To cancel: `REG DELETE "HKCU\Software\Classes\CLSID\{86ca1aa0-34aa-4e8b-a509-50c905bae2a2}" /f`

Disable “Sysmain” and “Windows Search” services if Windows is installed on an SSD (`Windows` + `R` - Type "services.msc")

**Change power options in Control Panel:** Intel CPU → choose “High Performance” | AMD Ryzen 1000, 2000, 3000 and 4000 CPUs → choose “AMD Ryzen Balanced” | AMD Ryzen 5000 CPU → choose “Balanced”. In advanced settings: stop hard drive after 0min (never) and disable USB selective suspend

**Nvidia control panel changes (similar AMD options):** select “Use the advanced 3D image settings”, low latency mode to “On”, prefer maximum performance, enable G-SYNC + V-SYNC + limit FPS to 2 below screen refresh rate to avoid frame tearing (144Hz screen → limit to 142FPS). If you enable V-SYNC in the Nvidia panel, you have to disable it in the game settings. Choose full dynamic range and choose 10bpc (or more) if possible. These settings are best for someone looking for the best possible image quality with negligible impact on latency

**Overclock your graphic card (Nvidia and AMD):** overclocking allows you to increase the clock frequency of the graphics card and thus have more performance in the game. However, the temperature of the card may increase. Personally I use [Afterburner](https://www.msi.com/Landing/afterburner/graphics-cards) and [Kombustor](https://msikombustor.com/). I like Kombustor because it allows to scan the number of artifacts (you have to check the box on the menu and choose your native resolution). Indeed, an overclock can be stable on Unigine Heaven but the computer can crash on a more resource-intensive game. I consider that an overclock is stable if the temperature of the card does not exceed 85°C and Kombustor does not detect any artifact in 10 minutes

## Optional
* Reinstall Windows completely (via a USB key) before applying these optimizations to start on a clean system
* Prefer [Firefox](https://www.mozilla.org/en-US/firefox/new/) or [Brave](https://brave.com/) to Chrome for privacy, learn more [here](https://privacytests.org/)
* [uBlock Origin](https://ublockorigin.com) extension recommended for blocking ads and trackers (I do not recommend Adblock which does not block all ads and no tracers, learn more [here](https://adblock-tester.com/))
* Enable Bitlocker on the Pro version of Windows to encrypt drive data and secure your files (Right click on a drive - Enable Bitlocker). It is possible to activate Bitlocker on the Home version of Windows, under certain conditions
* Uninstall Windows Media Player and install [VLC](https://www.videolan.org/) for better performance
* Install [7-Zip](https://www.7-zip.org/) to compress files and be able to encrypt archives. I do not recommend WinRAR which is not open source and not free
* Use [Hat.sh](https://hat.sh/) to encrypt your sensitive documents/archives with a password
* If you want a free VPN, [ProtonVPN](https://protonvpn.com/download) is the best choice. Avoid all other free VPNs that can sell your personal data
* Use [OpenRGB](https://gitlab.com/CalcProgrammer1/OpenRGB) to control all your RGB components through a single software. Thus, we avoid software like Razer Synapse or Dragon Center which use a lot of resources in the background
* Install [EqualizerAPO](https://sourceforge.net/projects/equalizerapo/) to improve sound rendering, boost bass and treble. You have to choose SFX/EFX during the installation. If you update your sound card drivers, you will have to reconfigure EqualizerAPO. This software has almost no performance impact

## Conclusion
That's it! Your PC should be faster and more efficient. I recommend reinstalling Windows every year, taking care to make backups. I advise against other manipulations coming from other sites which could damage the system (custom ISO, PowerShell scripts, Internet connection optimizer... these are scams).

### Sources
[Discord Entraide Informatique](https://discord.gg/WMsR7dT) | [Piwi](https://github.com/Piwielle) | [BlurBusters](https://blurbusters.com/) | [PrivacyGuides](https://privacyguides.org/)

*Update : 23/09/2022*
