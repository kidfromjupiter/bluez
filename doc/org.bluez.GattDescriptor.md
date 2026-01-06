# org.bluez.GattDescriptor

## BlueZ D-Bus GattDescriptor API documentation

| Field | Value |
| --- | --- |
| Version | BlueZ |
| Date | October 2023 |
| Manual section | 5 |
| Manual group | Linux System Administration |

### Description

GATT local/server and remote/client descriptor attribute representation share
the same high-level D-Bus API.

Local/Server refers to GATT-based descriptors exported by a plugin or an
external application.

Remote/Client refers to GATT descriptors exported by the peer.

### Interface

#### Client

- **Service:** `org.bluez`
- **Interface:** `org.bluez.GattDescriptor1`
- **Object path:** `[variable prefix]/{hci0,hci1,...}/[dev](){BDADDR}/service#/char#/descriptor#`

#### Server

- **Service:** unique name
- **Interface:** `org.bluez.GattDescriptor1`
- **Object path:** freely definable

### Methods

#### ReadValue

Signature: `array{byte} ReadValue(dict flags)`

Issues a request to read the value of the descriptor and returns the value if
successful.

Possible options:

- **offset** (`uint16`): Read start offset in bytes.
- **device** (`object`, server only): Device object path.
- **link** (`string`, server only): Link type. Possible values: `BR/EDR`, `LE`.

Possible errors:

- <span style="color:#c00">org.bluez.Error.Failed</span>
- <span style="color:#c00">org.bluez.Error.InProgress</span>
- <span style="color:#c00">org.bluez.Error.NotPermitted</span>
- <span style="color:#c00">org.bluez.Error.NotAuthorized</span>
- <span style="color:#c00">org.bluez.Error.NotSupported</span>

#### WriteValue

Signature: `void WriteValue(array{byte} value, dict flags)`

Issues a request to write the value of the descriptor.

Possible flags:

- **offset** (`uint16`): Write start offset in bytes.
- **mtu** (`uint16`, server only): Exchanged MTU.
- **device** (`object`, server only): Device path.
- **link** (`string`, server only): Link type. Possible values: `BR/EDR`, `LE`.
- **prepare-authorize** (`boolean`): True if prepare authorization request.

Possible errors:

- <span style="color:#c00">org.bluez.Error.Failed</span>
- <span style="color:#c00">org.bluez.Error.InProgress</span>
- <span style="color:#c00">org.bluez.Error.NotPermitted</span>
- <span style="color:#c00">org.bluez.Error.InvalidValueLength</span>
- <span style="color:#c00">org.bluez.Error.NotAuthorized</span>
- <span style="color:#c00">org.bluez.Error.NotSupported</span>
- <span style="color:#c00">org.bluez.Error.ImproperlyConfigured</span>

### Properties

| Property | Type | Access | Description |
| --- | --- | --- | --- |
| UUID | `string` | `read-only` | 128-bit descriptor UUID. |
| Characteristic | `object` | `read-only` | Object path of the [org.bluez.GattCharacteristic](org.bluez.GattCharacteristic.md) the descriptor belongs to. |
| Value | `array{byte}` | `read-only`, `optional` | Cached value of the descriptor. Updated only after a successful read, upon which PropertiesChanged is emitted. |
| Flags | `array{string}` | `read-only` | Defines how the descriptor value can be used. Possible values: `read`, `write`, `encrypt-read`, `encrypt-write`, `encrypt-authenticated-read`, `encrypt-authenticated-write`, `secure-read` (server only), `secure-write` (server only), `authorize`. |
| Handle | `uint16` | `read-only` (client only) | Descriptor handle. |
| Handle | `uint16` | `read-write`, `optional` (server only) | Descriptor handle. When available in the server, it attempts to use this handle to allocate in the database. Use `0x0000` to auto-allocate; the allocated handle will be set once registered. |
