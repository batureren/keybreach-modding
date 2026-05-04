Welcome to the official modding documentation for **KeyBreach**. The game natively supports modding via the Steam Workshop. Using a simple JSON configuration file, you can completely customize the aesthetics and audio of the game.

## Table of Contents
1. [Getting Started & Critical Rules](#1-getting-started--critical-rules)
2. [The `mod.json` File](#2-the-modjson-file)
3. [Custom Portraits](#3-custom-portraits)
4. [Custom Music & Audio](#4-custom-music--audio)
5. [Custom Fonts](#5-custom-fonts)
6. [Reference: Internal Character & Boss IDs](#6-reference-internal-character--boss-ids)

---

## 1. Getting Started & Critical Rules

A KeyBreach mod is a folder containing your custom assets (images, audio, fonts) and a configuration file named `mod.json`. 

### ⚠️ CRITICAL MODDING RULES
1. **NO `.wav` FILES!** For audio, strictly use **`.ogg`** (highly recommended for seamless looping) or **`.mp3`**. `.wav` files are too large and are restricted.
2. **ALL LOWERCASE FILENAMES.** Always use lowercase for your file names and extensions (e.g., `my_track.ogg`, NOT `My_Track.OGG`). Because KeyBreach supports Linux and Steam Deck, uppercase letters in file paths will break your mod for other players!

### Recommended Folder Structure
```text
MyAwesomeMod/
│
├── mod.json                  # The core configuration file (Required)
├── preview.png               # Workshop thumbnail (Recommended)
│
├── assets/
│   ├── portraits/            # Custom character faces (.webp / .png)
│   ├── music/                # Custom tracks (.ogg / .mp3)
│   └── fonts/                # Custom fonts (.ttf / .otf / .woff2)
```

---

## 2. The `mod.json` File

The `mod.json` file tells the game how to inject your custom assets. Here is a complete boilerplate template showcasing all currently supported features:

```json
{
  "name": "My Awesome Mod",
  "description": "Replaces the Merchant's face, adds a new font, and changes multiple music tracks.",
  "portraits": {
    "MERCHANT": {
      "neutral": "assets/portraits/merchant_neutral.webp",
      "happy": "assets/portraits/merchant_happy.webp",
      "sad": "assets/portraits/merchant_sad.webp"
    }
  },
  "music": {
    "boss_GATEKEEPER": "assets/music/gatekeeper_remix.ogg",
    "menumusic/main_menu.ogg": "assets/music/custom_menu.ogg",
    "music/panic.ogg": "assets/music/custom_game.ogg",
    "advmusic/pixel_path.ogg": "assets/music/custom_adventure.mp3",
    "duelmusic/panic_manic.ogg": "assets/music/custom_duel.ogg"
  },
  "fonts": {
    "my_custom_font": {
      "name": "Hacker Text",
      "family": "HackerFont",
      "file": "assets/fonts/hackerfont.ttf"
    }
  }
}
```

---

## 3. Custom Portraits

You can replace the UI portraits for any playable character. The game uses three states for portraits: **neutral**, **happy** (high combo/victory), and **sad** (errors/damage/defeat).

In your `mod.json`, add a `"portraits"` object. The keys must exactly match the internal **Character IDs** (see the [Reference Section](#6-reference-internal-character--boss-ids) below).

```json
"portraits": {
  "MAGE": {
    "neutral": "assets/portraits/mage_neutral.webp",
    "happy": "assets/portraits/mage_win.webp",
    "sad": "assets/portraits/mage_hurt.webp"
  }
}
```
* **Best Practices**: Use `.webp` or `.png` formats. Keep images square (e.g., 256x256) with transparent backgrounds for the best UI fit.
* **Missing States**: If you only provide a `"neutral"` state, the game will gracefully use it for all moods.
* **Conflicts**: If two mods attempt to replace the same character portrait, the game will automatically disable one of them to prevent visual overlap.

---

## 4. Custom Music & Audio

You can override specific music tracks in the game. The game pulls music from random pools depending on the game mode. To inject your custom music, you must **override the original file keys**.

In your `mod.json`, add a `"music"` object mapping the original game track to your modded file path:

### A. Replacing Specific Boss Themes
To replace a specific boss's theme, use the prefix `boss_` followed by the Boss ID (See Reference section at bottom for all Boss IDs).
```json
"music": {
  "boss_OVERLORD": "assets/music/overlord_theme.mp3",
  "boss_TRICKSTER": "assets/music/clown_music.ogg"
}
```

### B. Replacing Random Pool Tracks (Standard Rounds, Shop, Duel, etc.)
If you want to replace the music that plays during normal gameplay, shopping, or duels, you must target the specific file keys that the game randomly selects from. 

Use the exact folder and filename as the Key. Here is the full list of valid track keys you can override:

**Main Menu (`menumusic/`)**
* `"menumusic/main_menu.ogg"`

**Shop Tracks (`shopmusic/`)**
* `"shopmusic/pixel_checkout.ogg"`
* `"shopmusic/rush_hour.ogg"`
* `"shopmusic/shopping_panic.ogg"`
* `"shopmusic/word_checkout.ogg"`

**Adventure Mode Tracks (`advmusic/`)**
* `"advmusic/pixel_path.ogg"`
* `"advmusic/pixel_tea_break.ogg"`
* `"advmusic/pixel_trail.ogg"`

**Word Duel Tracks (`duelmusic/`)**
* `"duelmusic/countdown_to_doom.ogg"`
* `"duelmusic/final_word.ogg"`
* `"duelmusic/panic_manic.ogg"`
* `"duelmusic/panic_typo.ogg"`

**Standard Rounds (`music/`)**
*(Pick any of these keys to replace tracks that randomly play during normal floors)*
* `"music/circuit_panic_pursuit.ogg"`
* `"music/combo_chain_frenzy.ogg"`
* `"music/combo_counter_heart.ogg"`
* `"music/core_victory.ogg"`
* `"music/countdown.ogg"`
* `"music/frantic_keyboard_survival.ogg"`
* `"music/glitch_chain.ogg"`
* `"music/glitching_typist.ogg"`
* `"music/hexcore_panic_loop.ogg"`
* `"music/input_of_the_sky.ogg"`
* `"music/key_clackers.ogg"`
* `"music/keyboard_fever.ogg"`
* `"music/packet_storm.ogg"`
* `"music/panic.ogg"`
* `"music/pixel_deadline.ogg"`
* `"music/pixel_perfect_typist.ogg"`
* `"music/pixel_pop_per_minute.ogg"`
* `"music/pixel_rush_getaway.ogg"`
* `"music/rushing.ogg"`
* `"music/stress_rush.ogg"`
* `"music/survival.ogg"`
* `"music/task_manager_tyrant.ogg"`
* `"music/tick_tock.ogg"`
* `"music/type_or_die.ogg"`
* `"music/type_or_not.ogg"`
* `"music/typing_for_my_life.ogg"`
* `"music/typing_fury.ogg"`
* `"music/typing_to_stay_alive.ogg"`
* `"music/victory_run.ogg"`
* `"music/word_of_panic.ogg"`
* `"music/word_rush.ogg"`
* `"music/word_rush_countdown.ogg"`
* `"music/word_rush_gauntlet.ogg"`
* `"music/word_rush_overload.ogg"`
* `"music/word_rush_run.ogg"`
* `"music/word_survival.ogg"`

---

## 5. Custom Fonts

You can add entirely new fonts for the player to select in the **Options -> Input Fonts** menu.

In your `mod.json`, add a `"fonts"` object. The key you provide (e.g., `"hacker_font"`) will be used as the internal ID.

```json
"fonts": {
  "hacker_font": {
    "name": "Cyber Hacker 2077",
    "family": "CyberHacker",
    "file": "assets/fonts/cyberhacker.ttf"
  }
}
```

* **`name`**: The display name shown in the in-game font selection menu.
* **`family`**: The CSS `font-family` name you want to assign to this font.
* **`file`**: The path to your font file (use lowercase for filename and extension!).

*Note: Modded fonts are automatically unlocked for the player and require no achievements to use.*

---

## 6. Reference: Internal Character & Boss IDs

To successfully mod KeyBreach, you need to use the exact internal IDs defined in the game's code.

### Playable Character IDs
Use these keys in the `"portraits"` object:

| Character Name | Internal ID | Character Name | Internal ID |
| :--- | :--- | :--- | :--- |
| The Broker | `MERCHANT` | The Mutant | `MUTANT` |
| The Exploit | `ERROR` | The Troll | `TROLL` |
| The Glitcher | `GLITCHER` | The Anon | `ANON` |
| The Savior | `SAVIOR` | The Noob | `NOOB` |
| The Perfect | `PERFECT` | The ADHD | `ADHD` |
| The Vampire | `VAMPIRE` | The Sleeper | `SLEEPER` |
| The Mage | `MAGE` | The Dual-Wielder | `DUAL_WIELDER` |
| The Adventurer | `ADVENTURER` | The Virus | `VIRUS` |
| The Hunter | `HUNTER` | The Outcast | `OUTCAST` |

### Boss IDs
Use these keys (prefixed with `boss_`) in the `"music"` object to override a specific boss fight theme:

**Standard Bosses**
* `GATEKEEPER` (Floor 5)
* `FIREWALL` (Floor 5)
* `FRAGMENTOR` (Floor 5)
* `OVERCLOCKED` (Floor 5)
* `ELIMINATOR` (Floor 5)
* `HIVEMIND` (Floor 10)
* `RANSOM` (Floor 10)
* `NOISEMAKER` (Floor 10)
* `BANKER` (Floor 10)
* `DARK_MATTER` (Floor 10)
* `ARCHITECT` (Floor 15)
* `LOGIC` (Floor 15)
* `SCRAMBLER` (Floor 15)
* `PURIST` (Floor 15)
* `IRON_MAIDEN` (Floor 15)
* `CORE` (Floor 20)
* `NULL` (Floor 20)
* `GHOST` (Floor 20)
* `GAMBLER` (Floor 20)
* `SHAPESHIFTER` (Floor 20)
* `GLITCH` (Floor 25)
* `TRICKSTER` (Floor 25)
* `WATCHER` (Floor 30)
* `OVERLORD` (Floor 35)
* `DEEPBLUE` (Floor 40)
* `PARADOX` (Floor 45)
* `OMEGA` (Floor 50)
* `INVADER` (Anomaly Event)

**Mini-Bosses (Anomalies)**
* `MINI_WORM`
* `MINI_TROJAN`
* `MINI_SPYWARE`
* `MINI_TRACKER`
