# S3AIR Custom Boss Remake: Big Core MK.I from Gradius

This mod replaces the Fire Breath boss in Angel Island Zone Act 1 with Big Core MK.I from Gradius for Sonic 3 A.I.R. (Angel Island Revisited) v26.03.28.0.

## About

Big Core MK.I is the iconic first boss from the original Gradius arcade game. This mod faithfully recreates its behavior as a custom boss in Sonic 3 A.I.R., featuring sine-wave vertical oscillation, horizontal patrol movement, and a devastating laser beam attack.

## Boss Behavior

The Big Core MK.I boss has four distinct states:

1. **Intro (State 1)**: Flies in from the right side of the screen
2. **Active (State 2)**: Oscillates vertically using a sine wave pattern while patrolling horizontally (left for 120 frames, then right for 120 frames). Fires a laser every 180 frames.
3. **Firing Laser (State 3)**: Fires a laser beam projectile that travels left across the screen for 60 frames before returning to Active state.
4. **Defeat (State 4)**: After taking 8 hits, the boss explodes and breaks into four debris pieces.

## File Structure

```
scripts/
  main.lemon              - Mod entry point, global variables, mod compatibility checks
  boss_aiz1.lemon         - Address hooks overriding AIZ1 boss functions
  bigcore_boss_aiz1.lemon - Big Core MK.I boss state machine and AI logic
  render.lemon            - Custom sprite rendering via Standalone.onWriteToSpriteTable
  music.lemon             - Audio/music overrides via Standalone.getModdedSoundKey
sprites/
  BigCore.png             - Normal and hit-flash boss sprites
  BigCore_Intro.png       - Intro fly-in sprite
  BigCore_Defeat.png      - Defeat/explosion sprite
  BigCore_Broken.png      - Broken debris pieces (4 quarters)
  BigCore_Laser.png       - Laser beam projectile sprite
  boss.json               - Sprite definitions for custom rendering
  bossbar.json            - Bossbar mod sprite definition
  bossnames.png           - Boss name display sprite
audio/
  Aircraft Carrier.ogg    - Boss battle BGM (Gradius OST)
  DestroyTheCore.ogg      - Victory jingle on boss defeat
  LaserBeam.ogg           - Laser firing sound effect
  audio_replacements.json - Audio key definitions
mod.json                  - Mod metadata and dependencies
icon.png                  - Mod icon (high resolution)
icon-16px.png             - Mod icon (16x16)
icon-64px.png             - Mod icon (64x64)
```

## Global Variables

| Variable | Type | Description |
|----------|------|-------------|
| `bigcore.state` | u8 | Boss state machine (0=idle, 1=intro, 2=active, 3=laser, 4=defeat) |
| `bigcore.timer` | u8 | General-purpose frame counter (wraps at 255) |
| `bigcore.laser_timer` | u8 | Laser cooldown / firing frame counter |
| `bigcore.move_angle` | u8 | Sine-wave angle for vertical oscillation (0-255) |
| `bigcore.base_y` | u16 | Baseline Y position for sine-wave oscillation |
| `mod_bosswarnings_active` | bool | Whether Boss Warnings mod is active |
| `mod_bossbar_active` | bool | Whether Bossbar mod is active |
| `mod_stone3air_active` | bool | Whether Stone 3 A.I.R. mod is active |

## Mod Compatibility

| Mod | Required | Priority | Notes |
|-----|----------|----------|-------|
| Boss Warnings (Neo Boss Warnings) | No | Higher | Adds boss warning effects before the fight |
| Bossbar | No | Lower | Shows a health bar for the boss |
| Stone 3 A.I.R. (Agent Stone) | No | Lower | Visual style mod compatibility |

## Address Hooks

The mod uses the following ROM address hooks to override original boss behavior:

| Address | End | Original Function | Override Purpose |
|---------|-----|-------------------|------------------|
| 0x068a24 | 0x068a32 | Boss.AIZ1.BaseUpdate | Replace boss update with BigCore.Update() |
| 0x068a46 | 0x068a68 | Boss init | Initialize Big Core variables and position |
| 0x068a6e | 0x068a90 | Camera wait | Trigger Big Core intro fly-in |
| 0x068a94 | 0x068ab4 | Exhaust flames spawner | Suppress (unused by Big Core) |
| 0x068afe | 0x068b16 | Flamethrower spawner | Suppress (unused by Big Core) |
| 0x068c02 | 0x068c0c | Death callback | Setup signpost + Big Core giblets |
| 0x068c12 | 0x068c26 | Rocket launcher | Replace with laser beam projectile |
| 0x068e98 | 0x068eba | Debris init | Custom debris piece initialization |

## Minimum Game Version

Sonic 3 A.I.R. v26.03.28.0

## Credits

- **Author**: DynamicLemon
- **Boss Design**: Based on Big Core MK.I from Gradius (Konami)