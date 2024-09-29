# WindowsBestPractices

[en](/README.md), [fr](/README-FR.md)

Bonjour ! Voici des manipulations simples et saines pour un ordinateur fonctionnant sous Windows 10 ou 11. Elles permettent d'avoir un ordinateur plus réactif pour la bureautique et les jeux vidéo. Ces manipulations sont sans risque et peuvent résoudre les lenteurs et crashs de votre ordinateur. Ces pratiques ne sont pas "magiques", je ne promets pas un gain incroyable, le plus efficace étant d'acheter de nouveaux composants plus performants. Lire tout en entier avant de faire quoi que ce soit.

## 📖 Sommaire
- [Pratiques rapides](#pratiques-rapides)
- [Pratiques avancées](#pratiques-avancées)
- [Conseils](#conseils)
- [Conclusion](#conclusion)
- [Sources](#sources)

## 🧹Pratiques rapides
### Dans l'ordre, à répéter 1 fois par mois environ
* Mettre à jour les drivers de votre carte graphique [Nvidia](https://www.nvidia.fr/Download/index.aspx?lang=fr) ou [AMD](https://www.amd.com/fr/support), utiliser [DDU](https://www.guru3d.com/files-details/display-driver-uninstaller-download.html) pour supprimer les anciens divers proprement. DDU est **indispensable** car il permet de corriger les crashs et pertes de performance sur vos jeux
* Mettre Windows à jour via Windows Update dans les paramètres

Une fois que tout est bien à jour et que l'ordinateur a été redémarré

* Supprimer l'historique, le cache et les cookies du navigateur
* Supprimer les fichiers de Windows Update (`C:/Windows/SoftwareDistribution/Download/` - Supprimer tous les dossiers à l'intérieur pour éviter des erreurs lors des prochaines mises à jour)
* Supprimer tous les fichiers temporaires (`Windows` + `R` - Taper "%temp%" - Tout supprimer)
* Réparer les fichiers système : `sfc /scannow`
* Vider le cache DNS : `ipconfig /flushdns`
* Réparer l'image de Windows : `Dism /Online /Cleanup-Image /RestoreHealth`
* Réinitialiser le cache des icônes avec mon [script](https://github.com/PouletEnSlip/ResetIconCache) pour éviter les icônes blanches
* Nettoyer tous les lecteurs (Taper "Nettoyage de disque" dans la barre de recherche Windows - Exécuter en tant qu'administrateur - Tout cocher)
* Optimiser tous les lecteurs (Clic droit sur un lecteur - Propriétés - Outils - Optimiser)

## 🔧Pratiques avancées
Avant toute chose, je recommande l'édition Pro de Windows pour plus de fonctionnalités et de contrôle sur le système

Mettre le BIOS et les drivers à jour **via le site de votre carte mère**. Éviter CCleaner, Driverscloud ou DriverBooster, ces utilitaires peuvent installer des drivers obsolètes ou non compatibles avec vos composants

Installer uniquement les programmes essentiels pour éviter les ralentissements et les bugs du système

Désactiver un maximum de programmes qui se lancent au démarrage de Windows (`Ctrl` + `Maj` + `Esc` - Démarrage)

Désactiver les Widgets sur Windows 11 : `REG ADD "HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Dsh" /v AllowNewsAndInterests /t REG_DWORD /d 00000000 /f` - Redémarrer le PC | Pour annuler : `REG DELETE "HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Dsh" /v AllowNewsAndInterests /f`

Désactiver le service Sysmain qui peut causer des problèmes de performances sur le disque : `sc stop "SysMain" & sc config "SysMain" start=disabled` | Pour annuler : `sc config "SysMain" start=auto & sc start "SysMain"`

Décocher "Améliorer la précision du pointeur" pour désactiver l'accélération de la souris (Panneau de configuration - Matériels - Souris - Options du pointeur)

Désactiver le fast boot et la mise en veille prolongée pour libérer de la place sur le lecteur (~3Go) et prévenir des potentiels bugs, avec ces **deux** commandes : `REG ADD "HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Session Manager\Power" /v HiberbootEnabled /t REG_DWORD /d 00000000 /f` + `powercfg -h off` - Redémarrer le PC | Pour annuler : `REG ADD "HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Session Manager\Power" /v HiberbootEnabled /t REG_DWORD /d 00000001 /f` + `powercfg -h on`

> [!NOTE]
> Désactiver le fast boot va rendre le démarrage de votre PC un petit peu plus long (1-2s), cependant votre ordinateur s'arrêtera réellement, ce qui rendra le système plus stable

Décocher un maximum d'options dans la section "Confidentialité" dans les paramètres Windows pour limiter la collecte de données personnelles par Microsoft (données de diagnostic, localisation, contacts...)

Installer **toutes** les versions de [Visual C++](https://www.techpowerup.com/download/visual-c-redistributable-runtime-package-all-in-one/) pour éviter les erreurs de DLLs manquantes

Désactiver l'enregistrement permanent de la Xbox Game Bar qui prend des ressources an arrière-plan, avec ces **deux** commandes : `REG ADD "HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows\CurrentVersion\GameDVR" /v AppCaptureEnabled /t REG_DWORD /d 00000000 /f` + `REG ADD "HKEY_CURRENT_USER\System\GameConfigStore" /v GameDVR_Enabled /t REG_DWORD /d 00000000 /f` - Redémarrer le PC | Pour annuler : `REG ADD "HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows\CurrentVersion\GameDVR" /v AppCaptureEnabled /t REG_DWORD /d 00000001 /f` + `REG ADD "HKEY_CURRENT_USER\System\GameConfigStore" /v GameDVR_Enabled /t REG_DWORD /d 00000001 /f`

Désactiver les résultats Bing dans la Recherche Windows : `REG ADD "HKEY_CURRENT_USER\SOFTWARE\Policies\Microsoft\Windows\Explorer" /v DisableSearchBoxSuggestions /t REG_DWORD /d 00000001 /f` - Redémarrer le PC | Pour annuler : `REG ADD "HKEY_CURRENT_USER\SOFTWARE\Policies\Microsoft\Windows\Explorer" /v DisableSearchBoxSuggestions /t REG_DWORD /d 00000000 /f`

Remettre l'ancien menu du clic droit de Windows 10 sur Windows 11 : `REG ADD "HKCU\Software\Classes\CLSID\{86ca1aa0-34aa-4e8b-a509-50c905bae2a2}\InprocServer32" /f /ve` - Redémarrer le PC | Pour annuler : `REG DELETE "HKCU\Software\Classes\CLSID\{86ca1aa0-34aa-4e8b-a509-50c905bae2a2}" /f`

Pour une utilisation gaming, Microsoft recommande de désactiver l'intégrité de la mémoire et la plateforme de machines virtuelles [ici](https://support.microsoft.com/fr-fr/windows/options-pour-optimiser-les-performances-des-jeux-dans-windows-11-a255f612-2949-4373-a566-ff6f3f474613)

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
* Choisir la plus grande intensité/profondeur de couleur possible (8bpc, 10bpc...)
* Activer G-SYNC/FreeSync + V-SYNC + bloquer les IPS à 3 en dessous de la fréquence de rafraîchissement de l'écran pour éviter les déchirures d'images (écran 144Hz → bloquer à 141FPS)
* Spécifique Nvidia : choisir la plage dynamique "complète" dans l'onglet couleurs vidéo, choisir mode de faible latence sur "Activé" et privilégier les performances maximales dans les paramètres d'images 3D avancés
* Spécifique AMD : il est préférable de bloquer les FPS via [RTSS](https://www.guru3d.com/files-details/rtss-rivatuner-statistics-server-download.html) plutôt que via le panneau AMD pour une latence plus basse

> [!IMPORTANT]
> Si la V-SYNC est activée dans le panneau Nvidia/AMD, il faut la désactiver dans les paramètres de tous les jeux pour éviter des conflits

> [!NOTE]
> Pour les jeux qui permettent de bloquer les FPS, il est préférable de le faire dans les paramètres du jeu plutôt que dans le panneau Nvidia/RTSS pour une latence plus basse

Utiliser [MPO-GPU-FIX](https://github.com/RedDot-3ND7355/MPO-GPU-FIX) pour désactiver le MPO (Multi-Plane Overlay) qui peut causer des problèmes de performances et de stabilité dans les jeux

## 💡Conseils
* Réinstaller Windows complètement (avec une clé USB, pas via les paramètres) avant d'appliquer ces manipulations pour partir sur une base saine. Lors de l'installation de Windows, ne pas se connecter à son compte Microsoft, créer un compte local pour limiter la collecte de données
* Si vous pensez avoir un virus, installez [Malwarebytes](https://downloads.malwarebytes.com/file/mb4_offline) et effectuez un scan pour supprimer les menaces. Cependant, le plus efficace est de réinstaller Windows comme ci-dessus
* Utiliser l'antivirus de Windows qui fait très bien son travail. Éviter Avast, Bitdefender...
* Toujours garder Windows et ses programmes à jour pour des raisons de sécurité et de stabilité, notamment le navigateur
* Préférer [Firefox](https://www.mozilla.org/fr/firefox/new/) à Google Chrome pour des raisons de vie privée, configurer le pour bloquer les cookies tiers et utiliser HTTPS uniquement
* Installer l'extension [uBlock Origin](https://ublockorigin.com/) pour le blocage des publicités et pisteurs. Éviter tout autre adblock et essayer de limiter le nombre d'extensions installées
* Utiliser un DNS personnalisé (DoH, dans les paramètres Windows) comme [Quad9](https://www.quad9.net/fr/) ou [Mullvad](https://mullvad.net/fr/help/dns-over-https-and-dns-over-tls/) plutôt que celui du fournisseur local pour des raisons de sécurité et de vie privée
* Un bon VPN gratuit que je recommande est [ProtonVPN](https://protonvpn.com/fr) pour des raisons de vie privée. Ou un VPN payant comme [Mullvad](https://mullvad.net/fr) pour les mêmes raisons
* Activer BitLocker sur votre PC portable pour chiffrer les données du lecteur et sécuriser vos fichiers (Clic droit sur un lecteur - Activer BitLocker)
> [!WARNING]
> Veuillez à bien sauvegarder la clé de récupération BitLocker dans un cloud ou un disque externe !
* Éteindre l'ordinateur la nuit, ne pas le mettre en veille pour prévenir les bugs. Nettoyer aussi régulièrement le PC de la poussière pour éviter aux composants de trop chauffer et donc de perdre en performances
* Activer l'éclairage nocturne le soir pour éviter la fatigue oculaire
* Utiliser [OpenRGB](https://gitlab.com/CalcProgrammer1/OpenRGB) pour contrôler tous vos composants RGB via un seul logiciel. Ainsi, on évite les logiciels comme Razer Synapse, ASUS Aura ou MSI Dragon Center qui consomment des ressources en arrière-plan
* Pour aller plus loin, vous pouvez penser à overclocker et undervolter votre GPU, mais soyez sûr de ce que vous faites. Personnellement j'utilise [MSI Afterburner](https://www.msi.com/Landing/afterburner/graphics-cards) et [Kombustor](https://msikombustor.com/) pour tester la stabilité de mon système. Je considère qu'un GPU semble stable si sa température ne dépasse pas 85°C et que Kombustor ne détecte **aucun** artefact en au moins 10 minutes

## 🎉Conclusion
Voilà ! Votre PC devrait être plus rapide et performant. Je recommande une réinstallation de Windows tous les ans en prenant le soin de faire des sauvegardes. Je déconseille d'autres manipulations qui pourraient endommager le système (ISO custom, scripts PowerShell, optimiseur de connexion Internet... ce sont très souvent des arnaques).

### 🔗Sources
[Microsoft](https://learn.microsoft.com/en-us/windows/security/) | [Discord Entraide Informatique](https://discord.gg/WMsR7dT) | [Piwi](https://www.youtube.com/c/Piwi_youtube) | [BlurBusters](https://blurbusters.com/) | [PrivacyGuides](https://privacyguides.org/)
