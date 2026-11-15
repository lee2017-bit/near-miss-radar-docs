# Near-miss event schema (`nmr.event.v1`)

Machines and bridges send close calls to the app's ingest web trigger. The schema is vendor-neutral on purpose: any AMR fleet manager, forklift proximity system or robot bridge can map onto it.

## Request

```
POST <Ingest URL from the Setup tab>
Content-Type: application/json
X-NMR-Timestamp: <unix seconds>
X-NMR-Signature: sha256=<hex HMAC-SHA256(secret, "<timestamp>.<raw body>")>
X-NMR-Format: nmr.event.v1 | fleet-webhook | proximity-tag   (optional, default nmr.event.v1)

{"events": [ <event>, ... ]}       // or a single event object; up to 100 per request
```

The handler accepts timestamps up to 300 seconds old. Re-sending an event with the same `id` is harmless (it is ignored), so senders can retry safely.

Responses are fixed (static web trigger): `202 accepted`, `400 rejected`, `401 unauthorized`, `500 error`.

## Event

| Field | Type | Required | Notes |
| --- | --- | --- | --- |
| `schema` | string | no | `nmr.event.v1` |
| `id` | string | yes | Unique per sender, `[A-Za-z0-9._:-]`, max 64. Used for de-duplication. |
| `occurredAt` | ISO 8601 | no | Defaults to receive time. Future times are rejected. |
| `source` | `machine` \| `human` | yes | |
| `type` | string | machine: yes | `protective_stop`, `proximity_alert`, `hard_brake`; humans: `human_report` |
| `device.id` | string | machine: yes | e.g. `AMR-01`, `FL-12` |
| `device.kind` | string | no | `amr`, `agv`, `forklift`, `robot`, `other` |
| `location.x`, `location.y` | number (m) | one of | Floor plan coordinates, origin top-left |
| `location.zoneId` | string | one of | Use when the sender only knows the zone |
| `severity` | 1-3 | no | 3 = very close (under ~0.8 m or a protective stop with a person in the field) |
| `minDistanceM` | number | no | Closest distance measured |
| `note` | string | human only | Max 500 characters |

Not accepted, by design: names, badge or employee IDs, images, video, injury or health data. Near-Miss Radar measures exposure to close calls; it is not an injury register and not a functional safety system.

## Adapters

`fleet-webhook` (AMR fleet manager):

```json
{"robotId":"AMR-02","sequence":881,"event":"SAFETY_FIELD_STOP","timestamp":"2026-12-01T10:15:00Z","pose":{"x":33.4,"y":15.2},"fieldLevel":"protective","minDistanceM":0.4}
```

`proximity-tag` (forklift pedestrian-proximity system):

```json
{"truck":"FL-12","alert_id":"a-77","alert":"PEDESTRIAN_PROXIMITY","distance_cm":120,"zone":"C2","ts":"2026-12-01T10:16:00Z"}
```

## Senders in this repository

- `tools/send_events.py`: signed sample events from any machine with Python 3 (no robot).
- `tools/robot_bridge/nmr_bridge.py`: ROS 2 node for TurtleBot3 or any robot publishing `/scan`, `/odom`, `/cmd_vel`.
