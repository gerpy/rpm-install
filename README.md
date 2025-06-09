# Install notes for the Retroid Pocket Mini handheld console

*There are many general and very valuable guidelines such as the frequently updated and very valuable [RetroGamesCorps's](https://retrogamecorps.com/2022/03/13/android-emulation-starter-guide/). The present notes are personnal and reflect my own tastes. I don't rewrite everything out there but rather focus on topics which obvioulsy require too much time for content creators in a permanent rush, such as scaling or shaders with the attention they deserve.*

*At the end, the following settings permit a very **unified** experience over emulated platforms, with the generalisation of not fancy yet effective LCD and CRT shaders, custom dual-screen layouts and optimal scaling regarding the RPM screen. No offense and I get the point of a content creator wanting to show different possibilities, but settings such as in [RetroGamesCorps' RPMv2 review](https://www.youtube.com/watch?v=DqoQ-O_RKLc) are all over the place and I'm way too picky to tolerate such inconsistencies in renderings.*

---

When starting the RPM for the first time, don't install anything and set the Android AOSP launcher as the default.

## Android Play Store Applications

- MiXplorer (Silver version for support)
- Syncthing-Fork
- Entertainment: Plex, PlexAmp, YouTube...
- Android games

## Syncthing

I have an "Emulation" folder on my hard drive, with various resources. I'm using Syncthing extensively for convenience: if I want to add or edit something, I just do it on my PC, and the changes get synced automatically.

My typical main shares are the following (see the following sections for some details):

- **BIOS - RetroArch system** →  `/storage/7AE4-BF10/BIOS` (where `7AE4-BF10` is my external SD card ID)
- **ROMs** sorted according to the ES-DE folder naming scheme →  `/storage/7AE4-BF10/ROMs`  
  with exclusions (see below)
- **Shared folder** for stuff loke APKs (e.g., Obtainium, NetherSX2) or drivers (e.g., Turnip) to have them available on the device for initial setup →  `/storage/7AE4-BF10/Share`
- **RetroArch config folder** →  `/storage/emulated/0/RetroArch/config`
- **System screenshots** →  `/storage/emulated/0/Pictures/Screenshots`
- **RetroArch overlays for Vectrex** →  `/storage/emulated/0/RetroArch/overlays/vecx`
- **Dolphin shaders** →  `/storage/emulated/0/Android/data/org.dolphinemu.dolphinemu/files/Shaders`

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
- **Citra MMJ** from https://github.com/weihuoya/citra/releases because I couldn't get a LCD shader with Azahar or Lime3DS, and the RA cores don't offer custom layouts
- **Dolphin**, because I couldn't get the RetroArch core to work and there was is solution for a decent CRT shader (see below)
- **MelonDS**

### PS2 emulation:

1. Build/patch [`NetherSX2-patch`](https://github.com/Trixarian/NetherSX2-patch)
2. Build/patch [`NetherSX2-classic`](https://github.com/Trixarian/NetherSX2-classic)

Install the **Classic** version and keep the other one just in case. Other systems are emulated using **RetroArch** to benefit from better shader support.

At this point, I also make sure I have the **Turnip** drivers installed, just in case.  It seems that the `turnip_v24.3.0_R9v2` version works well for the RPM:  
[Turnip v24.3.0_r9 on GitHub](https://github.com/K11MCH1/AdrenoToolsDrivers/releases/tag/v24.3.0_r9)

### 3DS Layouts

Citra and Azahar permit multi-screen custom layouts. See here : https://github.com/gerpy/rpm-install/tree/main/Layouts%20for%203DS.

## Frontend

Either **Daijishō** via Obtainium, or **ES-DE** (personal download link sent by email after subscription). I choose **ES-DE**:

- Use [BinaryQuantumSoul/esde_android_apps](https://github.com/BinaryQuantumSoul/esde_android_apps) to display entries for Android apps, games, and emulators into ES-DE.
- To install, add [https://github.com/schattenphoenix/es_applauncher/releases](https://github.com/schattenphoenix/es_applauncher/releases) to Obtainium.

Set ES-DE as the **default launcher** to improve the "console-like" experience and forget about Android.  
This can be done at the **very end**, as the AOSP Android launcher is more convenient during the initial setup.

---

## RetroArch installation

### BIOS (system)

The [Libretro BIOS documentation](https://docs.libretro.com/library/bios/) lists the required and optional system files for every RetroArch core.

To download these resources, my go-to sources are:

- [RetroPieBIOS](https://github.com/archtaurus/RetroPieBIOS) have most often the right MD5
- [Abdess' retroarch_system repository](https://github.com/Abdess/retroarch_system) is OK as well
- A comprehensive **BIOS Batocera Pack** archive, in case something is missing from Abdess’ repo  
  (see for example: [https://theminicaketv.fr/PACK-BIOS-BATOCERA.htm](https://theminicaketv.fr/PACK-BIOS-BATOCERA.htm))

I'm using Syncthing to sync a folder on my computer with a `BIOS` folder on the external card in my RPM.

### General configuration

Many guides explain this setup in detail, such as:  
[RetroArch Starter Guide – Retro Game Corps](https://retrogamecorps.com/2022/02/28/retroarch-starter-guide/)

Setting `vulkan` as the default video driver is the most convenient option overall. However, **no RetroArch core for N64** can run games using Vulkan in their default configuration.  This causes an issue: you won’t be able to launch a game, and therefore won’t be able to access the RA **Quick Menu** to apply an override — so you cannot set a `gl` override from within RetroArch directly. It is actually technically possible through the UI, but it requires many steps and restarts.

Since the RA `config` folder is synced by **Syncthing**, the easiest way is to manually add an override file to `/storage/emulated/0/RetroArch/config/ParaLLEl N64`. For a **core override**, name the file `ParaLLEl N64.cfg` and include the following content:

```
video_driver = "gl"
```

If you prefer to apply a **content directory override**, name the file `n64.cfg`.

**Important: check your directory structure.**  I prefer having most of my RA directories in `/storage/emulated/0/RetroArch/` rather than in the default scoped storage path `/storage/emulated/0/Android/data/...`, for easier access if required.

### Settings saving policy

> - **Scaling** parameters are saved as **core** overrides, the alternative beeing at the core level in core settings
> - **Shaders** presets atre saved as **content directory** presets because some cores such as Pico Drive emulate platforms of different generations that I prefer to shade differently
> - **Video filters** such as Blargg's are saved as **content directory** overrides because these are related to shaders

## RetroArch cores

Everything I can, I emulate using RetroArch cores.  But :
- I couldn't make the Dolphin core work
- Performance is better on NetherSX2
- Standalone emulators for DS and 3DS are better at setting custom 2 screen layouts, and OK to LCD shade

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
| Saturn | Beetle Saturn |
| SNES | Snes9x - Current |
| Vectrex | Vecx |

For Vectrex, overlays are almost mandatory and I'll try to use an [archive.org](https://archive.org/details/moonland_202212) pack that seems to have good quality overlays

---

## Scaling in RetroArch

> These settings are highly device-dependent.

With 3D games, when the core renders at 2× or higher, I find the visual balance between textures, polygon count, and lines quite off. That’s why I always let shaders handle the upscaling and keep all emulators/cores running at **1×** internal resolution.

The Retroid Pocket Mini is excellent when it comes to scaling options. As a rule of thumb, I prefer a slight overscan crop to maximize object size on screen and ensure clean shader output. With the RPM, it also often translates into integer scaling..

For **4:3 systems**, I typically use configurations like the following:  
[Shaunimman Screen Config](https://shauninman.com/utils/screens/#src_screen:14,src_nn:1,src_crop:1,src_width:320,src_height:240,dst_screen:34,dst_width:1240,dst_height:1080,dst_size:3.92,show_all:0). Smart integer scaling results in tiny overscan on the sides and a very slightly increased height compared to boxed full 4:3.

For **8:7-like systems**, integer scaling with overscan induces a small amount of overscan crop both vertically and horizontally, which is suitable because since CRTs games were actually developped having in mind an overscan up to ~5% on consumer CRT TVs. See https://www.nesdev.org/wiki/Overscan for instance.

**PC Engine** mostly used a ~256×240 resolution but overscan isn't super welcome here. Indeed, many games seem to be designed with very little tolerance to overscan. The core mostly requests 4:3, which seems a bit stretched to my eyes, while a 1:1 pixel aspect ratio leads to tall images with black pillars. The RPM's display aspect ratio is a good compromise to me, very close to 1:1 PAR though.

For systems with **unusual pixel aspect ratios** like CPS, NeoGeo, or many arcade platforms, I allow **non-integer scaling** with the **core-provided aspect ratio**.

> A few errors just below. WIP

| Core    | Settings > Video > Scaling options |
| -------- | ------- |
| Default | Non integer, Core provided AR, Crop Overscan |
| Nestopia | Integer Overscale Y+X, 1:1 PAR |
| PicoDrive | Integer Overscale Y+X, 1:1 PAR |
| Snes9x | Integer Overscale Y+X, 1:1 PAR |
| SwanStation | Integer Overscale Y+X, Core provided AR |
| ParaLLEl | Integer Overscale Y, Core provided AR |
| Flycast | Smart Integer scale X+Y, Core provided AR |
| Beetle PCE | Non integer, Full, Crop Overscan |
| Gambatte | Smart Integer scale X+Y, 1:1 PAR |
| mGBA | Smart Integer scale X+Y, 1:1 PAR |
| Beetle NeoPop | Smart Integer scale X+Y, 1:1 PAR |

---

## Multi-screen layouts

For NDS and 3DS, custom layouts are a good way to control how both screens scale to ensure good looking grid shaders.

### Citra MMJ

So as to setup a custom layout for Citra MMJ, it is necessary to edit the `[Layout]` section of the `/storage/emulated/0/Citra-emu/config/config-mmj.ini` file. The following settings display the top screen with a 3x integer scaling on top of a smaller 1.5x bottom screen.

```
[Layout]
landscape_custom_layout = True
landscape_top_left=20
landscape_top_top=0
landscape_top_right=1220
landscape_top_bottom=720
landscape_bottom_left=380
landscape_bottom_top=720
landscape_bottom_right=860
landscape_bottom_bottom=1080
```
However, the top and the bottom screen of the 3DS are not the same apsct ratio so when you swan top and bottom screen, they come streched.

### Melon DS

I was not able to find the config files so I was left with the touchscreen to try to draw layouts by hand. I've set a layout with a top 4x screen (1020 pix wide) and a bottom 1.5x screen (384 pix wide). The LCD shader looks fine.

---

## Shaders for standalone emulators

### Dolphin

Built-in shaders don't help much. Clownacy has ported GLSL shaders for the standalone Dolphin emulator:  
https://clownacy.wordpress.com/2023/06/30/porting-crt-shaders-from-retroarch-to-dolphin/. The latest versions are found in this repo: https://github.com/dolphin-emu/dolphin/tree/master/Data/Sys/Shaders/.

The shaders are `crt-pi.glsl` and `crt-lottes-fast.glsl`. They are supposed to expose parameters to the Dolphin UI, but Dolphin on Android lacks the parameters tweaking feature as on Windows. Therefore, the shaders need manual edits of default values for optimal rendering.

Scanlines produce a lot of moiré/banding artifacts on the RPM, to the point that the shaders become unusable. There shouldn't be visible scanlines at all for GameCube 480p content. To remove scanlines as well as curvature, the shaders need to be modified in an editor by changing their default values. These are modified **`crt-lottes-fast.glsl`** :

- With **mask #1** (aperture grille) : [crt_lottes_fast_mask1.glsl](https://github.com/gerpy/rpm-install/blob/main/Dolphin%20Shaders/crt_lottes_fast_mask1.glsl)

- With **mask #2** (shadow mask) : [crt_lottes_fast_mask3.glsl](https://github.com/gerpy/rpm-install/blob/main/Dolphin%20Shaders/crt_lottes_fast_mask3.glsl)

Shadow masks do a better job at smoothing, in my understanding. Aperture grilles produce a sharper image.  I chose **mask #2** here. The `crt-pi.glsl` mask is just an aperture grille with scanlines, without added value over `crt-lottes-fast.glsl` IMO.

### NetherSX2

The built-in Triangle and Lottes shaders scale well. Others produce banding/moiré artifacts. I choose **`Lottes`**, which resembles Dolphin's mask #2.

### Citra

The built-in **`Dots`** postprocessing video shader is a decent LCD shader.

### MelonDS

There is a built-in **`LCD`** postprocessing video shader.

---

## RetroArch shaders

### Handheld consoles

As much I like the `dot-matrix` serie of shaders, they appeared buggy at the time of my install, with blacklines top and down after closing content and starting again.

So I've chosen **`simpletex_lcd`** for GB(C) with a slightly darkened grid for GBC (not to let white areas empty with just the background texture). For GB, prepening **`gb-palette-pocket`** makes it unnecessary and provides a retro vibe. Neo.Geo Pocket Color is setup as GBC. For GBA, **`gameboy-advance-dot-matrix`** is just fine as long as integer scaling is used.

| Platform | Shader | Settings |
| -------- | ------- | ------- |
| Gameboy | `gb-palette-pocket` + `simpletex_lcd` | `Darken Colours = 0.00` |
| Gameboy Color | `simpletex_lcd` | `Darken Colours = 0.0` and `Darken Grid = 0.20` |
| NeoGeo Pocket Color | `simpletex_lcd` | `Darken Colours = 0.0` and `Darken Grid = 0.20` |
| GB Advance | `gameboy-advance-dot-matrix` | Default |

These settings are saved as **content directory settings**.

### Home consoles (CRT)

#### Shaders

> For home consoles, I like CRT shaders but I'm not after fancy or high-fidelity effects. I'm just after the general mood by:
> - Considering a CRT shader as a specialized and effective way to perform interpolation  
> - Getting more colors and better gradients out of the limited 8-bit and 16-bit palettes  
> - Helping with de-dithering for 8-bit and 16-bit games  
> - Giving a bit more "organic" feel rather than flat, clinical square pixels

The following main CRT shader config is saved as my **global preset**. Specific configurations (such as Megadrive) come as **content directory presets**. If I want to make a system look more retro, I use **content directories overrides** with Blargg filters but I let the shaders as they are. Remember that scaling options were saved as **core directories overrides** so every aspect is independant from the other, which is simpler IMO.

More than just scanlines, I'm looking for masks that:

- Are tweakable, allowing a choice between shadow masks and aperture grilles  
- Are robust *with respect to* non-integer scalings  
- Are available in both `SLANG` and `GLSL` to unify platform rendering on my RPM (especially for N64)  
- Have a look on the clean side, without vignetting or curvature  
- Are not too complicated to configure (`guest` is typically far beyond my understanding)

I love the rendering of `crt-gdv-mini-ultra-trinitron` for the way it adds depth to pixels, but it performs poorly when not integer-scaled (tested on 5 different horizontal and vertical scalings with systems such as NeoGeo and CPS1).  

The best I could find is **`crt-easymode-halation`**, which is inline with the clean shaders used by Dolphin and NetherSX2. The author discusses configuration here: https://forums.libretro.com/t/configuring-crt-easymode/3384, for reference.

My **`crt-easymode-halation`** experiments show that, when scanlines are combined with mask, not every mask type is robust to non-integer scaling. Avoid masks **#4**, **#5**, and **#7**.  The remaining masks are:

- **#1** for a 2-color aperture grille  
- **#2** for a 3-color aperture grille  
- **#3** for a 2-color shadow mask  
- **#6** for a 3-color shadow mask

I keep **#2** or **#6** :
- **#2** when I want sharpness (typically NeoGeo)  
- **#6** when I want to smooth things out a bit more (typically PS1)

My default settings for **`crt-easymode-halation`** are :

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
| Interlacing Toggle | 0.00 for Dreamcast at least |
| Halation | 0.07 |
| Diffusion | 0.00 |
| Brightness | 1.00 |

The corner size of the shader is about the same size of the rounded corner of the physical RPM screen. It permits to get a more unified look with 4:3 boxed content.

For **Megadrive**, due to the super heavy use of dithering to overcome the low 61 simultaneous color display, I prepend a **`jinc2-dedither`** pass, which arguably produce way less artifacts than **`mdapt`** (on text in particular). It is kind of heretic but it helps preserving better details than blurring everything under heavy RC/composite stuff.

> In addition to this shader settings, it is possible to add nostalgy by using **video filters (CPU)** (saved as content directory overrides). I would try :
> - An RC Blargg filter for 8 bits
> - Composite for 16 bits
> - RGB or nothing for 32+ bits

### Filters

Not really saders, but let's put this section here as filters are closely related to proper shaders I stored them as **content directory overrides** and not core overrides. Shaders above are used to evocate how a CRT screen displays things (with a mask, scanlines, glows and such). I use filters to simulate how the video signal is transmitted to the screen, with more or less degradation, assuming *RF < Composite < S-Video < RGB < Perfect (no filter)*. I'm using the following filters in Settings > Video Filter. I beleive that there are ways to fine tune everything but I keep it basic.

| Filter | Platform |
| -------- | ------- |
| Blargg_NTSC_SNES_RF          | C64, Atari 2600 and such |
| Blargg_NTSC_SNES_Composite   | Amiga, NES, MasterSystem, Megadrive |
| Blargg_NTSC_SNES_S-Video     | PC-Engine, SNES, PS, Saturn, N64, CPS1, general Arcade |
| Blargg_NTSC_SNES_RGB         | Dreamcast, NeoGeo, CPS2, CPS3 |
| OFF                          | Vectrex |

It appears that when you store a **content directory override** while a **core override** is already present in the ```config\Core``` folder, then all the information in the **core override** is repeated. It can be an issue when multiple platforms are emulated by the same emulator that you scale the same (Picodrive, FBN) but want a different signal degradation along with the shader of choice for that particuluar platform.

For instance, the ```config\PicoDrive\PicoDrive.cfg``` has the following content :

```
video_scale_integer = "true"
video_scale_integer_axis = "1"
video_scale_integer_scaling = "1"
```

but the ```config\PicoDrive\mastersystem.cfg``` looks like the following. The last 3 lines beeing already in the more general ```config\PicoDrive\PicoDrive.cfg```, they can safely be removed with a text editor.

```
video_filter = "/data/user/0/com.retroarch.aarch64/filters/video/Blargg_NTSC_SNES_S-Video.filt"
video_scale_integer = "true"
video_scale_integer_axis = "1"
video_scale_integer_scaling = "1"
```

To define content directory overrides, I personally find easier to directly add ```platform.cfg``` files with a one line content among the following.

```
video_filter = "/data/user/0/com.retroarch.aarch64/filters/video/Blargg_NTSC_SNES_RF.filt"
video_filter = "/data/user/0/com.retroarch.aarch64/filters/video/Blargg_NTSC_SNES_Composite.filt"
video_filter = "/data/user/0/com.retroarch.aarch64/filters/video/Blargg_NTSC_SNES_S-Video.filt"
video_filter = "/data/user/0/com.retroarch.aarch64/filters/video/Blargg_NTSC_SNES_RGB.filt"
```


