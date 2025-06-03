# Install notes for the Retroid Pocket Mini handheld console

General guidelines : https://retrogamecorps.com/2022/03/13/android-emulation-starter-guide/

## Syncthing

I have an "Emulation" folder on my hardrive, with various ressources. I do syncthing extensively for convenience : if I want to add/edit sometging, I just do it on my PC and changes get synced automatically. My main shares are the following :
- BIOS - Platforms (files sorted by patform, as an archive) > storage/7AE4-BF10/BIOS/Plateformes (where 7AE4-BF10 is my external SD card number) as a large reference archive
- BIOS - RetroArch (same but filtered and flattened) > storage/7AE4-BF10/BIOS/RetroArch as an unaltered archive (not my RA system folder because a lot of stuff is added to the system folder by RA)
- ROMs sorted according the ES-DE folder naming scheme > /storage/7AE4-BF10/ROMs with exexclusions (see below)
- A shared folder where I store APKs and drivers such as Turnip's to have them on my device for the - initiali install
- Dolphin shaders
- RetroArch config folder
- RetroArch overlays for Vectrex
- RetroArch Guest slang shaders
- RetroArch Retro slang Crisis shaders
- RetroArch Sonkun shaders
- Screenshots

Syncthing offers the possibility to filter what gets synced. It's particularily usefull for ROMs. I don't sync 16:9 systems to my RPM so I've setup the followinf exclusions for the ROMs sync :
- psp
- psvita
- xbox
- wiiu
- switch
- *.3ds
- *.xls
- .nomedia
- m3u-generator.py
- gamelist.xml
- systems.txt
- .media
- .images

## Emulators install

Use Obtainium with the https://github.com/RJNY/Obtainium-Emulation-Pack to install :
- Azahar
- Dolphin
- RetroArch (nightly)
- MelonDS

For PS2 emulation :
1. Build/patch https://github.com/Trixarian/NetherSX2-patch
2. Build/patch https://github.com/Trixarian/NetherSX2-classic
Install Classic and keep the other just in case 

Other systems are emulated with RetroArch so as to get a better shader support.

## Frontend

Either Daijishō with Obtainium, or ES-DE (link in mailbox). I choose ES-DE :
- Use https://github.com/BinaryQuantumSoul/esde_android_apps so as to import Android apps apps, games and emulators in ES-DE. To install, add https://github.com/schattenphoenix/es_applauncher/releases in Obtainium
- Set as the default launcher to improve the "handheld console" experience and forget about Android. Before 



