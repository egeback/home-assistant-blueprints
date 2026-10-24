# Home Assistant blueprints

Automation blueprints for Zigbee wall switches and remotes.

| Blueprint | For | Import |
|---|---|---|
| [sunricher_switch.yaml](sunricher_switch.yaml) | Sunricher SR-ZGP2801K2-DIM: actions per button press type (on, off, move up/down, stop) | [![Import](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2Fegeback%2Fhome-assistant-blueprints%2Fblob%2Fmain%2Fsunricher_switch.yaml) |
| [sunricher_four_button_switch.yaml](sunricher_four_button_switch.yaml) | 4-button remote (event entity) controlling two lights: buttons 1/3 on + brighten, buttons 2/4 tap off, hold to dim | [![Import](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2Fegeback%2Fhome-assistant-blueprints%2Fblob%2Fmain%2Fsunricher_four_button_switch.yaml) |
| [sunricher_four_button_generic.yaml](sunricher_four_button_generic.yaml) | 4-button remote (event entity): any action per button on tap, repeated action while held | [![Import](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2Fegeback%2Fhome-assistant-blueprints%2Fblob%2Fmain%2Fsunricher_four_button_generic.yaml) |

The 4-button blueprints expect an `event` entity whose `event_type` is `press_N` / `release_N` (N = 1–4).
A release within 350 ms (generic) or 400 ms (dual light) counts as a tap.

Previously hosted on a private Gitea instance; moved here 2026-10-24.
