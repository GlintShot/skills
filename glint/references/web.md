# Glint-Web Reference

## Templates

### Enabled by Default
- `glint-gold-play` - Flagship, Play Store phone
- `glint-gold-ios` - Flagship, App Store iPhone
- `glint-gold-ipad` - Flagship, App Store iPad
- `blink-play` - Clean, Play Store phone
- `blink-ios` - Clean, App Store iPhone
- `blink-tablet` - Clean, tablet

### Template Family System
Templates use `common.json` + platform variants (`play.json`, `ios.json`, `tablet.json`) merged at load time.

## Export Sizes

| Target | Dimensions | Default Frame |
|--------|-----------|---------------|
| Play Phone | 1080×1920 | Pixel 9 |
| Play 7" Tablet | 1200×1920 | iPad Pro |
| Play 10" Tablet | 1600×2560 | iPad Pro 13" |
| Play TV | 1920×1080 | TV |
| Play Wear | 450×450 | None |
| Play Chromebook | 1920×1080 | iPad Pro 13" |
| iOS iPhone | 1290×2796 | iPhone 16 Pro Max |
| iOS iPad | 2048×2732 | iPad Pro 13" |

## Device Frames (13 bezels)

- **Android:** Pixel 9, Galaxy S24, Galaxy S21, Galaxy S21 Ultra
- **iOS:** iPhone 16 Pro, iPhone 16 Pro Max, iPhone 13 Pro, iPhone 13 Pro Max
- **iPad:** iPad Pro 13", iPad Pro 11", iPad Air 2020
- **Other:** TV (SVG)

## Import Formats

1. **Folder import:** `session.json` + PNG files
2. **.glint project:** Binary round-trip with Fabric canvas JSON
3. **Drag-and-drop:** Direct PNG/JPG/WebP upload

## Export Formats

1. **PNG ZIP:** `Frame_1.png`, `Frame_2.png`, ... in `Glint-ss.zip`
2. **SVG ZIP:** Same but SVG markup
3. **.glint project:** Full round-trip format

## Copilot (shared board)

Each editor tab that clicks **Allow agent** gets a unique 4-character **board code** (also in the tab title as `[Glint CODE]`).

Agents must:
1. Ask the user for that code (do not open a new tab)
2. Attach to `window.__GLINT_COPILOT__` on that page
3. Pass `pairCode` on `dispatch`
4. Stop on Pause until Resume

```js
__GLINT_COPILOT__.listBoards()
__GLINT_COPILOT__.isThisBoard('K7MP')
__GLINT_COPILOT__.dispatch('setDeviceScale', { frameIndex: 0, pct: 90 }, { pairCode: 'K7MP' })
// Board pass: cursor walks all frames, shared transform from source
__GLINT_COPILOT__.boardPass({ pairCode: 'K7MP' })
```

CLI (user Chrome with `--remote-debugging-port=9222`):

```bash
node scripts/copilot-attach.mjs --pair K7MP
```

## Headless Export

```bash
cd Glint-Web && npm run build && npm run preview &
node scripts/headless-export.mjs \
  --session <session-dir> \
  --template <template-id> \
  --out <output.zip>
```

## Store Verification

Export includes verification against store specs:
- Frame count (2-8 recommended)
- Store target validity
- Dimensions match expected store sizes
