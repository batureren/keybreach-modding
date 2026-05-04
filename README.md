# KeyBreach Modding Guide (`mod.md`)

Welcome to the official modding documentation for **KeyBreach**. The game natively supports modding via the Steam Workshop. Using a simple JSON configuration file, you can completely customize the aesthetics and audio of the game.

## Table of Contents
1. [Getting Started & Important Rules](#1-getting-started--important-rules)
2. [The `mod.json` File](#2-the-modjson-file)
3. [Custom Portraits](#3-custom-portraits)
4. [Custom Music & Audio](#4-custom-music--audio)
5. [Custom Fonts](#5-custom-fonts)
6. [Reference: Internal IDs](#6-reference-internal-ids)

---

## 1. Getting Started & Important Rules

A KeyBreach mod is a folder containing your custom assets (images, audio, fonts) and a configuration file named `mod.json`. 

### ⚠️ CRITICAL MODDING RULES
1. **Always use lowercase for filenames and extensions.** (e.g., `my_track.ogg`, NOT `My_Track.OGG`). Because KeyBreach supports Linux and Steam Deck, uppercase letters in file paths can break your mod for other players!
2. **NO `.wav` files.** For audio, strictly use **`.ogg`** (highly recommended for seamless looping) or **`.mp3`**. `.wav` files are too large and are not officially supported by the mod manager.

### Recommended Folder Structure
```text
MyAwesomeMod/
│
├── mod.json                  # The core configuration file (Required)
├── preview.png               # Workshop thumbnail (Recommended)
│
├── assets/
│   ├── portraits/            # Custom character faces (.webp / .png)
│   ├── music/                # Custom tracks (.ogg / .mp3 ONLY)
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
    "menumusic/menu1.ogg": "assets/music/custom_menu.ogg",
    "music/standard_round.ogg": "assets/music/custom_game.ogg",
    "advmusic/adventure1.ogg": "assets/music/custom_adventure.mp3",
    "duelmusic/duel1.ogg": "assets/music/custom_duel.ogg"
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

In your `mod.json`, add a `"portraits"` object. The keys must exactly match the internal **Character IDs** (see the [Reference Section](#6-reference-internal-ids) below).

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

You can override any specific music track in the game, including general folder tracks, specific game modes, and unique Boss themes.

In your `mod.json`, add a `"music"` object. **Audio must be `.ogg` or `.mp3`.**

### Replacing Boss Themes
To replace a specific boss's theme, use the prefix `boss_` followed by the Boss ID.
```json
"music": {
  "boss_OVERLORD": "assets/music/overlord_theme.mp3",
  "boss_TRICKSTER": "assets/music/clown_music.ogg"
}
```

### Replacing Standard Tracks & Game Modes
To replace standard tracks, you must match the game's internal folder mapping as the key. Here is how the folders map to game modes:

* **Menu:** `menumusic/filename.ogg`
* **Shop:** `shopmusic/filename.ogg`
* **Standard Rounds:** `music/filename.ogg`
* **Adventure Mode:** `advmusic/filename.ogg`
* **Word Duels:** `duelmusic/filename.ogg`

**Examples of overriding different modes:**
```json
"music": {
  "menumusic/main_theme.ogg": "assets/music/my_menu.ogg",
  "shopmusic/shop_chill.ogg": "assets/music/my_shop.ogg",
  "music/round_theme.ogg": "assets/music/my_standard_round.ogg",
  "advmusic/explore.ogg": "assets/music/my_adventure.ogg",
  "duelmusic/fight.ogg": "assets/music/my_duel.mp3"
}
```

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

## 6. Reference: Internal IDs

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
Use these keys (prefixed with `boss_`) in the `"music"` object (e.g., `boss_GATEKEEPER`):

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
