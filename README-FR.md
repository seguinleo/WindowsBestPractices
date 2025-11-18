# WindowsBestPractices

[en](/README.md), [fr](/README-FR.md)

Voici des manipulations simples et saines pour un ordinateur fonctionnant sous Windows 10 ou 11. Elles permettent d'avoir un ordinateur plus réactif pour la bureautique et les jeux vidéo. Ce guide est efficace pour résoudre les bugs, les lenteurs et les crashs de votre ordinateur. Ces pratiques ne sont pas "magiques", je ne promets pas un gain incroyable niveau performances, le plus efficace restant d'acheter des composants plus performants.

*Bien que le guide soit sans réel risque, lire tout en entier avant de faire quoi que ce soit.*

## 📖 Sommaire
- [Pratiques rapides](#pratiques-rapides)
- [Pratiques avancées](#pratiques-avancées)
- [Conseils](#conseils)
- [Conclusion](#conclusion)
- [Sources](#sources)

## 🧹Pratiques rapides
Dans l'ordre, à répéter 1 fois par mois environ :

* Mettre à jour les drivers de votre carte graphique [Nvidia](https://www.nvidia.fr/Download/index.aspx?lang=fr) ou [AMD](https://www.amd.com/fr/support), utiliser [DDU](https://www.guru3d.com/files-details/display-driver-uninstaller-download.html) pour supprimer les anciens drivers proprement
> [!IMPORTANT]
> DDU est **indispensable** car il permet de corriger les crashs et pertes de performance sur vos jeux

* Mettre Windows à jour via Windows Update dans les paramètres

Une fois que tout est bien à jour et que l'ordinateur a été redémarré :

* Réparer les fichiers système : `sfc /scannow`
* Réparer l'image de Windows : `Dism /Online /Cleanup-Image /RestoreHealth`
* En cas d'icônes blanches sur le bureau, réinitialiser le cache des icônes avec mon [script](https://github.com/seguinleo/ResetIconCache)
* En cas d'erreur pendant une mise à jour, supprimer les fichiers de Windows Update (`C:/Windows/SoftwareDistribution/Download/` - Supprimer tous les dossiers à l'intérieur)
* Nettoyer tous les lecteurs (Taper "Nettoyage de disque" dans la barre de recherche Windows - Exécuter en tant qu'administrateur)

## 🔧Pratiques avancées
Avant toute chose, je recommande l'édition Pro de Windows pour plus de contrôle sur le système

Mettre le BIOS et les drivers à jour **via le site de votre carte mère**. Éviter CCleaner, Driverscloud ou DriverBooster, ces utilitaires peuvent installer des drivers obsolètes ou incompatibles avec vos composants. Dans le BIOS, vérifier que le mode UEFI est activé et pour un usage gaming, vérifier aussi que le profil ram XMP est activé

Installer uniquement des programmes essentiels pour éviter les ralentissements et les bugs du système

Désactiver un maximum de programmes qui se lancent au démarrage de Windows (`Ctrl` + `Maj` + `Esc` - Démarrage)

Désactiver les Widgets sur Windows 11 : `REG ADD "HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Dsh" /v AllowNewsAndInterests /t REG_DWORD /d 00000000 /f` - Redémarrer le PC | Pour annuler : `REG DELETE "HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Dsh" /v AllowNewsAndInterests /f`

Désactiver Windows Copilot: `REG ADD "HKEY_CURRENT_USER\Software\Policies\Microsoft\Windows\WindowsCopilot" /v TurnOffWindowsCopilot /t REG_DWORD /d 1 /f`

Désactiver le service Sysmain qui peut causer des problèmes de performances sur le disque : `sc stop "SysMain" & sc config "SysMain" start=disabled` | Pour annuler : `sc config "SysMain" start=auto & sc start "SysMain"`

Décocher "Améliorer la précision du pointeur" pour désactiver l'accélération de la souris (Panneau de configuration - Matériels - Souris - Options du pointeur)

Désactiver le fast boot et la mise en veille prolongée pour libérer de la place sur le lecteur (~3Go) et prévenir des potentiels bugs, avec ces **deux** commandes : `REG ADD "HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Session Manager\Power" /v HiberbootEnabled /t REG_DWORD /d 00000000 /f` + `powercfg -h off` - Redémarrer le PC | Pour annuler : `REG ADD "HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Session Manager\Power" /v HiberbootEnabled /t REG_DWORD /d 00000001 /f` + `powercfg -h on`

> [!NOTE]
> Désactiver le fast boot va rendre le démarrage de votre PC un petit peu plus long (1-2s), cependant votre ordinateur s'arrêtera réellement, ce qui rendra le système plus stable.

Décocher un maximum d'options dans la section "Confidentialité" dans les paramètres Windows pour limiter la collecte de données personnelles par Microsoft (ID publicitaire, données de diagnostic, localisation, contacts...)

Installer **toutes** les versions de [Visual C++](https://www.techpowerup.com/download/visual-c-redistributable-runtime-package-all-in-one/) pour éviter les erreurs de DLLs manquantes

Désactiver l'enregistrement permanent de la Xbox Game Bar qui prend des ressources an arrière-plan, avec ces **deux** commandes : `REG ADD "HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows\CurrentVersion\GameDVR" /v AppCaptureEnabled /t REG_DWORD /d 00000000 /f` + `REG ADD "HKEY_CURRENT_USER\System\GameConfigStore" /v GameDVR_Enabled /t REG_DWORD /d 00000000 /f` - Redémarrer le PC | Pour annuler : `REG ADD "HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows\CurrentVersion\GameDVR" /v AppCaptureEnabled /t REG_DWORD /d 00000001 /f` + `REG ADD "HKEY_CURRENT_USER\System\GameConfigStore" /v GameDVR_Enabled /t REG_DWORD /d 00000001 /f`

Désactiver les résultats Bing dans la Recherche Windows : `REG ADD "HKEY_CURRENT_USER\SOFTWARE\Policies\Microsoft\Windows\Explorer" /v DisableSearchBoxSuggestions /t REG_DWORD /d 00000001 /f` - Redémarrer le PC | Pour annuler : `REG ADD "HKEY_CURRENT_USER\SOFTWARE\Policies\Microsoft\Windows\Explorer" /v DisableSearchBoxSuggestions /t REG_DWORD /d 00000000 /f`

Remettre l'ancien menu du clic droit de Windows 10 sur Windows 11 : `REG ADD "HKCU\Software\Classes\CLSID\{86ca1aa0-34aa-4e8b-a509-50c905bae2a2}\InprocServer32" /f /ve` - Redémarrer le PC | Pour annuler : `REG DELETE "HKCU\Software\Classes\CLSID\{86ca1aa0-34aa-4e8b-a509-50c905bae2a2}" /f`

Pour une utilisation gaming, Microsoft recommande de désactiver l'intégrité de la mémoire et la plateforme de machines virtuelles [ici](https://www.microsoft.com/en-us/windows/learning-center/optimize-pc-for-gaming-performance)

**Microsoft Edge :**
* Je déconseille l'utilisation de Microsoft Edge pour des raisons de vie privée, préférer Firefox
* Désactiver le démarrage rapide et l'exécution en arrière-plan dans l'onglet "Système et performances"
* Désactiver le partage de données avec d'autres fonctionnalités Windows dans l'onglet "Profils"
* Sélectionner Protection contre le suivi "Strict" dans l'onglet "Confidentialité, recherche et services"
* Bloquer les cookies tiers et le préchargement dans l'onglet "Cookies et autres données de site"

**Options d'alimentation dans le Panneau de configuration :**
* CPU Intel : choisir "Performances élevées"
* CPU AMD Ryzen 1000, 2000, 3000 et 4000 : choisir "AMD Ryzen Balanced"
* CPU AMD Ryzen 5000 et plus récent : choisir "Utilisation normale"
* Dans les paramètres avancés du mode : désactiver la suspension sélective USB

**Panneau Nvidia/AMD :**
* Sélectionner la plus grande fréquence de rafraîchissement possible (144Hz, 180Hz...)
* Activer G-SYNC/FreeSync + V-SYNC + bloquer les IPS à 3 en dessous de la fréquence de rafraîchissement de l'écran pour éviter les déchirures d'images (écran 144Hz → bloquer à 141FPS)
* Spécifique Nvidia : choisir mode de faible latence sur "Activé"
* Spécifique AMD : il est préférable de bloquer les FPS via [RTSS](https://www.guru3d.com/files-details/rtss-rivatuner-statistics-server-download.html) plutôt que via le panneau AMD pour une latence plus basse

> [!IMPORTANT]
> Si la V-SYNC est activée dans le panneau Nvidia/AMD, il faut la désactiver dans les paramètres de tous les jeux pour éviter des conflits.

> [!NOTE]
> Pour les jeux qui permettent de bloquer les FPS, il est préférable de le faire dans les paramètres du jeu plutôt que dans le panneau Nvidia/RTSS pour une latence plus basse.

Utiliser [MPO-GPU-FIX](https://github.com/RedDot-3ND7355/MPO-GPU-FIX) pour désactiver le MPO (Multi-Plane Overlay) qui peut causer des problèmes de performances et de stabilité dans les jeux

Pour aller plus loin et pour un usage gaming uniquement, vous pouvez penser à overclocker/undervolter votre GPU, mais renseignez-vous et soyez sûr de ce que vous faites. Personnellement j'utilise [MSI Afterburner](https://www.msi.com/Landing/afterburner/graphics-cards) et [Kombustor](https://msikombustor.com/) pour tester la stabilité de mon système. Je considère qu'un GPU semble stable si sa température ne dépasse pas 85°C et que Kombustor ne détecte **aucun** artefact en au moins 10 minutes

## 💡Conseils
* Réinstaller Windows avant d'appliquer ces manipulations pour avoir une base saine :
    * avec une clé USB pour une réinstallation complète du système (faire une sauvegarde avant)
    * ou via [un fichier ISO](https://www.microsoft.com/fr-fr/software-download/) pour garder tous ses documents/paramètres

> [!NOTE]
> La réinstallation est très efficace pour corriger les bugs/crashs. Une fois finie, ne pas se connecter à son compte Microsoft, créer un compte local pour limiter la collecte de données

* Utiliser l'antivirus de Windows qui fait très bien son travail. Éviter Avast, Bitdefender...
* Toujours garder Windows et ses programmes à jour pour des raisons de sécurité et de stabilité, notamment le navigateur
* Préférer [Firefox](https://www.mozilla.org/fr/firefox/new/) à Google Chrome pour des raisons de vie privée, configurer le pour bloquer les cookies tiers et utiliser HTTPS uniquement. Installer l'extension [uBlock Origin](https://ublockorigin.com/) (avec [ce filtre supplémentaire](https://raw.githubusercontent.com/DandelionSprout/adfilt/master/LegitimateURLShortener.txt)) pour le blocage des publicités et pisteurs. Éviter tout autre adblock et essayer de limiter le nombre d'extensions installées. Ne jamais enregistrer vos mots de passe dans le navigateur, utilisez un gestionnaire comme [Bitwarden](https://bitwarden.com/fr-fr/)
* Utiliser un [DNS](https://www.privacyguides.org/fr/dns/) personnalisé (DoH, dans les paramètres Windows) plutôt que celui de votre fournisseur local
* Un bon VPN gratuit que je recommande est [ProtonVPN](https://protonvpn.com/fr) ou un payant comme [Mullvad](https://mullvad.net/fr)
* Activer BitLocker sur votre PC portable pour chiffrer les données du lecteur et sécuriser vos fichiers (Clic droit sur un lecteur - Activer BitLocker)
> [!WARNING]
> Veuillez à bien sauvegarder la clé de récupération BitLocker dans un cloud ou un disque externe !

* Éteindre l'ordinateur la nuit, ne pas le mettre en veille pour prévenir les bugs. Nettoyer aussi régulièrement le PC de la poussière pour éviter aux composants de trop chauffer et donc de perdre en performances
* Activer l'éclairage nocturne de Windows pour éviter la fatigue oculaire
* Utiliser [OpenRGB](https://gitlab.com/CalcProgrammer1/OpenRGB) pour contrôler tous vos composants RGB via un seul logiciel. Ainsi, on évite les logiciels comme Razer Synapse, ASUS Aura ou MSI Dragon Center qui consomment des ressources en arrière-plan

## 🎉Conclusion
Voilà ! Votre PC devrait être plus rapide et performant. Je déconseille d'autres manipulations qui pourraient endommager le système (ISO custom, scripts PowerShell, optimiseur de connexion Internet... ce sont très souvent des arnaques ou du placebo).

## 🔗Sources
[Microsoft](https://learn.microsoft.com/en-us/windows/security/) | [Discord Entraide Informatique](https://discord.gg/WMsR7dT) | [Piwi](https://www.youtube.com/c/Piwi_youtube) | [BlurBusters](https://blurbusters.com/) | [PrivacyGuides](https://privacyguides.org/) | [Reddit](https://www.reddit.com/r/Windows11/)
