# WindowsBestPractices

[en](/README.md), [fr](/README-FR.md)

Bonjour ! Voici des pratiques simples et saines pour un ordinateur fonctionnant sous Windows 10 ou 11. Ces manipulations permettent d'avoir un ordinateur plus performant pour la bureautique et les jeux vidéo. Ces manipulations sont sans risque et peuvent résoudre les lenteurs et crashs de votre ordinateur. Ces pratiques ne sont pas "magiques", je ne promets pas un gain incroyable, le plus efficace étant d'acheter de nouveaux composants plus performants. Lisez tout en entier avant de faire quoi que ce soit.

## Table des matières
- [Pratiques rapides](#🧹-pratiques-rapides)
- [Pratiques avancées](#🛠️-pratiques-avancées)
- [Optionnel](#💡-optionnel)
- [Conclusion](#🎉-conclusion)
- [Sources](#🔗-sources)

## 🧹 Pratiques rapides
Dans l'ordre, à répéter 1 fois par mois environ :
* Vérifier que vous n'avez pas de virus/malware avec [Malwarebytes](https://fr.malwarebytes.com/)
* Supprimer l'historique, le cache et les cookies de votre navigateur
* Mettre le BIOS et les drivers à jour **via le site de votre carte mère**. Éviter CCleaner, Driverscloud ou DriverBooster, ces utilitaires peuvent installer des drivers obsolètes ou non compatibles avec vos composants
* Mettre à jour les drivers de votre carte graphique [Nvidia](https://www.nvidia.fr/Download/index.aspx?lang=fr) ou [AMD](https://www.amd.com/fr/support), utiliser [DDU](https://www.guru3d.com/files-details/display-driver-uninstaller-download.html) pour supprimer les anciens divers proprement. DDU est **indispensable** car il permet de corriger les crashs et pertes de performance sur vos jeux (recommandé par Nvidia). Activer le [Message Signaled Interrupts](https://www.mediafire.com/file/ewpy1p0rr132thk/MSI_util_v3.zip) **uniquement pour la carte graphique** (activé par défaut sur les cartes graphiques AMD), il faudra le réactiver après chaque mise à jour des drivers
* Mettre Windows à jour via Windows Update dans les paramètres

Une fois que tout est bien à jour et que l'ordinateur a été redémarré :
* Supprimer les fichiers de Windows Update (`C:/Windows/SoftwareDistribution/Download/` - Supprimer tous les dossiers pour éviter des erreurs lors des prochaines mises à jour)
* Supprimer tous les fichiers temporaires (`Windows` + `R` - Taper "%temp%" - Tout supprimer)
* Réparer les fichiers système : `sfc /scannow`
* Vider le cache DNS : `ipconfig /flushdns`
* Réparer l’image de Windows : `Dism /Online /Cleanup-Image /RestoreHealth`
* Réinitialiser le cache des icônes avec mon [script](https://github.com/PouletEnSlip/ResetIconCache) pour éviter les icônes blanches
* Nettoyer tous les lecteurs (Taper "Nettoyage de disque" dans la barre de recherche Windows - Exécuter en tant qu'administrateur - Tout cocher)
* Optimiser tous les lecteurs (Clic droit sur un lecteur - Propriétés - Outils - Optimiser)

> **Note** Pensez à éteindre votre ordinateur la nuit, ne le mettez pas en veille pour prévenir les bugs. Nettoyer aussi régulièrement votre PC de la poussière pour éviter aux composants de trop chauffer et donc de perdre en performances

## 🛠️ Pratiques avancées
Désinstaller un maximum d'applications Windows et logiciels que vous n'utilisez pas via le Panneau de configuration. N'utilisez pas d'outils comme Revo Uninstaller ou CCleaner qui peuvent désinstaller des applications système comme Edge ou le Store, ce qui va rendre le système instable

Désactiver un maximum de programmes qui se lancent au démarrage de Windows (`Ctrl` + `Maj` + `Esc` - Démarrage)

Désactiver Cortana : `REG ADD "HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\Windows Search" /v AllowCortana /t REG_DWORD /d 00000000 /f` - Redémarrer le PC | Pour annuler : `REG ADD "HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\Windows Search" /v AllowCortana /t REG_DWORD /d 00000001 /f`

Désactiver les Widgets sur Windows 11 : `REG ADD "HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Dsh" /v AllowNewsAndInterests /t REG_DWORD /d 00000000 /f` - Redémarrer le PC | Pour annuler : `REG DELETE "HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Dsh" /v AllowNewsAndInterests /f`

Décocher "Améliorer la précision du pointeur" pour éviter l'accélération de la souris (Panneau de configuration - Matériels - Souris - Options du pointeur)

Désactiver le fast boot et la mise en veille prolongée pour libérer de la place sur le lecteur (~3Go) et prévenir les bugs, avec ces **deux** commandes : `REG ADD "HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Session Manager\Power" /v HiberbootEnabled /t REG_DWORD /d 00000000 /f` + `powercfg -h off` - Redémarrer le PC | Pour annuler : `REG ADD "HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Session Manager\Power" /v HiberbootEnabled /t REG_DWORD /d 00000001 /f` + `powercfg -h on`
> **Note** Désactiver le fast boot va rendre le démarrage de votre PC un peu plus long, cependant votre ordinateur s'arrêtera réellement quand vous l'éteignerez, ce qui rendra le système plus stable et évitera les bugs

Décocher un maximum de cases dans la section "Confidentialité" dans les paramètres Windows pour limiter la collecte de données personnelles par Microsoft (données de diagnostic, localisation, contacts...)

Installer **toutes** les versions de [Visual C++](https://www.techpowerup.com/download/visual-c-redistributable-runtime-package-all-in-one/) pour éviter les erreurs de DLLs manquantes

Désactiver la Xbox Game Bar si vous ne l'utilisez pas, avec ces **trois** commandes : `Get-AppxPackage Microsoft.XboxGamingOverlay | Remove-AppxPackage` + `REG ADD "HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows\CurrentVersion\GameDVR" /v AppCaptureEnabled /t REG_DWORD /d 00000000 /f` + `REG ADD "HKEY_CURRENT_USER\System\GameConfigStore" /v GameDVR_Enabled /t REG_DWORD /d 00000000 /f` - Redémarrer le PC | Pour annuler : `Get-AppxPackage -allusers *Microsoft.XboxGamingOverlay* | Foreach {Add-AppxPackage -DisableDevelopmentMode -Register “$($_.InstallLocation)\AppXManifest.xml”}` + `REG ADD "HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows\CurrentVersion\GameDVR" /v AppCaptureEnabled /t REG_DWORD /d 00000001 /f` + `REG ADD "HKEY_CURRENT_USER\System\GameConfigStore" /v GameDVR_Enabled /t REG_DWORD /d 00000001 /f`

Désactiver les résultats Bing dans la Recherche Windows : `REG ADD "HKEY_CURRENT_USER\SOFTWARE\Policies\Microsoft\Windows\Explorer" /v DisableSearchBoxSuggestions /t REG_DWORD /d 00000001 /f` - Redémarrer le PC | Pour annuler : `REG ADD "HKEY_CURRENT_USER\SOFTWARE\Policies\Microsoft\Windows\Explorer" /v DisableSearchBoxSuggestions /t REG_DWORD /d 00000000 /f`

Remettre l'ancien menu du clic droit de Windows 10 sur Windows 11 : `REG ADD "HKCU\Software\Classes\CLSID\{86ca1aa0-34aa-4e8b-a509-50c905bae2a2}\InprocServer32" /f /ve` - Redémarrer le PC | Pour annuler : `REG DELETE "HKCU\Software\Classes\CLSID\{86ca1aa0-34aa-4e8b-a509-50c905bae2a2}" /f`

**Modifier les options d’alimentation dans le panneau de configuration :**
* CPU Intel : choisir "Performances élevées"
* CPU AMD Ryzen 1000, 2000, 3000 et 4000 : choisir "AMD Ryzen Balanced"
* CPU AMD Ryzen 5000 et plus récent : choisir "Utilisation normale"
* Dans les paramètres avancés du mode : arrêter le disque dur après 0min (jamais) et désactiver la suspension sélective USB

**Modifications du panneau Nvidia/AMD :**
* Sélectionner la plus grande fréquence de rafraîchissement possible (144Hz, 180Hz...)
* Choisir la plus grande intensité/profondeur de couleur possible (8bpc, 10bpc...)
* Activer G-SYNC/FreeSync + V-SYNC + limiter les IPS à 2 en dessous de la fréquence de rafraîchissement de l’écran pour éviter les déchirures d’images (écran 144Hz → limite à 142FPS)
* Spécifique Nvidia : choisir la plage dynamique "complète" dans l'onglet couleurs vidéo, sélectionner "Utiliser les paramètres d’images 3D avancés", dans ces paramètres -> mode de faible latence sur "Activé", privilégier les performances maximales
* Spécifique AMD : activer Radeon Anti-Lag et Radeon Chill
> **Warning** Si vous activez la V-SYNC dans le panneau Nvidia/AMD, il faut la désactiver dans les paramètres de tous vos jeux pour éviter des conflits !

**Overclocker sa carte graphique :** l'overclocking permet d'augmenter la fréquence d'horloge de la carte graphique et ainsi avoir plus de performances en jeu. Cependant la température de la carte risque d'augmenter. Guide vidéo [ici](https://www.youtube.com/watch?v=64GJck-GWaM). Personnellement j'utilise [Afterburner](https://www.msi.com/Landing/afterburner/graphics-cards) et [Kombustor](https://msikombustor.com/). Kombustor permet de scanner le nombre d'artefacts (il faut cocher la case sur l'écran d'accueil et choisir la résolution de votre écran). Je considère qu'un overclocking est stable si la température de la carte graphique ne dépasse pas 85°C et que Kombustor ne détecte **aucun** artefact en minimum 10 minutes. Ensuite, essayer sur un jeu très gourmand en ressources pour vérifier que le système est stable sur la durée

**Undervolter sa carte graphique :** l'undervolting permet de baisser la tension que reçoit la carte graphique et ainsi avoir de plus basses températures. Guide vidéo [ici](https://www.youtube.com/watch?v=ABWhmciIpe4). Comme pour l'overclocking, j'utilise Afterburner et Kombustor

## 💡 Optionnel
* Réinstaller Windows (Pro de préférence) complètement (avec une clé USB) avant d’appliquer ces manipulations pour partir sur une base saine
* Toujours garder Windows et ses programmes à jour pour des raisons de sécurité, de stabilité et de compatibilité
* Utiliser l'antivirus de Windows qui fait très bien son travail. Éviter Avast, Bitdefender...
* Préférer [Firefox](https://www.mozilla.org/fr/firefox/new/) à Chrome pour des raisons de vie privée
* Installer l'extension [uBlock Origin](https://ublockorigin.com/) pour le blocage des publicités et pisteurs
* Configurer votre navigateur pour bloquer les cookies tiers et utiliser HTTPS uniquement
* Utiliser un DNS personnalisé (DoH, dans les paramètres Windows) comme [Quad9](https://www.quad9.net/fr/) ou [Mullvad](https://mullvad.net/fr/help/dns-over-https-and-dns-over-tls/) plutôt que celui du fournisseur local pour des raisons de sécurité et de vie privée
* Activer BitLocker avec TPM 2.0 sur Windows Pro pour chiffrer les données du lecteur et sécuriser vos fichiers (Clic droit sur un lecteur - Activer BitLocker)
> **Warning** Veuillez à bien sauvegarder la clé de récupération BitLocker dans un cloud ou un disque externe !
* Utiliser [OpenRGB](https://gitlab.com/CalcProgrammer1/OpenRGB) pour contrôler tous vos composants RGB via un seul logiciel. Ainsi, on évite les logiciels comme Razer Synapse, ASUS Aura ou MSI Dragon Center qui consomment énormément de ressources en arrière-plan

## 🎉 Conclusion
Voilà ! Votre PC devrait être plus rapide et performant. Je recommande une réinstallation de Windows tous les ans en prenant le soin de faire des sauvegardes. Je déconseille d'autres manipulations qui pourraient endommager le système (ISO custom, scripts PowerShell, optimiseur de connexion Internet... ce sont très souvent des arnaques).

### 🔗 Sources
[Microsoft](https://learn.microsoft.com/en-us/windows/security/) | [Discord Entraide Informatique](https://discord.gg/WMsR7dT) | [Piwi](https://github.com/Piwielle) | [BlurBusters](https://blurbusters.com/) | [PrivacyGuides](https://privacyguides.org/)
