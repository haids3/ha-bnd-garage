# B&D Garage for Home Assistant

Control a B&D SmartDoorDevices garage door hub (Basestation) from Home Assistant.

Exposes the garage door as a `cover` entity supporting open, close, and stop,
polling the hub locally over your LAN every 10 seconds.

## Requirements

- A B&D SmartDoorDevices hub (Basestation) reachable on your local network.
- The [B&D Smart Garage Access](https://www.bnd.com.au/) app, used once to
  generate an activation code for pairing.

## Installation

### HACS (recommended)

1. In HACS, go to **Integrations** → the **⋮** menu → **Custom repositories**.
2. Add this repository URL with category **Integration**.
3. Search for "B&D Garage" in HACS and install it.
4. Restart Home Assistant.

### Manual

Copy `custom_components/bnd_garage` into your Home Assistant `config/custom_components`
directory and restart Home Assistant.

## Setup

1. In the B&D Smart Garage Access app, go to **Settings → Users → your hub → Add new user**
   and set a password for that user. Note the activation code shown.
2. In Home Assistant, go to **Settings → Devices & Services → Add Integration**
   and search for "B&D Garage".
3. Enter the hub's local IP address, the activation code, and the password
   you set in step 1.

## Known limitations

- Only open/close/stop are supported — the hub does not support commanding
  the door to an arbitrary position.
- Only one hub per config entry; hubs with multiple doors are not yet supported.
- Discovery (zeroconf/DHCP) is not implemented — the hub's IP must be entered manually.

## Issues

Please report bugs and feature requests via [GitHub Issues](https://github.com/haids3/ha-bnd-garage/issues).
