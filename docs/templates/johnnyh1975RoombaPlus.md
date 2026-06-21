# Roomba+ (`johnnyh1975_roomba_plus`)

Platform template for the [Roomba+](https://github.com/johnnyh1975/ha_roomba_plus) Home Assistant custom integration — a HACS Gold-quality local-push integration for iRobot Roomba and Braava devices.

**Requires:** Roomba+ **v2.7.3** or later, iRobot cloud credentials configured, SMART-capable robot (i-series, s-series, j-series).

---

## Map source entity

Roomba+ exposes a **Rooms Map** image entity that provides both the floor plan PNG and the `calibration_points` / `rooms` attributes XVMC reads directly.

Find your entity under Developer Tools → States, filtering for `image.`:

| Installation type | Entity ID |
|---|---|
| Fresh install (v2.7.1+) | `image.{prefix}_rooms_map` |
| Upgraded from earlier version | `image.{prefix}_rooms_cleaning_map` |

Both expose identical attributes.

---

## Minimal configuration

```yaml
type: custom:xiaomi-vacuum-map-card
entity: vacuum.roomba
vacuum_platform: johnnyh1975_roomba_plus
map_source:
  camera: image.roomba_rooms_map
calibration_source:
  camera: true
map_modes:
  - template: vacuum_clean_segment
```

The card reads `calibration_points` (vacuum mm → image pixel transform) and `rooms` (polygon outlines, labels, icons) from the map entity automatically.

---

## Room IDs and special characters

The `rooms` attribute on the map entity exposes a `room_id` field alongside the display name for each room. `room_id` is an ASCII slug derived from the display name — safe to use as a `predefined_selections.id` value in XVMC:

```yaml
# From Developer Tools → States → image.roomba_rooms_map → rooms attribute:
# "Küche": {"room_id": "kuche", "name": "Küche", "outline": [...], "x": ..., "y": ...}

predefined_selections:
  - id: "kuche"           # ← room_id (ASCII-safe, used by XVMC as selection id)
    outline:
      - [320.5, 410.2]    # ← from rooms.Küche.outline (vacuum mm)
      - [600.0, 410.2]
      - [600.0, 720.8]
      - [320.5, 720.8]
    label:
      text: "Küche"       # ← from rooms.Küche.name
      x: 460.0            # ← from rooms.Küche.x
      y: 565.0            # ← from rooms.Küche.y
    icon:
      name: mdi:fridge    # ← from rooms.Küche.icon
      x: 460.0
      y: 565.0
```

The `clean_room` service accepts both the display name and the `room_id` slug — no manual mapping needed.

---

## Available map modes

| Mode | Service | Description |
|---|---|---|
| `vacuum_clean_segment` | `roomba_plus.clean_room` | Standard room clean, ordered |
| `vacuum_clean_segment_two_pass` | `roomba_plus.clean_room` | Two-pass clean (overrides robot setting) |
| `vacuum_clean_segment_with_blocking` | `roomba_plus.smart_start` | Respects door/occupancy blocking sensors |

---

## Full working config with predefined_selections

```yaml
type: custom:xiaomi-vacuum-map-card
entity: vacuum.roomba
vacuum_platform: johnnyh1975_roomba_plus
map_source:
  camera: image.roomba_rooms_map
calibration_source:
  camera: true
map_modes:
  - template: vacuum_clean_segment
    predefined_selections:
      - id: "kitchen"
        outline:
          - [320.5, 410.2]
          - [600.0, 410.2]
          - [600.0, 720.8]
          - [320.5, 720.8]
        label:
          text: "Kitchen"
          x: 460.0
          y: 565.0
        icon:
          name: mdi:fridge
          x: 460.0
          y: 565.0
      - id: "living_room"
        outline:
          - [100.0, 100.0]
          - [320.0, 100.0]
          - [320.0, 500.0]
          - [100.0, 500.0]
        label:
          text: "Living Room"
          x: 210.0
          y: 300.0
        icon:
          name: mdi:sofa
          x: 210.0
          y: 300.0
  - template: vacuum_clean_segment_two_pass
    predefined_selections:
      # same entries as above
```

Get coordinates from `Developer Tools → Template`:
```
{{ state_attr('image.roomba_rooms_map', 'rooms') | tojson(indent=2) }}
```

---

## Alignment status

Room outlines require UmfAligner calibration. Check:
```
{{ state_attr('image.roomba_rooms_map', 'alignment_pending') }}
```

`true` = pose alignment not yet complete. Coordinates remain accurate in UMF-space fallback mode — XVMC overlays will display correctly regardless.

---

## Full documentation

[Roomba+ XVMC integration guide](https://github.com/johnnyh1975/ha_roomba_plus/blob/main/docs/xiaomi-vacuum-map-card.md)
