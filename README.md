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
- A shared folder where I store APKs (such as Obtainium, NetherSX2) and drivers (such as Turnip) to have them on my device for the - initial install > `storage/7AE4-BF10/Synchronisation/Shared`
- Dolphin shaders >  `storage/emulated/0/Android/data/org.dolphinemu.dolphinemu/files/Shaders
storage/emulated/0/Android/data/org.dolphinemu.dolphinemu/files/Shaders`
- RetroArch config folder > `/storage/emulated/0/RetroArch/config`
- RetroArch overlays for Vectrex > `/storage/emulated/0/RetroArch/overlays/vecx`
- RetroArch Guest slang shaders >  `/storage/emulated/0/RetroArch/shaders/shaders_slang/crt/shaders/guest` 
- RetroArch Retro slang Crisis shaders `/storage/emulated/0/RetroArch/shaders/shaders_slang/retro crisis
/storage/emulated/0/RetroArch/shaders/shaders_slang/retro crisis`
- RetroArch Sonkun shaders `/storage/emulated/0/RetroArch/shaders/shaders_slang/sonkun/storage/emulated/0/RetroArch/shaders/shaders_slang/sonkun`
- Screenshots > `storage/emulated/0/Pictures/Screenshots`

Syncthing offers the possibility to filter what gets synced in a shared folder. It's particularily useful for ROMs. I don't sync 16:9 systems to my RPM so I've defined the following exclusions for the ROMs sync :
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
- RetroArch (nightly)
- Azahar (requires renaming of the *.3ds files)
- Dolphin because I could not make the RA core work
- MelonDS

For PS2 emulation :
1. Build/patch https://github.com/Trixarian/NetherSX2-patch
2. Build/patch https://github.com/Trixarian/NetherSX2-classic
Install Classic and keep the other just in case 

Other systems are emulated with RetroArch so as to get a better shader support.

At this point I also make sure that I have Turnip drivers, just in case. It seems that the `turnip_v24.3.0_R9v2` version is nice for the RPM : https://github.com/K11MCH1/AdrenoToolsDrivers/releases/tag/v24.3.0_r9

## Frontend

Either Daijishō with Obtainium, or ES-DE (personal download link sent by mail after subscription). I choose ES-DE :
- Use https://github.com/BinaryQuantumSoul/esde_android_apps so as to import Android apps apps, games and emulators in ES-DE. To install, add https://github.com/schattenphoenix/es_applauncher/releases in Obtainium
- Set as the default launcher to improve the "console" experience and forget about Android. It can be done at the very end because the AOSP Android launcher is more convenient during setup.

## RetroArch installation

### BIOS (system)

The https://docs.libretro.com/library/bios/ documentation lists the required and optional system files for every RA core. To download these ressources, my go-to sources are :
- https://github.com/Abdess/retroarch_system
- a comprehensive Bios Batocera Pack archive in case something is missing by Abdess' (see https://theminicaketv.fr/PACK-BIOS-BATOCERA.htm for instance)

I sync a small custom BIOS pack for RA with Syncthing on my external SD. From there, I copy the files into the `/storage/emulated/0/RetroArch/system` folder on my internal RPM storage.

### General configuration

Many guides detail that, such as https://retrogamecorps.com/2022/02/28/retroarch-starter-guide/

Setting `vulkan` as the default driver is the most convenient way overall. However, no RA core for N64 is able to run games with Vulkan in their default configuration. So you won't be able to start a game and thus you won't be able to access the RA Quick Menu with the override option. So you just cannot define a `gl` override. It can however be done with the UI but with many steps and restarts. Since the `config` is synced by Syncthing, it is very easy to add an override file in the `/storage/emulated/0/RetroArch/config/ParaLLEl N64` folder. For a core override, name the file `ParaLLEl N64.cfg` and add/update the following.
```
video_driver = "gl"
```
If you prefer a content directory override, name the file `n64.cfg`.

**Important : check the directories.** I like more of them than default in `/storage/emulated/0/RetroArch/` rather than `/storage/emulated/0/Android/data/...`

## RetroArch cores



## Scaling in RetroArch

Even for 3D games, when the emulator renders at 2x or more, I strongly dislike the imbalance between texture resolution, the small number of polygons and the super smooth object borders and textures. So I always let shaders do the upsacale and emulators/cores all run at 1x. 

The RPM is great when it comes to scaling options. As a rule of thumb, I'm all in slightly overscaned integer scaling to maximise the size of the objects on the screen, and help with clean shaders. For 4:3 systems, I typically go for settings such as https://shauninman.com/utils/screens/#src_screen:14,src_nn:1,src_crop:1,src_width:320,src_height:240,dst_screen:34,dst_width:1240,dst_height:1080,dst_size:3.92,show_all:0 

For fancy pixel aspect ratios such as CPS or NeoGeo or many Arcade games, I let non-integer scaling with core provided screen aspect ratio.

## Shaders

For CRT consoles, I like CRT shaders. I'm not after fancy and high-fidelity but rather about :
- considering a crt shader as a kind of specific interpolation shader
- getting more colors and better gradients out of the limited 8bits and 16bits palettes
- helping with de-dithering for 8bits and 16bits games
- give a bit more "organic" feel rather than the flat and clinical square pixels
So in any shader I mention, I remove curvature and vignetting.

### Dolphin

Built-in shaders don't help. Clownacy has ported GLSL shaders  for the standalone Dolphin

### NetherSX2

The build in Triangle and Lottes shaders scale well. Others produce banding/moire artifacts. I choose Lottes.  



