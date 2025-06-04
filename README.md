> WIP. For instance, saves sync not covered yet.

# Install notes for the Retroid Pocket Mini handheld console

General guidelines: https://retrogamecorps.com/2022/03/13/android-emulation-starter-guide/

When starting the RPM for the first time, don't install anything and set the Android AOSP launcher as the default.

## Android Play Store Applications

- MiXplorer (Silver version for support)
- Syncthing-Fork
- Entertainment: Plex, PlexAmp, YouTube...
- Android games

## Syncthing

I have an "Emulation" folder on my hard drive, with various resources. I'm using Syncthing extensively for convenience: if I want to add or edit something, I just do it on my PC, and the changes get synced automatically.

My typical main shares are the following (see the following sections for some details):

- **BIOS - Platforms** (files sorted by platform, as an archive) →  
  `/storage/7AE4-BF10/BIOS/Plateforms`  
  (where `7AE4-BF10` is my external SD card ID) — used as a large reference archive
- **BIOS - RetroArch** (same content but filtered and flattened) →  
  `/storage/7AE4-BF10/BIOS/RetroArch`  
  This is an unaltered archive (not my RetroArch system folder because RA adds many files to it)
- **ROMs sorted according to the ES-DE folder naming scheme** →  
  `/storage/7AE4-BF10/ROMs`  
  with exclusions (see below)
- **Shared folder** for APKs (e.g., Obtainium, NetherSX2) and drivers (e.g., Turnip) to have them available on the device for initial setup →  
  `/storage/7AE4-BF10/Synchronisation/Shared`
- **Dolphin shaders** →  
  `/storage/emulated/0/Android/data/org.dolphinemu.dolphinemu/files/Shaders`
- **RetroArch config folder** →  
  `/storage/emulated/0/RetroArch/config`
- **RetroArch overlays for Vectrex** →  
  `/storage/emulated/0/RetroArch/overlays/vecx`
- **Screenshots** →  
  `/storage/emulated/0/Pictures/Screenshots`

Syncthing offers the possibility to filter what gets synced in a shared folder. It's particularly useful for ROMs. I don't sync 16:9 systems to my RPM, so I've defined the following exclusions for the ROMs sync:

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

