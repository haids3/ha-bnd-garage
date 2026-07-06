# B&D Garage for Home Assistant

Control a B&D SmartDoorDevices garage door hub (Basestation) from Home Assistant.

Exposes the garage door as a `cover` entity supporting open, close, and stop,
polling the hub locally over your LAN every 10 seconds.

## Requirements

- A B&D SmartDoorDevices hub (Basestation) reachable on your local network.
- The [B&D Smart Garage Access](https://www.bnd.com.au/) app, used once to
  generate an activation code for pairing.

## Current limitation: private dependency

This integration depends on [bnd-garage-client](https://github.com/haids3/bnd-garage-client),
an independent client for the B&D SmartDoorDevices LAN protocol (see that
repo's README for full attribution to THE-MAVER1CK's original
protocol-reverse-engineering research). It's currently private pending
further review before wider distribution.

Practically, this means:
- Installing this integration only works for accounts with access to the
  private `bnd-garage-client` repo (its git credentials need to be available
  to wherever Home Assistant installs Python requirements).
- It is **not usable via HACS by anyone else** until `bnd-garage-client` is
  published (PyPI or a public repo). Don't submit this to the default HACS
  store in the meantime.

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

1. In the B&D Smart Garage Access app, go to **Settings → Users → your hub → Add new user**.
   Note the activation code and password it shows you — the password is
   assigned automatically, not one you choose.
2. In Home Assistant, go to **Settings → Devices & Services → Add Integration**
   and search for "B&D Garage".
3. Enter the hub's local IP address, and the activation code and password
   from step 1.

## Development

Tests need `bnd-garage-client` installed, which is a private repo (see
above). CI (`.github/workflows/test.yml`) needs a `BND_GARAGE_CLIENT_PAT`
repository secret — a GitHub personal access token with read access to
`haids3/bnd-garage-client` — to install it. Add this under
**Settings → Secrets and variables → Actions** before the test workflow will
pass.

The "Hassfest validation" job in `.github/workflows/validate.yml` is expected
to fail with `[REQUIREMENTS] ... contains a space` for the same reason: core's
hassfest rejects the `name @ git+https://...` requirement syntax outright,
with no way to opt out. This resolves itself once `bnd-garage-client` is
public and `manifest.json` can go back to a normal PyPI version pin.

## Known limitations

- Only open/close/stop are supported — the hub does not support commanding
  the door to an arbitrary position.
- Only one hub per config entry; hubs with multiple doors are not yet supported.
- Discovery (zeroconf/DHCP) is not implemented — the hub's IP must be entered manually.

## Issues

Please report bugs and feature requests via [GitHub Issues](https://github.com/haids3/ha-bnd-garage/issues).
