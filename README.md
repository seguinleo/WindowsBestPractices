# WindowsBestPractices

[en](/README.md), [fr](/README-FR.md)

Hello! Here are simple and healthy practices for a PC running Windows 10 or 11. These manipulations allow you to have a more powerful PC for office and video games. These manipulations are without risk and can solve the slowness and crashes of your PC. These practices are not "magic", I do not promise an incredible gain, the most effective being to buy new components. Read everything before doing anything.

## Table of contents
- [Quick practices](#quick-practices)
- [Advanced practices](#advanced-practices)
- [Optional](#optional)
- [Conclusion](#conclusion)

## Quick practices
In order, to be repeated approximately once a month:
* Check for viruses/malware with [Malwarebytes](https://malwarebytes.com/)
* Delete your browser history, cache and cookies
* Update the BIOS and drivers **via your motherboard's website**. Avoid CCleaner, Driverscloud or DriverBooster, these utilities may install outdated or non-compatible drivers with your components
* Update the drivers for your [Nvidia](https://www.nvidia.com/Download/index.aspx?lang=en-us) or [AMD](https://www.amd.com/en/support) graphics card, use [DDU](https://www.guru3d.com/files-details/display-driver-uninstaller-download.html) to remove old drivers properly. DDU is **essential** because it allows you to fix crashes and performance issues on your games (recommended by Nvidia). Activate the [Message Signaled Interrupts](https://www.mediafire.com/file/ewpy1p0rr132thk/MSI_util_v3.zip) **only for graphics card** (enabled by default on AMD graphics card), it will have to be reactivated after each drivers update
* Update Windows via Windows Update in settings

Once everything is up to date and the PC has been restarted:
* Delete Windows Update files (`C:/Windows/SoftwareDistribution/Download/` - Delete all folders to avoid errors in future updates)
* Delete all temporary files (`Windows` + `R` - Type "%temp%" - Delete all)
* Repair system files: `sfc /scannow`
* Flush DNS cache: `ipconfig /flushdns`
* Repair Windows Image: `Dism /Online /Cleanup-Image /RestoreHealth`
* Reset icon cache with my [script](https://github.com/PouletEnSlip/ResetIconCache) to avoid white icons
* Clean all drives (Type "Disk Cleanup" in the Windows search bar - Run as administrator - Check all)
* Optimize all drives (Right click on a drive - Properties - Tools - Optimize)

> **Note** Remember to turn off your computer at night, do not put it to sleep to avoid bugs. Also regularly clean your PC of dust to avoid components from overheating and therefore losing performance

## Advanced practices
Uninstall a maximum of Windows applications and software that you do not use via the Control Panel. Do not use tools like Revo Uninstaller or CCleaner which can uninstall system apps like Edge or Store which will make the system unstable

Disable as many programs as possible that launch at Windows startup (`Ctrl` + `Maj` + `Esc` - Startup)

Disable Cortana: `REG ADD "HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\Windows Search" /v AllowCortana /t REG_DWORD /d 00000000 /f` - Restart PC | To cancel: `REG ADD "HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\Windows Search" /v AllowCortana /t REG_DWORD /d 00000001 /f`

Disable Widgets on Windows 11: `REG ADD "HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Dsh" /v AllowNewsAndInterests /t REG_DWORD /d 00000000 /f` - Restart PC | To cancel: `REG DELETE "HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Dsh" /v AllowNewsAndInterests /f`

Uncheck "Enhance pointer precision" 
to avoid mouse acceleration (Control Panel - Hardware - Mouse - Pointer Options)

Disable fast boot and hibernation to free up space (~3GB) and avoid bugs, with these **two** commands: `REG ADD "HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Session Manager\Power" /v HiberbootEnabled /t REG_DWORD /d 00000000 /f` + `powercfg -h off` - Restart PC | To cancel: `REG ADD "HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Session Manager\Power" /v HiberbootEnabled /t REG_DWORD /d 00000001 /f` + `powercfg -h on`
> **Note** Disabling fast boot will make your PC start up a bit longer, however your computer will completely shut down when you turn it off, which will make the system more stable and avoid bugs

Uncheck as many boxes as possible in the "Privacy" section in Windows settings to limit the collection of personal data by Microsoft (diagnostic data, location, contacts...)

Install **all** versions of [Visual C++](https://www.techpowerup.com/download/visual-c-redistributable-runtime-package-all-in-one/) to avoid missing DLL errors

Disable Xbox Game Bar if you don't use it, with these **three** commands: `Get-AppxPackage Microsoft.XboxGamingOverlay | Remove-AppxPackage` + `REG ADD "HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows\CurrentVersion\GameDVR" /v AppCaptureEnabled /t REG_DWORD /d 00000000 /f` + `REG ADD "HKEY_CURRENT_USER\System\GameConfigStore" /v GameDVR_Enabled /t REG_DWORD /d 00000000 /f` - Restart PC | To cancel: `Get-AppxPackage -allusers *Microsoft.XboxGamingOverlay* | Foreach {Add-AppxPackage -DisableDevelopmentMode -Register “$($_.InstallLocation)\AppXManifest.xml”}` + `REG ADD "HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows\CurrentVersion\GameDVR" /v AppCaptureEnabled /t REG_DWORD /d 00000001 /f` + `REG ADD "HKEY_CURRENT_USER\System\GameConfigStore" /v GameDVR_Enabled /t REG_DWORD /d 00000001 /f`

Disable Bing results in Windows Search: `REG ADD "HKEY_CURRENT_USER\SOFTWARE\Policies\Microsoft\Windows\Explorer" /v DisableSearchBoxSuggestions /t REG_DWORD /d 00000001 /f` - Restart PC | To cancel: `REG ADD "HKEY_CURRENT_USER\SOFTWARE\Policies\Microsoft\Windows\Explorer" /v DisableSearchBoxSuggestions /t REG_DWORD /d 00000000 /f`

Bring old Windows 10 context menu back on Windows 11: `REG ADD "HKCU\Software\Classes\CLSID\{86ca1aa0-34aa-4e8b-a509-50c905bae2a2}\InprocServer32" /f /ve` - Restart PC | To cancel: `REG DELETE "HKCU\Software\Classes\CLSID\{86ca1aa0-34aa-4e8b-a509-50c905bae2a2}" /f`

**Change power options in control panel:**
* Intel CPU: choose "High performance"
* AMD Ryzen 1000, 2000, 3000 and 4000 CPU: choose "AMD Ryzen Balanced"
* AMD Ryzen 5000, 6000, 7000 and + CPU: choose "Balanced"
* In advanced settings: shutdown hard drive after 0min (never) and disable USB selective suspend

**Nvidia/AMD panel changes:**
* Display: select the highest possible refresh rate, choose the highest possible output colour depth (10bpc or more), select "No scaling"
* 3D settings: select "Use the advanced 3D image settings", low latency mode to "On", prefer maximum performance, enable G-SYNC + V-SYNC + limit FPS to 2 below screen refresh rate to avoid screen tearing (144Hz screen → limit to 142FPS)
> **Warning** If you enable V-SYNC in the Nvidia/AMD panel, you have to disable it in all your games settings to avoid incompatibilities!
* Video: choose "full" dynamic range

**Overclock your graphic card:** overclocking allows you to increase the clock frequency of the graphics card and thus have more performance in the game. However, the temperature of the card may increase. Personally I use [Afterburner](https://www.msi.com/Landing/afterburner/graphics-cards) and [Kombustor](https://msikombustor.com/). I like Kombustor because it allows to scan the number of artifacts (you have to check the box on the menu and choose your native resolution). I consider an overclock to be stable if the temperature of the graphics card does not exceed 85°C (185F) and Kombustor detects **no** artifacts in at least 10 minutes. Then try on a very resource-intensive game to verify that the system is stable over time

## Optional
* Reinstall Windows (preferably Pro) completely (with a USB key) before applying these manipulations to start on a clean system
* Always keep Windows and its programs up-to-date for security, stability and compatibility reasons
* Use the Windows antivirus which does its job very well. Avoid Avast, Bitdefender...
* Install the extension [uBlock Origin](https://ublockorigin.com/) for blocking ads and trackers, avoid installing other extensions that could slow down the browser
* Configure your browser to block third-party cookies and use HTTPS only
* Use a custom DNS (DoH, in Windows settings) like [Quad9](https://www.quad9.net) or [Mullvad](https://mullvad.net/fr/help/dns-over-https-and-dns-over-tls/) rather than the local provider one for more security and privacy
* Enable BitLocker with TPM 2.0 on Windows Pro to encrypt drive data and secure your files (Right click on a drive - Enable BitLocker)
> **Warning** Be sure to back up the BitLocker recovery key to a cloud or an external drive!
* Use [OpenRGB](https://gitlab.com/CalcProgrammer1/OpenRGB) to control all your RGB components through a single software. Thus, we avoid software like Razer Synapse or MSI Dragon Center which use a lot of resources in the background

## Conclusion
That's it! Your PC should be faster and more efficient. I recommend reinstalling Windows every year, taking care to make backups. I advise against other manipulations which could damage the system (custom ISO, PowerShell scripts, Internet connection optimizer... these are scams).

### Sources
[Microsoft](https://learn.microsoft.com/en-us/windows/security/) | [Discord Entraide Informatique](https://discord.gg/WMsR7dT) | [Piwi](https://github.com/Piwielle) | [BlurBusters](https://blurbusters.com) | [PrivacyGuides](https://privacyguides.org/)
