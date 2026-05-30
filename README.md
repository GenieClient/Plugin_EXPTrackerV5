# Plugin_EXPTrackerV5

The **Experience tracker** plugin for [Genie 5](https://github.com/GenieClient/Genie5) —
a port of VTCifer's `EXPTracker` for the Genie 5 platform.

Tracks skill ranks and learning rates (mindstates) and renders them to the
**Experience** dock window:

```
Learning: 4
──────────────────────────────────────
Small Edged        142 71%  examining (13/34)
Outdoorsmanship    335 82%  studious (19/34)
Astrology          432 15%  dabbling (1/34)
Scholarship        406 45%  dabbling (1/34)
```

It also publishes the per-skill `$<Skill>.Ranks`, `$<Skill>.LearningRate`
(0–34), `$<Skill>.LearningRateName` globals plus `$TDPs` for `.cmd` scripts —
the same surface the Genie 4 `EXPTracker` plugin provided.

## Requirements

- [.NET 8 SDK](https://dotnet.microsoft.com/download/dotnet/8.0) (to build).
- A Genie 5 install with the plugin system (host version ≥ 5.0).

## Build

```sh
dotnet build -c Release
```

Output: `bin/Release/net8.0/Plugin_Experience.dll`.

The project references the Genie 5 plugin contract (`Genie.Plugins.Abstractions`)
from a committed copy in `lib/`, so no NuGet feed is required to compile.

## Install

Copy `Plugin_Experience.dll` into your Genie 5 plugins folder:

- **Windows:** `%APPDATA%\Genie5\Plugins\`
- **macOS:** `~/Library/Application Support/Genie5/Plugins/`
- **Linux:** `~/.local/share/Genie5/Plugins/` (or `$XDG_DATA_HOME/Genie5/Plugins/`)

Then in Genie 5 either:

- **Reconnect** — plugins load on connect, or
- **Plugins → Load → Plugin_Experience.dll** (menu), or
- `#plugin load Plugin_Experience` (command bar).

Open the panel via **Window → Experience**, then type `exp` in-game (or train
a skill) to populate it.

## Manage

From the **Plugins** menu:

- **Enable / Disable → Experience** — toggle without unloading (panel blanks
  when disabled; re-enabling repaints on the next prompt).
- **Unload → Experience** — fully remove (releases the `.dll`).
- **Load → Plugin_Experience.dll** — load it back.

Or from the command bar:

```
#plugin list
#plugin enable  Experience
#plugin disable Experience
#plugin unload  Experience
#plugin load    Plugin_Experience
#plugin reload
#plugin folder
```

## How it works

- Reads the live `<component id='exp Skill'>…rank pct% mindstate…</component>`
  push the StormFront XML stream emits as skills pulse.
- Also parses the `exp` verb's full-dump text for the complete snapshot.
- Re-renders the Experience window on each `<prompt>` when anything changed.

The plugin has no UI dependency — it writes formatted text to a *named window*
via the host API (`SetWindow("Experience", …)`), and the host surfaces that
window as a dock panel.

## License

[GPL-3.0](LICENSE) — same as Genie 5 and the Genie 4 ecosystem.

## Credits

Behaviour ported from VTCifer's
[Plugin_EXPTracker](https://github.com/GenieClient/Plugin_EXPTracker) for
Genie 4.
