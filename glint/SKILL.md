---
name: glint
description: >-
  Create App Store and Play Store screenshots for a Flutter or Android app.
  Use this skill when the user asks for store screenshots, app store assets,
  screenshot generation, or mentions Glint, Capture, or Bridge. Also use it
  to fix, re-export, or polish screenshots Glint already made.
---
# glint: App Store screenshots for the app in this repo

Glint captures real app UI at exact store dimensions and turns them into
upload-ready screenshots. Two capture paths, one output format:

- **Path A - Flutter (Glint-Capture):** renders widgets in test isolation.
  No device needed. Fast, CI-friendly. Best for Flutter apps.
- **Path B - Android device (Glint-Bridge):** captures real pixels from a
  connected phone via ADB. Works for any Android app (native, React Native,
  Flutter, Kotlin).

Both produce `session.json` + PNGs → import into Glint-Web for templates,
frames, and export.

## Before anything: check what exists

```bash
ls glint.yaml test/glint_screenshots_test.dart 2>/dev/null
ls output/session.json 2>/dev/null
ls goldie/goldie.config.ts .argent/flows/ 2>/dev/null
```

If `glint.yaml` exists, this is a follow-up - read it and the test file
before doing anything. If `output/session.json` exists, screenshots are
already captured - skip to Web import.

## Step 0: Decide capture path

Check what the repo contains:

- **Flutter app** (has `lib/` with `.dart` files, `pubspec.yaml`):
  → Use Glint-Capture (Path A). Faster, no device required.
- **Android-only** (has `android/` with `.gradle`, no `lib/`):
  → Use Glint-Bridge (Path B). Needs a connected device.
- **Cross-platform** (Flutter with both `lib/` and `android/`):
  → Ask the user which path. Flutter capture is simpler.

## Step 1: Gather facts

### Path A - Flutter (Glint-Capture)

```bash
# Check if glint CLI is available
glint help 2>/dev/null || dart run glint_capture --help 2>/dev/null
```

If not installed, activate it:

```bash
dart pub global activate --source git https://github.com/GlintShot/Glint-Capture.git
```

### Path B - Android device (Glint-Bridge)

```bash
cd Glint-Bridge
python3 glint.py check
python3 glint.py devices
```

If no device: tell the user to connect via USB and enable USB debugging.
If no ADB: tell them to install platform-tools.

## Step 2: Capture

### Path A - Flutter

From the Flutter app root:

```bash
glint init          # creates glint.yaml + test file (skip if exists)
glint discover --write --max 5   # find best marketing screens
glint capture       # run widget tests, capture PNGs
```

Or one command: `glint capture --auto` (discovers + captures).

The agent should:

1. Review discovered screens - reject login, debug, empty, loading screens
2. Edit `test/glint_screenshots_test.dart` if needed
3. Run `glint capture`
4. Verify output: `ls glint_screenshots/android/pixel9/` should have PNGs

### Path B - Android device

From Glint-Bridge:

```bash
# Single screenshot (developer navigates manually)
python3 glint.py capture

# Batch capture (5 screenshots with delay)
python3 glint.py batch --count 5

# Auto-crawl with AI (best for full app exploration)
python3 glint.py crawl com.app.package --ai

# Auto-crawl without AI (heuristic scroll/tap)
python3 glint.py crawl com.app.package
```

Output goes to `Glint-Bridge/output/`.

## Step 3: Validate session

Both paths produce `session.json` + PNGs. Verify the handoff:

```bash
# Check session.json exists and has screens
cat glint_screenshots/session.json  # Path A
cat Glint-Bridge/output/session.json  # Path B
```

Session format:

```json
{
  "screens": ["android/pixel9/home.png", "android/pixel9/profile.png"],
  "store": "play/phone",
  "version": "1.0",
  "exportedAt": "2026-09-03T12:00:00.000Z"
}
```

Store values: `play/phone`, `ios/iphone`, `ios/ipad`, `play/tablet-7`, etc.

## Step 4: Import into Glint-Web and export

### Option A - Browser editor (interactive)

1. Start Glint-Web: `cd Glint-Web && npm install && npm run dev`
2. Open http://localhost:5173
3. Click "Import Folder" → select the output directory
4. Pick a template from the gallery
5. Set headlines, backgrounds, device frames in the editor
6. Export → PNG ZIP

### Option B - Headless export (CI / agent)

```bash
cd Glint-Web
npm install && npm run build && npm run preview &
# Wait for server to start
npx playwright install chromium
node scripts/headless-export.mjs \
  --session ../glint_screenshots \
  --template blink-play \
  --out ../Glint-ss.zip
```

### Option C - Via MCP tools

If the MCP server is running:

- `glint_validate_session` → check session folder
- `glint_export` → headless ZIP export

## Step 5: Verify and report

After export:

1. Check the ZIP exists and has the right number of files
2. Report: "Exported N screenshots at [store size] → [path]"
3. If the user wants View handoff: use "Copy for Glint View" in the editor

## Iterating

| User asks for          | Edit                                  | Then run          |
| ---------------------- | ------------------------------------- | ----------------- |
| Different screens      | `test/glint_screenshots_test.dart`  | `glint capture` |
| Different template     | Re-import into Web, pick new template | Export            |
| Different headlines    | Edit in Web editor                    | Export            |
| Different device frame | Edit in Web editor (Device tab)       | Export            |
| Dark mode captures     | `appearance: 'dark'` in config      | `glint capture` |
| More/fewer screenshots | Edit rules in test file               | `glint capture` |

## Gotchas

- **Debug builds paint LogBox banners** - always use Release builds for Bridge
- **Ahem font** - if text shows as boxes, ensure `flutter_test_config.dart` loads Roboto (Glint-Capture ships it)
- **session.json missing screens** - means no PNGs were captured; check the test file has real builders
- **Wrong store size in export** - check the `store` field in session.json matches the template
- **Bridge: no device** - `adb devices` must show a connected device
- **Bridge: crawl needs Appium** - `pip install Appium-Python-Client` + running Appium server

## References

- [Capture](references/capture.md) - CLI, device presets, `glint.yaml`
- [Bridge](references/bridge.md) - commands, WebSocket, crawl modes
- [Web](references/web.md) - templates, export sizes, `.glint` format
- [Session schema](references/session-schema.md) - `session.json` fields
