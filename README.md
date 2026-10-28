# Home Assistant blueprints

Automation blueprints for Zigbee wall switches and remotes (Green Power, event entities in Zigbee2MQTT).

| Blueprint | For | Import |
|---|---|---|
| [sunricher_switch.yaml](sunricher_switch.yaml) | Sunricher SR-ZGP2801K2-DIM: actions for short and long press on the upper and lower button, optionally repeated while held | [![Import](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2Fegeback%2Fhome-assistant-blueprints%2Fblob%2Fmain%2Fsunricher_switch.yaml) |
| [sunricher_four_button_switch.yaml](sunricher_four_button_switch.yaml) | 4-button remote controlling two lights: buttons 1/3 tap on and hold to brighten, buttons 2/4 tap off and hold to dim | [![Import](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2Fegeback%2Fhome-assistant-blueprints%2Fblob%2Fmain%2Fsunricher_four_button_switch.yaml) |
| [sunricher_four_button_generic.yaml](sunricher_four_button_generic.yaml) | 4-button remote: any action per button on tap, a repeated action while held, and taps on 1+3 / 2+4 together | [![Import](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2Fegeback%2Fhome-assistant-blueprints%2Fblob%2Fmain%2Fsunricher_four_button_generic.yaml) |
| [hue_dimmer_scene_cycle.yaml](hue_dimmer_scene_cycle.yaml) | Philips Hue dimmer (RWL021/RWL022): repeated presses on the On button step through scenes, as with the Hue bridge | [![Import](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2Fegeback%2Fhome-assistant-blueprints%2Fblob%2Fmain%2Fhue_dimmer_scene_cycle.yaml) |

## How they work

- The 4-button blueprints expect an `event` entity whose `event_type` is `press_N` / `release_N`
  (N = 1–4, plus `press_1_and_3` / `release_2_and_4` for two buttons), as EnOcean PTM 216Z Green Power
  switches report in Zigbee2MQTT.
- **Tap or hold** is decided by the time between the press and release events (the entity's
  state is the event time). Released before the hold delay (350 ms generic, 400 ms dual light)
  it is a tap; held longer, the hold action repeats.
- **A hold always ends.** The release starts a new run (`mode: restart`), which stops the
  repeat. If the release is lost on the radio, the repeat stops after the set maximum anyway.
- **No ghost presses.** Changes to `unavailable`/`unknown`, and an old event replayed when the
  entity comes back (e.g. after a restart), are ignored: only events stamped within the last
  5 seconds run.
- Timing and step size are under the collapsed sections; the defaults match the earlier
  behaviour, so existing automations keep working after a re-import.

### Hue dimmer scene cycle

- Expects the Zigbee2MQTT event entity of a Hue dimmer: `event_type` `press` / `press_release` /
  `hold` / `hold_release` with a `button` attribute (`on`, `off`, `up`, `down`).
- **The first On press after a pause gives the first scene**, each press within the cycle window
  (3 s) the next, and after the last it starts over. Another button ends the series.
- **No counter helper.** One run covers the whole series: `mode: single` drops the new runs and a
  `wait_for_trigger` inside the run picks up the next press.
- **Scenes or presets.** Pick scenes (any order), or leave them empty for the Hue bridge presets
  on the light: Energize, Concentrate, Read, Relax, Nightlight (6410/4292/2890/2237/2237 K) on
  lights with colour temperature or colour, and 100 % / 56 % / night brightness on lights that only dim.
- **Off, up and down are left alone**, so a direct binding from the dimmer to the light keeps
  working without Home Assistant. The binding also turns the light on at the first press; the
  automation then sets the first scene.

Requires Home Assistant 2024.10 or later. Previously hosted on a private Gitea instance;
moved here 2026-10-24.
