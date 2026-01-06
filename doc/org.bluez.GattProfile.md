# org.bluez.GattProfile

## BlueZ D-Bus GattProfile API documentation

| Field | Value |
| --- | --- |
| Version | BlueZ |
| Date | October 2023 |
| Manual section | 5 |
| Manual group | Linux System Administration |

### Description

Local profile (GATT client) instance. By registering this type of object an
application indicates support for a specific GATT profile and requests
automatic connections to devices supporting it.

Profiles are registered through
[org.bluez.GattManager](org.bluez.GattManager.md).

### Interface

- **Service:** `<application dependent>`
- **Interface:** `org.bluez.GattProfile1`
- **Object path:** `<application dependent>`

### Methods

#### Release

Signature: `void Release()`

Called when **bluetoothd(8)** unregisters the profile.

The profile can use it to do cleanup. There is no need to unregister the
profile manually because it has already been unregistered by the time this
method is called.

### Properties

| Property | Type | Access | Description |
| --- | --- | --- | --- |
| UUIDs | `array{string}` | `read-only` | 128-bit GATT service UUIDs to auto-connect. |
