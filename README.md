# Install notes for the Retroid Pocket Mini handheld console

General guidelines : https://retrogamecorps.com/2022/03/13/android-emulation-starter-guide/

Starting the RPM for the first time, don't install anything and set the Android AOSP launcher by default.

## Android Playstore Applications

- MiXplorer (Silver for support)
- Syncthing-fork
- Entertainment : Plex, Youtube ...
- Android games

## Syncthing

I have an "Emulation" folder on my hardrive, with various ressources. I' using Syncthing extensively for convenience : if I want to add/edit sometging, I just do it on my PC and changes get synced automatically.

My main shares are the following (see following sectons for some details):
- BIOS - Platforms (files sorted by patform, as an archive) > `storage/7AE4-BF10/BIOS/Plateforms` (where 7AE4-BF10 is my external SD card number) as a large reference archive
- BIOS - RetroArch (same but filtered and flattened) > `storage/7AE4-BF10/BIOS/RetroArch` as an unaltered archive (not my RA system folder because a lot of stuff is added to the system folder by RA)
- ROMs sorted according the ES-DE folder naming scheme > `/storage/7AE4-BF10/ROMs` with exexclusions (see below)
- A shared folder where I store APKs and drivers such as Turnip's to have them on my device for the - initial install > `storage/7AE4-BF10/Synchronisation/Shared`
- Dolphin shaders >  `storage/emulated/0/Android/data/org.dolphinemu.dolphinemu/files/Shaders
storage/emulated/0/Android/data/org.dolphinemu.dolphinemu/files/Shaders`
- RetroArch config folder > `/storage/emulated/0/RetroArch/config`
- RetroArch overlays for Vectrex > `/storage/emulated/0/RetroArch/overlays/vecx`
- RetroArch Guest slang shaders >  `/storage/emulated/0/RetroArch/shaders/shaders_slang/crt/shaders/guest` 
- RetroArch Retro slang Crisis shaders `/storage/emulated/0/RetroArch/shaders/shaders_slang/retro crisis
/storage/emulated/0/RetroArch/shaders/shaders_slang/retro crisis`
- RetroArch Sonkun shaders `/storage/emulated/0/RetroArch/shaders/shaders_slang/sonkun/storage/emulated/0/RetroArch/shaders/shaders_slang/sonkun`
- Screenshots > `storage/emulated/0/Pictures/Screenshots`

Syncthing offers the possibility to filter what gets synced. It's particularily usefull for ROMs. I don't sync 16:9 systems to my RPM so I've setup the followinf exclusions for the ROMs sync :
```psp
psvita
xbox
wiiu
switch
*.3ds
*.xls
.nomedia
m3u-generator.py
gamelist.xml
systems.txt
.media
.images
```

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

At this point I also make sure that I have Turni^p drovers, just in case. It seems that the `turnip_v24.3.0_R9v2` version is nice for the RPM : https://github.com/K11MCH1/AdrenoToolsDrivers/releases/tag/v24.3.0_r9

## Frontend

Either Daijishō with Obtainium, or ES-DE (personnal download link sent by mail after subscription). I choose ES-DE :
- Use https://github.com/BinaryQuantumSoul/esde_android_apps so as to import Android apps apps, games and emulators in ES-DE. To install, add https://github.com/schattenphoenix/es_applauncher/releases in Obtainium
- Set as the default launcher to improve the "console" experience and forget about Android. It can be done at the very end because the AOSP Android launcher is more convenient during setup.



