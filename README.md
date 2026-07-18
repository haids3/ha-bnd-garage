# B&D Garage for Home Assistant

Control a B&D SmartDoorDevices garage door hub (Basestation) from Home Assistant.

Polls the hub locally over your LAN every 10 seconds and exposes:

- A `cover` entity: open, close, stop, and set to an exact position.
- A `light` entity, if the hub has one wired.
- A `button` per position preset configured in the vendor app (e.g. "Pet", "Parcel").
- A `switch` for the auxiliary relay output, if the hub has one wired.
- `switch` entities for remote-control and phone lockout, if the hub reports them.
- A diagnostic `sensor` showing the hub's own last-action log entry.

## Requirements

- A B&D SmartDoorDevices hub (Basestation) reachable on your local network.
- The [B&D Smart Garage Access](https://www.bnd.com.au/) app, used once to
  generate an activation code for pairing.

## Dependency

This integration depends on [bnd-garage-client](https://github.com/haids3/bnd-garage-client),
an independent client for the B&D SmartDoorDevices LAN protocol (see that
repo's README for full attribution to THE-MAVER1CK's original
protocol-reverse-engineering research). It's public, but not yet published
to PyPI, so `manifest.json` pins it via a git commit hash rather than a
version number — HACS installs fetch it automatically, no extra setup needed.

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

Tests need `bnd-garage-client` installed — `pip install -r requirements_test.txt`
pulls it straight from its public repo, no credentials required.

The "Hassfest validation" job in `.github/workflows/validate.yml` is expected
to fail with `[REQUIREMENTS] ... contains a space`: core's hassfest rejects
the `name @ git+https://...` requirement syntax outright, with no way to opt
out. This resolves once `bnd-garage-client` is published to PyPI and
`manifest.json` can use a normal version pin instead.

## Known limitations

- Setting an exact door position only supports 5% increments (5-95%) - the
  hub doesn't support arbitrary percentages. The door also settles roughly
  ±1% off the requested value (a mechanical tolerance, not a bug) - e.g. a
  target of 50% may settle at 49%.
- **Phone lockout switch**: turning this on blocks *all* app-based control of
  the door - including this integration - until it's turned back off. Status
  reads keep working, and turning it back off is never itself blocked, so
  there's no risk of a permanent lockout, but expect the cover and other
  switches to stop responding to commands while it's on.
- Only one hub per config entry; hubs with multiple doors are not yet supported.
- Discovery (zeroconf/DHCP) is not implemented — the hub's IP must be entered manually.
- Implemented in the underlying client library but not yet exposed as
  entities here: hub info, device activity log history, WiFi diagnostics,
  notification history, and advanced parameters (auto-close/light timers).

## Issues

Please report bugs and feature requests via [GitHub Issues](https://github.com/haids3/ha-bnd-garage/issues).
