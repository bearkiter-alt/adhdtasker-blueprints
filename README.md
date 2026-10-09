# ADHDTasker – Announce & Lights (Home Assistant Blueprint)

A per-profile announcement blueprint for the [ADHDTasker](https://github.com/bearkiter-alt/adhdtasker-hass)
Home Assistant integration. It listens for the integration's `adhdtasker_event` bus event and, for **one
profile**, announces chore events (task added / due / completed / reward requested) on chosen speakers and
optionally flashes a light a chosen colour (with snapshot-and-restore).

Create **one instance per person** — each filters on its profile name and targets that person's devices.

Unassigned (**"Anyone"**) chores carry no profile name, so no per-person instance can match them. Switch on
**Also announce Anyone chores** to cover those — on exactly one instance, or on a dedicated instance whose
profile name matches nobody (e.g. `Anyone`).

## Requirements
- The [ADHDTasker integration](https://github.com/bearkiter-alt/adhdtasker-hass) installed and configured
  (it fires `adhdtasker_event` when its webhook receives a push from the web app).
- A TTS engine (default `tts.home_assistant_cloud`).

## Install
Blueprints don't go through HACS (HACS has no blueprint category) — they import natively:

[![Open your Home Assistant instance and show the blueprint import dialog with a specific blueprint pre-filled.](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2Fbearkiter-alt%2Fadhdtasker-blueprints%2Fblob%2Fmain%2Fannounce.yaml)

Or manually: **Settings → Automations & Scenes → Blueprints → Import Blueprint** and paste:
```
https://github.com/bearkiter-alt/adhdtasker-blueprints/blob/main/announce.yaml
```
Then **Create Automation** from *ADHDTasker – Announce & Lights*, one instance per person.

## Updating
On the **Blueprints** page, open the blueprint's ⋮ menu → **Re-import** (existing automations
keep their settings; new inputs pick up their defaults).

## Inputs
| Input | What |
|---|---|
| `profile` | The profile name to match exactly (e.g. `Lachlan`, `Bruce Muir`). |
| `events` | Which events announce: `task.added`, `interrupt.added`, `task.due`, `task.completed`, `task.approved`, `reward.requested`. *(added/interrupt/approved new in v1.2.0)* |
| `announce_unassigned` | Also announce unassigned **"Anyone"** chores, named *"anyone"* instead of a person (default **off**). Switch on for ONE instance only — two instances sharing a speaker with this on will announce the same chore twice. *(v1.2.0)* |
| `speakers` | One or more media players (e.g. a shared speaker + this person's room speaker). |
| `tts_entity` | TTS engine (default `tts.home_assistant_cloud`). |
| `lights` | Optional light(s) to flash; leave empty for none. |
| `colour` | Flash colour (RGB). |
| `flash_seconds` | Flash duration. |
| `quiet_enabled` | Quiet time on/off (default **on**) — suppresses TTS + lights in the window; events/sensors keep updating. *(v1.1.0)* |
| `quiet_start` / `quiet_end` | The quiet window (default **20:30 → 07:00**; spans midnight). *(v1.1.0)* |

## Example
- **Lachlan** → speakers: living-room pair + his room speaker; light: living-room 4D; colour: blue.
- **Bruce** → speaker: games-room display; no light.
- **Anyone** → profile `Anyone`, *Also announce Anyone chores* **on**, events `task.added` + `task.due`;
  shared speakers, no light — so unclaimed chores get called out once to the whole house.

The announcement message varies by event (e.g. *"New chore for Lachlan: Make bed."* /
*"Heads up Lachlan — Make bed is due."* / *"Nice work Lachlan! You finished Make bed for 5 points."*).
Unassigned chores are phrased for the room instead: *"New chore up for grabs: Mow the lawn. Anyone can take it."*
