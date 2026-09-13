# omarchy-agent-pace

Claude Code, Codex and Fireworks usage in the [Omarchy](https://omarchy.org/)
bar — with the numbers on the bar itself and a projection of when the
allowance runs dry.

> **This is a fork of the first-party `omarchy.agents` plugin**, by David
> Heinemeier Hansson / Basecamp, MIT licensed. `Agent.qml`, `Main.qml`, the
> vendor logos and most of `Panel.qml` are upstream's work — see
> [Credit](#credit). What follows is what this fork adds on top.

## What this fork adds

**The figures are on the bar, not behind a click.** Upstream shows an icon;
this shows the session and weekly windows as live percentages
(`󱚣 41%↗ 68%→`). Model-scoped limits like "Fable Weekly" keep out of the bar
deliberately — three figures in a row is a dashboard, not a status bar.

**A pace projection.** An allowance refills on a schedule, so a window that is
40% elapsed has "afforded" 40% of its quota. Comparing what you have actually
spent against that straight line answers the only question worth asking of a
rate limit:

```
↗ On track to empty ~14:32 · 2h 10m before reset
→ Lasts past reset at this rate
```

It stays silent for the first stretch of a window — two minutes into five
hours, a single prompt extrapolates to nonsense — so a blank row means "too
early to say", not "nothing to worry about". The text grades from foreground
toward urgent as the margin narrows, mixed from the active theme.

**Session token spend that matches the window.** Upstream's collector buckets
transcript usage by calendar day, which cannot answer "what has this session
cost" — a five-hour window opens whenever it opens. The `session-tokens`
helper beside `Panel.qml` walks the same transcripts against the real window
start. Today's total comes out of that same scan, so the day can no longer be
out-totalled by the five-hour window inside it.

**A tooltip that spells out the arrows** — resets, pace, tokens and prompts
this session, tokens today.

## Install

```bash
omarchy plugin add https://github.com/dbarke/omarchy-agent-pace.git --enable
```

It installs as `dbarke.agents` and coexists with the built-in `omarchy.agents`
— enable whichever you prefer, or both.

## Requirements

- Omarchy 4.x (`omarchy-shell` / Quickshell)
- `python3` — the `session-tokens` helper
- Usage records from `omarchy-agent-usage-update`, which the panel only reads

## Settings

Unchanged from upstream: refresh interval, per-agent enable switches for
Claude / Codex / Fireworks, and the optional cross-device synced aggregation
(`syncMode`, `syncDir`, `syncFileName`, `syncDeviceId`).

## Credit

Everything this plugin knows about reading usage records, drawing the panel,
handling auth and endpoint failures, and aggregating across devices comes from
the Omarchy project:

- **Upstream:** [`shell/plugins/agents`](https://github.com/basecamp/omarchy)
  in basecamp/omarchy, MIT
- **Unmodified from upstream:** `Agent.qml`, `Main.qml`, `assets/*.svg`
- **Modified:** `Panel.qml` (upstream's, plus the bar readout, pace projection
  and session-token helper described above), `manifest.json`

The bundled Anthropic, OpenAI and Fireworks marks are the respective
trademarks of their owners, carried over from upstream and used to identify
those services.

## License

MIT, with copyright held by both upstream and this fork — see [LICENSE](LICENSE).
