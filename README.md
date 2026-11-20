# Grok Soundboard

A flashy, VR-friendly soundboard for Unity (tested conceptually with Unity 2021 & 2022).  
Press **Y** on the left controller (or the Y key in the editor) to open a mod-menu style interface with an RGB strobe background. The menu is clearly labeled **“Grok Soundboard”**.

This is a normal in-project Unity system — no native plugins, no injection, no LibLoader required.

---

## Features

- Opens / closes with the **Y** button (left controller Primary Button)
- Looks like a classic mod menu
- Animated **RGB strobe** background
- Title: **Grok Soundboard**
- Automatically loads user-provided sound files at runtime
- Supports `.wav` and `.ogg` (recommended formats)
- Creates buttons dynamically for every loaded sound
- Simple and expandable codebase
- Works in both Editor and on device (including Quest)

---

## Requirements

- Unity 2021.3+ or Unity 2022.3+ (should also work on newer versions)
- TextMeshPro (usually already included)
- XR Interaction Toolkit or at least the old Input Helpers (for controller button detection)
- A World Space or Screen Space Canvas

---

## Setup Instructions

### 1. Create the Script
1. Create a new C# script called `GrokSoundboard.cs`
2. Paste the full script provided earlier into it
3. Save the script

### 2. Create the Menu Hierarchy
Recommended hierarchy:### 3. Create a Button Prefab
1. Create a UI Button
2. Add a TextMeshPro text child
3. Style it however you like (dark background + bright text works well for the “mod menu” look)
4. Drag it into your Project window to make a prefab
5. Delete it from the scene
6. Assign the prefab to the `soundButtonPrefab` field

### 4. Configure the Script
On the `GrokSoundboard` component:

- **Menu Root** → your MenuRoot panel
- **Title Text** → the TextMeshPro title
- **Button Container** → the object with the Vertical Layout Group
- **Sound Button Prefab** → the prefab you just made
- **Background Image** → the Image used for the RGB strobe
- **Strobe Speed** → 2.0 – 3.5 feels good
- Leave `loadFromPersistentData` enabled

### 5. Canvas Settings (VR)
- Render Mode: **World Space**
- Scale the canvas so it’s comfortable in front of the player (common starting scale: 0.001 – 0.002)
- Place it roughly 1–1.5 meters in front of the player’s start position

---

## Adding Sounds

### Where to put the files

The soundboard automatically looks in:#### On Quest / Android:You can access this folder with SideQuest, Quest Games Optimizer, or any file manager that has permission.

#### In the Unity Editor (Windows example):### Supported Formats
| Format | Recommended | Notes                              |
|--------|-------------|------------------------------------|
| .wav   | Yes         | Best compatibility                 |
| .ogg   | Yes         | Good quality + small size          |
| .mp3   | No          | Unreliable on Android/Quest        |
| .mp4   | No          | Video container – extract audio first |

Just drop the files into the `Soundboard` folder and restart the game (or call `ReloadSounds()`).

---

## Controls

| Action              | Input                          |
|---------------------|--------------------------------|
| Open / Close Menu   | Y button (Left controller)     |
| Open / Close Menu   | Y key (Editor)                 |
| Play Sound          | Click the button in the menu   |

---

## Customization Ideas

- Change the strobe speed
- Make the background more or less transparent
- Add a close button
- Add a “Play Random” button
- Add volume slider
- Add categories / tabs
- Make the menu follow the left hand
- Add a small cooldown so people can’t spam sounds

---

## Troubleshooting

**Menu doesn’t open**
- Check that the Left controller Primary Button is being detected
- Make sure `menuRoot` is assigned
- Try the keyboard Y key in the Editor first

**No sounds appear**
- Check the folder path (use `Debug.Log(Application.persistentDataPath)`)
- Only `.wav` and `.ogg` are loaded by default
- Make sure the files are actually in the `Soundboard` folder
- Call `ReloadSounds()` after adding new files

**Sounds are silent**
- Check the AudioSource volume
- Check device volume
- Make sure the AudioClips actually have data (not zero-length)

**RGB strobe not working**
- Confirm the Background Image is assigned
- Make sure the menu is open (strobe only runs while open)

---

## Notes

- This is intentionally written as a normal Unity component so it works in your own projects without any external loaders.
- The visual style is inspired by classic mod menus, but the code itself is clean and dependency-free.
- You are responsible for only using audio files that you have the rights to use.

---

## License

You’re free to use and modify this script in your own projects.  
Do whatever you want with it.

---

Enjoy the Grok Soundboard.  
Press Y and make some noise.