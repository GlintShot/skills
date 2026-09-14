# Glint agent skills

Agent skill for [Glint](https://github.com/GlintShot) - create Play Store and App Store screenshots from real app UI.

**Docs:** https://glintshot.github.io/Glint-Docs/

## Install

```bash
npx skills add GlintShot/skills
```

## How to use it

1. Run the install command in your app repo (or globally, if your agent supports that).
2. Open the project in **Cursor**, **Claude Code**, or another skills-aware agent.
3. Ask in plain language, for example:
   - *Create store screenshots for this app*
   - *Capture Play Store screens with Glint*
4. The agent loads the `glint` skill and follows Capture → Web → export (or Bridge / MCP when that fits).

You still need Glint tools available for the path you use (Capture CLI, Bridge + device, or Web). See the [docs](https://glintshot.github.io/Glint-Docs/).

## Layout

```
glint/
  SKILL.md           # when / how to run Capture, Bridge, Web, MCP
  references/        # deeper CLI and schema notes (loaded on demand)
```

## Related repos

| Repo | Role |
|------|------|
| [Glint-Capture](https://github.com/GlintShot/Glint-Capture) | Flutter capture |
| [Glint-Bridge](https://github.com/GlintShot/Glint-Bridge) | Device / ADB capture |
| [Glint-Web](https://github.com/GlintShot/Glint-Web) | Frames editor + ZIP |
| [Glint-MCP](https://github.com/GlintShot/Glint-MCP) | Agent tools |
| [Glint-Docs](https://github.com/GlintShot/Glint-Docs) | Public documentation |

## License

MIT - see [LICENSE](LICENSE).
