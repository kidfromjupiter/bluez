# org.bluez.obex.Session

## BlueZ D-Bus OBEX Client API documentation

| Field | Value |
| --- | --- |
| Version | BlueZ |
| Date | October 2023 |
| Manual section | 5 |
| Manual group | Linux System Administration |

### Interface

- **Service:** `org.bluez.obex`
- **Interface:** `org.bluez.obex.Session1`
- **Object path:** `/org/bluez/obex/server/session{#}` or `/org/bluez/obex/client/session{#}`

Sessions are created via
[org.bluez.obex.Client](org.bluez.obex.Client.md).

### Methods

#### GetCapabilities

Signature: `string GetCapabilities()`

Get remote device capabilities.

Possible errors:

- <span style="color:#c00">org.bluez.obex.Error.NotSupported</span>
- <span style="color:#c00">org.bluez.obex.Error.Failed</span>

### Properties

| Property | Type | Access | Description |
| --- | --- | --- | --- |
| Source | `string` | `readonly` | Bluetooth adapter address. |
| Destination | `string` | `readonly` | Bluetooth device address. |
| Channel | `byte` | `readonly` | Bluetooth channel. |
| PSM | `uint16` | `readonly` | Bluetooth L2CAP PSM. |
| Target | `string` | `readonly` | Target UUID. |
| Root | `string` | `readonly` | Root path. |
