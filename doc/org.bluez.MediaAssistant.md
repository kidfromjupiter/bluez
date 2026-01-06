# org.bluez.MediaAssistant

## BlueZ D-Bus MediaAssistant API documentation

| Field | Value |
| --- | --- |
| Version | BlueZ |
| Date | June 2024 |
| Manual section | 5 |
| Manual group | Linux System Administration |

### Interface

- **Service:** `org.bluez`
- **Interface:** `org.bluez.MediaAssistant1`
- **Object path:** `/org/bluez/{hci0,hci1,...}/[dev](){BDADDR}/[src](){BDADDR}/sid#/bis#`

### Methods

#### Push

Signature: `void Push(dict properties)`

Send stream information to the remote device.

`properties` indicate stream properties that will be sent to the peer:

- **Metadata** (`array{byte}`, ISO only): See **Metadata** property.
- **QoS** (`dict`, ISO only): See **QoS** property.
- **Device** (`object`, ISO only, State=`local` only): Push to a specific device.
  Device must be connected with an active BASS session.

### Properties

| Property | Type | Access | Description |
| --- | --- | --- | --- |
| State | `string` | `readonly` | Assistant object state. Possible values: `idle` (created for the stream), `pending` (pushed to peer), `requesting` (remote device requires Broadcast_Code), `active` (remote device started receiving stream), `local` (created for a local stream). |
| Metadata | `array{byte}` | `readwrite`, ISO only, experimental | Stream metadata. |
| QoS | `dict` | `readwrite`, ISO only, experimental | Stream QoS capabilities. Values: `Encryption` (`byte`, whether the stream is encrypted), `BCode` (`array{byte}`, Broadcast_Code to decrypt stream). |