Use **Obtainium** with [Obtainium-Emulation-Pack](https://github.com/RJNY/Obtainium-Emulation-Pack) to install emulators:

- **RetroArch** (nightly)
- **Azahar** (requires renaming of the `.3ds` files)
- **Dolphin**, because I couldn't get the RetroArch core to work
- **MelonDS**

### For PS2 emulation:

1. Build/patch [`NetherSX2-patch`](https://github.com/Trixarian/NetherSX2-patch)
2. Build/patch [`NetherSX2-classic`](https://github.com/Trixarian/NetherSX2-classic)

Install the **Classic** version and keep the other one just in case.

Other systems are emulated using **RetroArch** to benefit from better shader support.

At this point, I also make sure I have the **Turnip** drivers installed, just in case.  
It seems that the `turnip_v24.3.0_R9v2` version works well for the RPM:  
[Turnip v24.3.0_r9 on GitHub](https://github.com/K11MCH1/AdrenoToolsDrivers/releases/tag/v24.3.0_r9)

## Frontend

Either **Daijishō** via Obtainium, or **ES-DE** (personal download link sent by email after subscription).  
I choose **ES-DE**:

- Use [BinaryQuantumSoul/esde_android_apps](https://github.com/BinaryQuantumSoul/esde_android_apps) to import Android apps, games, and emulators into ES-DE.
- To install, add [https://github.com/schattenphoenix/es_applauncher/releases](https://github.com/schattenphoenix/es_applauncher/releases) to Obtainium.

Set ES-DE as the **default launcher** to improve the "console-like" experience and forget about Android.  
This can be done at the **very end**, as the AOSP Android launcher is more convenient during the initial setup.


## RetroArch installation

### BIOS (system)
The [Libretro BIOS documentation](https://docs.libretro.com/library/bios/) lists the required and optional system files for every RetroArch core.

To download these resources, my go-to sources are:

- [Abdess' retroarch_system repository](https://github.com/Abdess/retroarch_system)
- A comprehensive **BIOS Batocera Pack** archive, in case something is missing from Abdess’ repo  
  (see for example: [https://theminicaketv.fr/PACK-BIOS-BATOCERA.htm](https://theminicaketv.fr/PACK-BIOS-BATOCERA.htm))

I sync a small custom BIOS pack for RetroArch with **Syncthing** on my external SD card.  
From there, I copy the necessary files into the internal RPM storage at:  
`/storage/emulated/0/RetroArch/system`

### General configuration

Many guides explain this setup in detail, such as:  
[RetroArch Starter Guide – Retro Game Corps](https://retrogamecorps.com/2022/02/28/retroarch-starter-guide/)

Setting `vulkan` as the default video driver is the most convenient option overall.  
However, **no RetroArch core for N64** can run games using Vulkan in their default configuration.  
This causes a major issue: you won’t be able to launch a game, and therefore won’t be able to access the RA **Quick Menu** to apply a core override — so you cannot set a `gl` override from within RetroArch directly.

It is technically possible through the UI, but it requires many steps and restarts.

Since the `config` folder is synced via **Syncthing**, the easiest approach is to manually add an override file to:

```
/storage/emulated/0/RetroArch/config/ParaLLEl N64
```

For a **core override**, name the file `ParaLLEl N64.cfg`
and include the following content:

```
video_driver = "gl"
```

If you prefer to apply a **content directory override**, name the file `n64.cfg`.

**Important: check your directory structure.**  
I prefer having more of my config folders in
`/storage/emulated/0/RetroArch/`
rather than in the default scoped storage path:
`/storage/emulated/0/Android/data/...`

## RetroArch cores

| Platform    | Prefered core |
| -------- | ------- |
| Dreamcast | Flycast |
| GB | Gambatte |
| GBC | Gambatte |
| Jaguar | Virtual Jaguar |
| MasterSystem | PicoDrive |
| Megadrive | PicoDrive |
| MS DOS | DOSBox-Pure |
| N64 | ParaLLEl N64 |
| NDS | melonDS |
| NeoGeo | Final Burn Neo |
| NES | Nestopia |
| NGPC | Beetle NeoPop |
| PC Engine | Beetle PCE |
| PSX | SwanStation |
| Saturn | Beetle ? |
| SNES | Snes9x - Current |
| Vectrex | Vecx |

## Scaling in RetroArch

Even for 3D games, when the emulator renders at 2× or more, I strongly dislike the imbalance between texture resolution, the low polygon count, and the overly smooth object borders and textures.  
So I always let **shaders handle the upscaling**, and I run all emulators/cores at **1× internal resolution**.

The **Retroid Pocket Mini (RPM)** is excellent when it comes to scaling options.  
As a rule of thumb, I prefer **slightly overscanned integer scaling** to maximize object size on screen and ensure clean shader output.

For **4:3 systems**, I typically use configurations like the following:  
[https://shauninman.com/utils/screens/#src_screen:14,src_nn:1,src_crop:1,src_width:320,src_height:240,dst_screen:34,dst_width:1240,dst_height:1080,dst_size:3.92,show_all:0](https://shauninman.com/utils/screens/#src_screen:14,src_nn:1,src_crop:1,src_width:320,src_height:240,dst_screen:34,dst_width:1240,dst_height:1080,dst_size:3.92,show_all:0)

For systems with **unusual pixel aspect ratios** like CPS, NeoGeo, or many arcade platforms,  
I allow **non-integer scaling** with the **core-provided aspect ratio**.

> These settings are highly device-dependent.

| Core    | Settings > Video > Scaling options |
| -------- | ------- |
| Default | Non integer, Core provided AR, Crop Overscan |
| Nestopia | Smart Integer scale X+Y, Overscale, 1:1 PAR |
| PicoDrive | Smart Integer scale X+Y, Overscale, 1:1 PAR |
| Snes9x | Smart Integer scale X+Y, Overscale, 1:1 PAR |
| Virtual Jaguar | Smart Integer scale X+Y, Overscale, Core provided AR |
| SwanStation | Smart Integer scale X+Y, Overscale, Core provided AR |
| Flycast | Smart Integer scale X+Y, Overscale, Core provided AR |
| Beetle PCE | Non integer, Full, Crop Overscan |

GB, GBC and NGPC are set as default with non integer scaling but the shader performs an underscaled integer scaling with borders of its own.

## Shaders

For home consoles, I like CRT shaders.  
I'm not after fancy or high-fidelity effects, but rather:

- Considering a CRT shader as a specialized and effective way to perform interpolation  
- Getting more colors and better gradients out of the limited 8-bit and 16-bit palettes  
- Helping with de-dithering for 8-bit and 16-bit games  
- Giving a bit more "organic" feel rather than flat, clinical square pixels

So in any shader I mention, I remove curvature and vignetting.

### Dolphin

Built-in shaders don't help much. Clownacy has ported GLSL shaders for the standalone Dolphin emulator:  
https://clownacy.wordpress.com/2023/06/30/porting-crt-shaders-from-retroarch-to-dolphin/

The latest versions are found in this repo:  
https://github.com/dolphin-emu/dolphin/tree/master/Data/Sys/Shaders

The shaders are `crt-pi.glsl` and `crt-lottes-fast.glsl`.  
They are supposed to expose parameters to the Dolphin UI, but Dolphin on Android lacks the same features as on Windows.  
Therefore, the shaders need manual tweaking for optimal rendering:

- Scanlines produce a lot of moiré/banding artifacts on the RPM, to the point that the shaders become unusable  
- There shouldn't be visible scanlines at all for GameCube 480p content

To remove scanlines as well as curvature, the shaders need to be modified in an editor by changing their default values:

- With **mask #1**: aperture grille  
  [crt_lottes_fast_mask1.glsl](https://github.com/gerpy/rpm-install/blob/main/Dolphin%20Shaders/crt_lottes_fast_mask1.glsl)

- With **mask #2**: shadow mask  
  [crt_lottes_fast_mask3.glsl](https://github.com/gerpy/rpm-install/blob/main/Dolphin%20Shaders/crt_lottes_fast_mask3.glsl)

Shadow masks do a better job at smoothing, in my understanding. Aperture grilles produce a sharper image.  
I chose **mask #2** here.

### NetherSX2

The built-in Triangle and Lottes shaders scale well. Others produce banding/moiré artifacts.  
I choose **Lottes**, which resembles Dolphin's mask #2.

### RetroArch

I save the shader configs as **content directory settings**.  
I'm looking for masks that:

- Are tweakable, allowing a choice between shadow masks and aperture grilles  
- Are robust *with respect to* non-integer scalings  
- Are available in both **SLANG** and **GLSL** to unify systems on my RPM (especially for N64)  
- Have a clean look without vignetting or curvature  
- Are not too complicated

I love the rendering of `crt-gdv-mini-ultra-trinitron` for the way it adds depth to pixels, but it performs poorly when not integer-scaled (tested on 5 different horizontal and vertical scalings with systems such as NeoGeo and CPS1).  
The best I could find is **`crt-easymode-halation`**, which is on the same clean side as the shaders used by Dolphin and NetherSX2.

The author discusses configuration here:  
https://forums.libretro.com/t/configuring-crt-easymode/3384

My experiments show that when scalings are combined with different mask types, not every mask is robust to non-integer scaling. Avoid masks **#4**, **#5**, and **#7**.  
The remaining masks are:

- **#1** for a 2-color aperture grille  
- **#2** for a 3-color aperture grille  
- **#3** for a 2-color shadow mask  
- **#6** for a 3-color shadow mask

I choose **#2** or **#6** depending on the system:

- **#2** when I want sharpness (typically NeoGeo)  
- **#6** when I want to smooth things out a bit more (typically PS1)

My default is :

| Parameter | Value |
| -------- | ------- |
| Gamma Input | 2.80 |
| Gamma Output | 2.10 |
| Sharpness H | 0.1 |
| Sharpness V | 0.9 |
| Mask Type | 6 |
| Mask Str Min | 0.50 |
| Mask Str Max | 0.50 |
| Max Size | 1.00 |
| Scanline Str Min | 0.30 |
| Scanline Str Max | 0.60 |
| Scanline Beam Min | 1.00 |
| Scanline Beam Max | 1.00 |
| Geom Curvature | 0.00 |
| Geom Warp | 0.00 |
| Geom Corner Size | 0.02 |
| Geom Corner Smooth | 1000.00 |
| Interlacing Toggle | 1.00 |
| Halation | 0.07 |
| Diffusion | 0.00 |
| Brightness | 1.00 |

The corner size of the shader is about the same size of the rounded corner of the physical size. It permits to get a unified look with 4:3 boxed content and full screen oversize cropped.




