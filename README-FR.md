# Optimiser Windows

[en](/README.md), [fr](/README-FR.md)

Bonjour ! Voici des optimisations simples et saines pour un PC fonctionnant sous Windows 10 et 11. Ces manipulations permettent d'avoir un ordinateur plus fluide et performant pour la bureautique, le montage ou les jeux vidéo. Ces manipulations sont sans risque et peuvent résoudre les lenteurs et crashs de votre PC. Votre ordinateur pourra également démarrer plus vite et lancera des tâches plus rapidement. Ces optimisations ne sont pas "magiques", je ne promets pas un gain incroyable, la principale optimisation étant d'acheter des composants plus performants. Lisez tout en entier avant de faire quoi que ce soit.

## Table des matières
- [Optimisations rapides](#optimisations-rapides)
- [Optimisations avancées](#optimisations-avancées)
- [Optionnel](#optionnel)
- [Conclusion](#conclusion)

## Optimisations rapides
Dans l'ordre, à répéter 1 fois par mois environ :
* Vérifier que vous n'avez pas de virus/malware avec [Malwarebytes](https://fr.malwarebytes.com/)
* Supprimer l'historique, le cache et les cookies du navigateur (il faudra se reconnecter aux sites)
* Mettre le BIOS et les drivers à jour via le site de votre carte mère. Évitez CCleaner, Driverscloud, DriverBooster, TousLesDrivers... ces utilitaires peuvent installer des drivers obsolètes ou non compatibles avec vos composants
* Mettre à jour les drivers de votre carte graphique [Nvidia](https://www.nvidia.fr/Download/index.aspx?lang=fr) ou [AMD](https://www.amd.com/fr/support), utiliser [DDU](https://www.guru3d.com/files-details/display-driver-uninstaller-download.html) et [NVCleanstall](https://www.techpowerup.com/download/techpowerup-nvcleanstall/) pour mettre à jour les drivers proprement. DDU est **indispensable** car il permet de corriger de nombreux bugs/crashs sur vos jeux, utilitaire recommandé par Nvidia eux-mêmes. NVCleanstall permet une installation minimale des drivers Nvidia, vous pouvez aussi cocher la case Message Signaled Interrupts **uniquement**, dans les tweaks experts, qui offre une communication du GPU plus efficace (paramètre activé par défaut sur les GPU AMD)
* Mettre Windows à jour via Windows Update dans les paramètres. Ne plus utiliser Windows 7, trop de failles de sécurité et des problèmes de compatibilité

Une fois que tout est bien à jour et que le PC a été redémarré :
* Supprimer les fichiers de Windows Update **après chaque mise à jour** (C:/Windows/SoftwareDistribution/Download/ - Supprimer tous les dossiers pour éviter des erreurs lors des prochaines mises à jour)
* Supprimer tous les fichiers temporaires (`Windows` + `R` - Taper %temp% - Tout supprimer)
* Réparer les fichiers système : `sfc /scannow`
* Vider le cache DNS : `ipconfig /flushdns`
* Réparer l’image de Windows : `Dism /Online /Cleanup-Image /RestoreHealth`
* Réinitialiser le cache des icônes avec mon [script](https://github.com/PouletEnSlip/ResetIconCache)
* Nettoyer tous les lecteurs (Clic droit sur un lecteur - Propriétés - Nettoyage - Nettoyer les fichiers système - Tout cocher)
* Optimiser tous les lecteurs (Clic droit sur un lecteur - Propriétés - Outils - Optimiser)

Pensez à éteindre votre ordinateur la nuit, ne le mettez pas en veille pour prévenir les bugs. Nettoyer aussi régulièrement votre PC de la poussière pour éviter aux composants de trop chauffer et donc de perdre des performances.

## Optimisations avancées
Désinstaller un maximum d'applications Windows et logiciels que vous n'utilisez pas (Menu démarrer - Clic droit sur un programme - Désinstaller)

Désactiver un maximum de programmes qui se lancent au démarrage de Windows (`Ctrl` + `Maj` + `Esc` - Démarrage)

Désactiver Cortana : `REG ADD "HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\Windows Search" /v AllowCortana /t REG_DWORD /d 00000000 /f` - Redémarrer le PC | Pour annuler : `REG ADD "HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\Windows Search" /v AllowCortana /t REG_DWORD /d 00000001 /f`

Désactiver les Widgets sur Windows 11 : `REG ADD "HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Dsh" /v AllowNewsAndInterests /t REG_DWORD /d 00000000 /f` - Redémarrer le PC | Pour annuler : `REG DELETE "HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Dsh" /v AllowNewsAndInterests /f`

Désactiver l’amélioration de la précision du pointeur de la souris pour être plus précis (Panneau de configuration - Matériels - Souris - Options du pointeur)

Désactiver la mise en veille prolongée (hibernation) pour libérer de la place sur le lecteur (~3Go) et faire en sorte que le PC s'éteigne complètement quand vous l'éteignez, avec ces deux commandes : `REG ADD "HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Session Manager\Power" /v HiberbootEnabled /t REG_DWORD /d 00000000 /f` + `powercfg -h off` - Redémarrer le PC | Pour annuler : `REG ADD "HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Session Manager\Power" /v HiberbootEnabled /t REG_DWORD /d 00000001 /f` + `powercfg -h on`

Réduire les ressources du processeur réservées pour certains processus Windows : `REG ADD "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\WindowsNT\CurrentVersion\Multimedia\SystemProfile" /v SystemResponsiveness /t REG_DWORD /d 00000010 /f` - Redémarrer le PC | Pour annuler : `REG ADD "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\WindowsNT\CurrentVersion\Multimedia\SystemProfile" /v SystemResponsiveness /t REG_DWORD /d 00000020 /f`

Décocher **toutes les cases de chaque onglet** de la section "Confidentialité" dans les paramètres Windows pour limiter la collecte de données personnelles par Microsoft (localisation, contacts...)

Installer **toutes** les versions de [Visual C++](https://www.techpowerup.com/download/visual-c-redistributable-runtime-package-all-in-one/) pour éviter les erreurs de DLLs manquantes

Désactiver la Xbox Game Bar, avec ces trois commandes : `Get-AppxPackage Microsoft.XboxGamingOverlay | Remove-AppxPackage` + `REG ADD "HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows\CurrentVersion\GameDVR" /v AppCaptureEnabled /t REG_DWORD /d 00000000 /f` + `REG ADD "HKEY_CURRENT_USER\System\GameConfigStore" /v GameDVR_Enabled /t REG_DWORD /d 00000000 /f` - Redémarrer le PC | Pour annuler : `Get-AppxPackage -allusers *Microsoft.XboxGamingOverlay* | Foreach {Add-AppxPackage -DisableDevelopmentMode -Register “$($_.InstallLocation)\AppXManifest.xml”}` + `REG ADD "HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows\CurrentVersion\GameDVR" /v AppCaptureEnabled /t REG_DWORD /d 00000001 /f` + `REG ADD "HKEY_CURRENT_USER\System\GameConfigStore" /v GameDVR_Enabled /t REG_DWORD /d 00000001 /f`

Désactiver les résultats Bing dans la Recherche Windows : `REG ADD "HKEY_CURRENT_USER\SOFTWARE\Policies\Microsoft\Windows\Explorer" /v DisableSearchBoxSuggestions /t REG_DWORD /d 00000001 /f` - Redémarrer le PC | Pour annuler : `REG ADD "HKEY_CURRENT_USER\SOFTWARE\Policies\Microsoft\Windows\Explorer" /v DisableSearchBoxSuggestions /t REG_DWORD /d 00000000 /f`

Remettre l'ancien menu du clic droit de Windows 10 sur Windows 11 : `REG ADD "HKCU\Software\Classes\CLSID\{86ca1aa0-34aa-4e8b-a509-50c905bae2a2}\InprocServer32" /f /ve` - Redémarrer le PC | Pour annuler : `REG DELETE "HKCU\Software\Classes\CLSID\{86ca1aa0-34aa-4e8b-a509-50c905bae2a2}" /f`

Désactiver les services "Sysmain" et "Windows Search" si Windows est installé sur un SSD (`Windows` + `R` - Taper "services.msc")

**Modifier les options d’alimentation dans le panneau de configuration :** CPU Intel → choisir "Performances élevées" | CPU AMD Ryzen 1000, 2000, 3000 et 4000 → choisir "AMD Ryzen Balanced" | CPU AMD Ryzen 5000 → choisir "Utilisation normale". Dans les paramètres avancés : arrêter le disque dur après 0min (jamais) et désactiver la suspension sélective USB

**Modifications du panneau Nvidia (options AMD similaires) :** sélectionner "Utiliser les paramètres d’images 3D avancés", mode de faible latence sur "On", privilégier les performances maximales, activer G-SYNC + V-SYNC + limiter les IPS à 2 en dessous de la fréquence de rafraîchissement de l’écran pour éviter les déchirures d’images (écran 144Hz → limite à 142FPS). Si vous activez la V-SYNC dans le panneau Nvidia, il faut la désactiver dans les paramètres du jeu. Choisir la plage dynamique complète et choisir 10bpc (ou plus) si possible. Ces paramètres sont les meilleurs pour quelqu'un qui recherche la meilleure qualité d'image possible avec un impact négligeable sur la latence

**Overclocker sa carte graphique (Nvidia et AMD) :** l'overclocking permet d'augmenter la fréquence d'horloge de la carte graphique et ainsi avoir plus de performances en jeu. Cependant la température de la carte risque d'augmenter. Personnellement j'utilise [Afterburner](https://www.msi.com/Landing/afterburner/graphics-cards) et [Kombustor](https://msikombustor.com/). Dans [ce tuto](https://www.youtube.com/watch?v=64GJck-GWaM), Unigine Heaven est utilisé, je préfère Kombustor car il permet de scanner le nombre d'artefacts (il faut cocher la case sur l'écran d'accueil et choisir votre résolution native). En effet, un overclocking peut être stable sur Unigine Heaven mais l'ordinateur peut planter sur un jeu plus gourmand en ressources. Je considère qu'un overclocking est stable si la température de la carte ne dépasse pas 85°C et que Kombustor ne détecte **aucun** artefact en 10 minutes

## Optionnel
* [Réinstaller Windows](https://www.youtube.com/watch?v=uHOP4UbEGug) complètement (via une clé USB) avant d’appliquer ces optimisations pour partir sur une base saine
* Préférer [Firefox](https://www.mozilla.org/fr/firefox/new/) ou [Brave](https://brave.com/fr/) à Chrome pour le respect de la vie privée, plus d'info [ici](https://privacytests.org/)
* Extension [uBlock Origin](https://ublockorigin.com/fr) recommandée pour le blocage des publicités et pisteurs (je déconseille Adblock qui ne bloque pas toutes les pubs et aucun pisteur, plus d'info [ici](https://adblock-tester.com/))
* Activer Bitlocker sur la version Pro de Windows pour chiffrer les données du lecteur et sécuriser vos fichiers (Clic droit sur un lecteur - Activer Bitlocker). Il est possible d'activer Bitlocker sur la version Famille de Windows, à certaines conditions, tuto disponible [ici](https://lecrabeinfo.net/activer-le-chiffrement-de-lappareil-bitlocker-sur-windows-10-famille.html)
* Désinstaller le lecteur Windows Media et installer [VLC](https://www.videolan.org/) pour de meilleures performances
* Installer [7-Zip](https://www.7-zip.org/) pour compresser les fichiers et pouvoir chiffrer les archives. Je déconseille WinRAR qui n'est pas open source et est payant
* Utiliser [Hat.sh](https://hat.sh/) pour chiffrer vos documents/archives sensibles avec un mot de passe
* Si vous souhaitez un VPN gratuit, [ProtonVPN](https://protonvpn.com/download) est le meilleur choix. Evitez tous les autres VPN gratuits qui peuvent vendre vos données personnelles
* Utiliser [OpenRGB](https://gitlab.com/CalcProgrammer1/OpenRGB) pour contrôler tous vos composants RGB via un seul logiciel. Ainsi, on évite les logiciels comme Razer Synapse ou Dragon Center qui consomment énormément de ressources en arrière-plan
* Installer [EqualizerAPO](https://sourceforge.net/projects/equalizerapo/) pour améliorer le rendu sonore, booster les basses et les aigus. Il faut choisir SFX/EFX lors de l'installation. En cas de mise à jour des drivers de la carte son, il faudra reconfigurer EqualizerAPO. Ce logiciel n'a quasiment aucun impact sur les performances

## Conclusion
Voilà ! Votre PC devrait être plus rapide et performant. Je recommande une réinstallation de Windows tous les ans en prenant le soin de faire des sauvegardes. Je déconseille d'autres manipulations venant d'autres sites qui pourraient endommager le système (ISO custom, scripts PowerShell, optimiseur de connexion Internet... ce sont très souvent des arnaques).

### Sources
[Discord Entraide Informatique](https://discord.gg/WMsR7dT) | [Piwi](https://github.com/Piwielle) | [BlurBusters](https://blurbusters.com/) | [PrivacyGuides](https://privacyguides.org/)

*Mise à jour : 23/09/2022*
