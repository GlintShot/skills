# Glint-Bridge Reference

## Agentic IDE (preferred - no API key)

The IDE agent plans. Bridge only senses and acts.

| Command | Description |
|---------|-------------|
| `python glint.py check` | Verify ADB |
| `python glint.py devices` | List USB/WiFi devices |
| `python glint.py agent launch com.app` | Open app |
| `python glint.py agent screenshot` | One PNG + session.json update |
| `python glint.py agent hierarchy` | UI targets + text |
| `python glint.py agent tap X Y` | Tap |
| `python glint.py agent scroll [forward\|backward]` | Swipe |
| `python glint.py agent back` | Back |
| `python glint.py capture` | Single shot (manual nav) |
| `python glint.py batch --count N` | N shots with delay |
| `python glint.py start` | WebSocket for Glint Studio |

MCP: `glint_bridge_launch` · `screenshot` · `hierarchy` · `tap` · `scroll` · `back`

## Headless crawl (CI)

| Command | Description |
|---------|-------------|
| `python glint.py crawl --package com.app` | Appium heuristic |
| `python glint.py crawl --package com.app --ai` | Optional: your vision key |
| `python glint.py crawl-web --url https://…` | Playwright |

## Prerequisites

- **ADB** + USB debugging
- **Python 3.10+**
- **Agentic path:** ADB only (no Appium, no API key)
- **Headless Android crawl:** Appium + `Appium-Python-Client`
- **Optional `--ai`:** `GLINT_AI_API_KEY` (or OpenAI/Anthropic) already in env - agents must not ask users for keys

## Output

```
Glint-Bridge/output/
├── screenshot_0001.png
├── …
└── session.json
```

## WebSocket

`ws://127.0.0.1:7700` - pair with token, then capture / crawl actions.
