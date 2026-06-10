# ADHDTasker – Announce & Lights (Home Assistant Blueprint)

A per-profile announcement blueprint for the [ADHDTasker](https://github.com/bearkiter-alt/adhdtasker-hass)
Home Assistant integration. It listens for the integration's `adhdtasker_event` bus event and, for **one
profile**, announces chore events (task due / completed / reward requested) on chosen speakers and optionally
flashes a light a chosen colour (with snapshot-and-restore).

Create **one instance per person** — each filters on its profile name and targets that person's devices.

## Requirements
- The [ADHDTasker integration](https://github.com/bearkiter-alt/adhdtasker-hass) installed and configured
  (it fires `adhdtasker_event` when its webhook receives a push from the web app).
- A TTS engine (default `tts.home_assistant_cloud`).

## Install (HACS)
1. HACS → ⋮ → **Custom repositories** → add `https://github.com/bearkiter-alt/adhdtasker-blueprints`,
   category **Blueprint**.
2. Download it, then **Settings → Automations & Scenes → Blueprints → Create Automation** from
   *ADHDTasker – Announce & Lights*.

Or import directly: **Settings → Blueprints → Import Blueprint** and paste the raw URL of `announce.yaml`.

## Inputs
| Input | What |
|---|---|
| `profile` | The profile name to match exactly (e.g. `Lachlan`, `Bruce Muir`). |
| `events` | Which events announce: `task.due`, `task.completed`, `reward.requested`. |
| `speakers` | One or more media players (e.g. a shared speaker + this person's room speaker). |
| `tts_entity` | TTS engine (default `tts.home_assistant_cloud`). |
| `lights` | Optional light(s) to flash; leave empty for none. |
| `colour` | Flash colour (RGB). |
| `flash_seconds` | Flash duration. |

## Example
- **Lachlan** → speakers: living-room pair + his room speaker; light: living-room 4D; colour: blue.
- **Bruce** → speaker: games-room display; no light.

The announcement message varies by event (e.g. *"Heads up Lachlan — Make bed is due."* /
*"Nice work Lachlan! You finished Make bed for 5 points."*).
