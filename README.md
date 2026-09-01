# PadForge

Use PlayStation and generic controllers on Windows, wired or wireless,
several at once, in any game including ones that have no controller support
at all.

Games on PC almost always speak **XInput**, which is an Xbox-only protocol. A
DualSense or DualShock 4 on Bluetooth is just a raw HID device, so most games
simply do not see it. PadForge reads the real controller and presents a virtual
Xbox or DualShock 4 pad in its place. For games that only understand a
keyboard and mouse, it sends key presses instead.

---

## Install

Two editions on the [Releases page](https://github.com/kasraferi-cmd/padforge/releases).
Both contain the same app; pick whichever suits you.

**`PadForge-Setup-x.y.z.exe`** installs normally, with Start-menu and Desktop
shortcuts and an entry in Add or remove programs.

**`PadForge-Portable-x.y.z.zip`** does not install at all. Unzip it and run
`PadForge.exe`. Settings live in a `PadForge-data` folder beside the exe, so the
whole thing travels on a USB stick and leaves nothing behind on the PC.

.NET is built into both, so there is no runtime to install and nothing extra to
download.

### One driver is still needed

ViGEmBus creates the virtual controller that games actually see. It runs inside
the Windows kernel, which means no application can carry one, portable or not.
The installer offers to fetch it for you; otherwise get it from the
[ViGEmBus releases page](https://github.com/nefarius/ViGEmBus/releases). PadForge
says so on screen if it is missing.

Worth adding too: the [HidHide driver](https://github.com/nefarius/HidHide/releases),
which powers the **Hide originals** switch described below.

### From source

`build.cmd` compiles it, `install.cmd` installs that build to
`%LOCALAPPDATA%\PadForge`, and `package.cmd` produces both release files. You
only need these if you are changing the code.

---

## Supported controllers

| Controller | USB | Bluetooth | Battery | Lightbar | Rumble |
|---|---|---|---|---|---|
| DualSense (PS5) | yes | yes | yes | yes | yes |
| DualSense Edge | yes | yes | yes | yes | yes |
| DualShock 4 (PS4) | yes | yes | yes | yes | yes |
| Generic USB pads | yes | — | — | — | — |

Up to **8 controllers at once**, each with its own player slot and colour.

A pad that is plugged in *while* also paired over Bluetooth shows up twice at
the hardware level. PadForge recognises both as one controller (it matches them
by MAC address) and uses the cable, which has lower latency and charges at the
same time.

---

## Modes

Each controller card has four modes:

- **Xbox** — games see an Xbox pad and draw A / B / X / Y prompts. Best
  compatibility; use this unless you want otherwise.
- **PlayStation** — games see a DualShock 4 and draw cross / circle / square /
  triangle prompts. Works for *any* controller, so a generic pad can show
  PlayStation buttons.
- **Keyboard** — the pad sends key presses and mouse movement. **This is how you
  play games with no controller support**, like Counter-Strike. Configure it
  under **Buttons → Keyboard layout**.
- **Charge only** — input is ignored and PadForge stops talking to the pad, so
  it drops into low power and charges at full speed. The **Charge light** switch
  decides whether the lightbar reports what the battery is doing or stays dark.

The **ON** switch on each card turns the bridge off entirely, handing the raw
controller straight back to Windows, exactly as if PadForge were not running.

---

## Buttons — per-controller setup

Four tabs:

**Buttons** — remap any button to any other action. For generic pads there is
also a teaching mode: cheap pads number their buttons differently from one
another and nobody documents it, so the app asks you to press each button in
turn and records which index actually fired.

**Keyboard layout** — what each button sends in Keyboard mode, what each stick
does (mouse, scroll, or WASD-style direction keys), and mouse speed and
precision curve. Three presets to start from:

- *Desktop* — navigating Windows from the couch
- *FPS* — left stick walks, right stick aims, triggers shoot and aim
- *Cursor* — point-and-click and strategy games

**Stick feel** — response curve, separate horizontal and vertical sensitivity,
**aim steadying** (slows the right stick while a trigger is held, the way console
shooters steady your aim down sights), **trigger snap** (turns the analogue
triggers into plain on/off buttons that fire the moment they pass a point you
choose, which suits shooters and ruins racing games), stick swap, Y inversion,
turbo, and **motion aiming** for pads with a gyro.

**Single-player** — recoil compensation, rapid fire and auto-sprint. Off until
you turn on the master switch and confirm the warning; see below.

Everything in these tabs works on the pad's own input. Nothing reads the game,
sees the screen, or knows where anything is. These are the same knobs a
console game puts in its own options screen.

---

## Single-player assists

The **Single-player** tab holds three helpers:

| Helper | What it does |
|---|---|
| Recoil compensation | Pulls the right stick down while you hold fire, easing in over about a quarter second. It is a fixed pull on a timer and does not watch the screen, so it only approximates a weapon's pattern and will fight you on weapons that kick differently. |
| Rapid fire | Pulses the fire trigger while held, so semi-automatic weapons fire as fast as the game allows. |
| Auto-sprint | Holds sprint whenever the left stick is pushed fully forward. |

These are off until you enable the master switch and confirm a warning, and the
confirmation is remembered per controller.

**None of this can aim for you.** Nothing here reads the game's memory or scans
the screen, so nothing knows where an enemy is. That boundary is deliberate,
and it is what separates a controller helper from a cheat program.

It still gives an advantage a plain pad does not have. In a story or offline game
that harms nobody. In a competitive online game like Counter-Strike, Rainbow
Six, Call of Duty or Fall Guys, it counts as cheating and can get an account
banned permanently. PadForge cannot tell which game is running, so that call is yours.

---

## Other switches

**Hide originals** (top bar) — hides the physical controllers from everything
except PadForge, using HidHide. A bridged pad is otherwise visible twice: as the
real HID device and as the virtual pad. Some games, especially ones launched
outside Steam, bind to the real device, see a layout they do not understand,
and act as though no controller is attached. Turn this on if a game ignores your
controller. It is lifted automatically when PadForge closes.

**Stay awake** (per pad) — keeps nudging the controller so it does not power
itself off when idle. Turn it **off** while charging: a silent pad drops into
low power and charges much faster.

**Charge light** (per pad) — in Charge only mode, whether the lightbar reports
the battery or stays off. When it is on, the colour is read from the pad rather
than guessed at: amber while power is going in, green once it is full, and dark
when nothing is charging. That last case matters, because a controller on
Bluetooth is not charging at all no matter how it is lit, and a light that
implied otherwise would be lying to you. The lightbar is also the pad's biggest
power draw, so charging dark is slightly faster.

**Language** (top bar) — switches the whole interface between English and
Persian. Persian flips the layout right-to-left and uses B Nazanin, scaled up
and bolded so it reads as clearly as the English build. The pad diagram and
device names stay left-to-right, because mirroring a picture of hardware would
put L2 on the right. The choice is remembered.

**Other brands** (top bar) — whether to pick up non-PlayStation controllers.

**Close hides to tray** (top bar) — with this on, the X button tucks PadForge
into the notification area so games keep seeing your controller. Turn it off and
the X quits properly.

**Start with Windows** (top bar) — launch PadForge at login. Worth turning on,
because of the ordering rule below.

---

## If a game does not see the controller

In this order:

1. **Start PadForge before the game.** Most engines only look for XInput
   controllers once, at startup. If the game was already running when the
   virtual pad appeared, it will never notice it. Close the game, make sure
   PadForge shows your pad, then launch the game.
2. **Turn on Hide originals.** This is the usual fix for non-Steam and cracked
   releases, which often bind to the raw HID device instead of XInput.
3. **Check Steam.** If Steam Input's PlayStation support is on, Steam grabs the
   pad for itself. Turn it off and let PadForge handle the controller.
4. **If the game has no controller support at all**, switch that pad to
   **Keyboard** mode and pick the FPS preset.

---

## Notes

**An idle pad reads as zero, and that is correct.** ViGEm does not resend an
unchanged report, so a diagnostic tool may show a stale value for a controller
nobody is touching. Move a stick and it updates immediately.

**Charging speed.** A DualSense holds 1560 mAh; from empty that is roughly three
hours on a normal USB port. It charges far slower with the lightbar lit and the
pad kept awake, which is why Charge only mode goes quiet and gives the lights
back. Use a data cable and a rear-panel USB port for the best rate.

**Files.** An installed copy keeps settings and button profiles in
`%LOCALAPPDATA%\PadForge`. A portable copy keeps them in `PadForge-data` beside
the exe instead. Either way it is `profiles.json` for button maps and
`settings.json` for preferences, with `padforge.log` alongside for the rare
unexpected error. Updating never touches any of them.

---

## How it works

```
physical pad ──HID──▶ remap ──▶ tuning ──┬──ViGEmBus──▶ virtual Xbox / DS4 ──▶ game
     ▲                                   └──SendInput──▶ keyboard + mouse ──▶ game
     └──────────────── rumble ◀──────────────────────────────────────────────┘
```

Each controller gets its own reader thread, so one pad stalling cannot affect
another. Input runs at the controller's native rate, around 250 Hz over USB and up to
roughly 700 Hz over Bluetooth, while the on-screen readouts refresh at 60 Hz.

In Keyboard mode the key events carry hardware scan codes, not just virtual-key
codes, because games that read raw input or DirectInput ignore anything else.
