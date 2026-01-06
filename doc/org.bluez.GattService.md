# org.bluez.GattService

## BlueZ D-Bus GattService API documentation

| Field | Value |
| --- | --- |
| Version | BlueZ |
| Date | October 2023 |
| Manual section | 5 |
| Manual group | Linux System Administration |

### Description

GATT local/server and remote/client services share the same high-level D-Bus
API.

Local/Server refers to a GATT-based service exported by a plugin or an external
application.

Remote/Client refers to GATT services exported by the peer.

### Interface

#### Client

- **Service:** `org.bluez`
- **Interface:** `org.bluez.GattService1`
- **Object path:** `[variable prefix]/{hci0,hci1,...}/[dev](){BDADDR}/service#`

#### Server

- **Service:** unique name
- **Interface:** `org.bluez.GattService1`
- **Object path:** freely definable

### Properties

| Property | Type | Access | Description |
| --- | --- | --- | --- |
| UUID | `string` | `read-only` | 128-bit service UUID. |
| Primary | `boolean` | `read-only` | Indicates whether this GATT service is a primary service. If false, the service is secondary. |
| Device | `object` | `read-only`, `optional` | Object path of the [org.bluez.Device](org.bluez.Device.md) the service belongs to. Only present for services from remote devices. |
| Includes | `array{object}` | `read-only`, `optional` | Array of object paths representing included services. |
| Handle | `uint16` | `read-only` (client only) | Service handle. |
| Handle | `uint16` | `read-write`, `optional` (server only) | Service handle. When available in the server, it attempts to use this handle to allocate in the database. Use `0x0000` to auto-allocate; the allocated handle will be set once registered. |
