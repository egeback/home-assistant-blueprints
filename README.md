# Home Assistant blueprints

Automation blueprints for Zigbee wall switches and remotes (Green Power, event entities in Zigbee2MQTT).

| Blueprint | For | Import |
|---|---|---|
| [sunricher_switch.yaml](sunricher_switch.yaml) | Sunricher SR-ZGP2801K2-DIM: actions for short and long press on the upper and lower button, optionally repeated while held | [![Import](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2Fegeback%2Fhome-assistant-blueprints%2Fblob%2Fmain%2Fsunricher_switch.yaml) |
| [sunricher_four_button_switch.yaml](sunricher_four_button_switch.yaml) | 4-button remote controlling two lights: buttons 1/3 tap on and hold to brighten, buttons 2/4 tap off and hold to dim | [![Import](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2Fegeback%2Fhome-assistant-blueprints%2Fblob%2Fmain%2Fsunricher_four_button_switch.yaml) |
| [sunricher_four_button_generic.yaml](sunricher_four_button_generic.yaml) | 4-button remote: any action per button on tap, a repeated action while held, and taps on 1+3 / 2+4 together | [![Import](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2Fegeback%2Fhome-assistant-blueprints%2Fblob%2Fmain%2Fsunricher_four_button_generic.yaml) |

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

Requires Home Assistant 2024.10 or later. Previously hosted on a private Gitea instance;
moved here 2026-10-24.
