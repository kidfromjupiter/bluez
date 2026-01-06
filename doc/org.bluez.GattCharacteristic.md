# org.bluez.GattCharacteristic

## BlueZ D-Bus GattCharacteristic API documentation

| Field | Value |
| --- | --- |
| Version | BlueZ |
| Date | October 2023 |
| Manual section | 5 |
| Manual group | Linux System Administration |

### Description

GATT local/server and remote/client characteristic attribute
representation share the same high-level D-Bus API.

Local/Server refers to GATT-based characteristics exported by a plugin or an
external application.

Remote/Client refers to GATT characteristics exported by the peer.

### Interface

#### Client

- **Service:** `org.bluez`
- **Interface:** `org.bluez.GattCharacteristic1`
- **Object path:** `[variable prefix]/{hci0,hci1,...}/[dev](){BDADDR}/service#/char#`

#### Server

- **Service:** unique name
- **Interface:** `org.bluez.GattCharacteristic1`
- **Object path:** freely definable

### Methods

#### ReadValue

Signature: `array{byte} ReadValue(dict options)`

Issues a request to read the value of the characteristic and returns the value
if the operation was successful.

Possible options:

- **offset** (`uint16`): Read start offset in bytes.
- **mtu** (`uint16`, server only): Exchanged MTU in bytes.
- **device** (`object`, server only): Device object path.
- **link** (`string`, server only): Link type. Possible values: `BR/EDR`, `LE`.

Possible errors:

- <span style="color:#c00">org.bluez.Error.Failed</span> (possible values: string `0x80` - `0x9f`)
- <span style="color:#c00">org.bluez.Error.InProgress</span>
- <span style="color:#c00">org.bluez.Error.NotPermitted</span>
- <span style="color:#c00">org.bluez.Error.NotAuthorized</span>
- <span style="color:#c00">org.bluez.Error.InvalidOffset</span>
- <span style="color:#c00">org.bluez.Error.NotSupported</span>

#### WriteValue

Signature: `void WriteValue(array{byte} value, dict options)`

Issues a request to write the value of the characteristic.

Possible options:

- **offset** (`uint16`): Write start offset in bytes.
- **type** (`string`): Possible values:
  - `command`: Use Write without response procedure.
  - `request`: Use Write with response procedure.
  - `reliable`: Use Reliable Write procedure.
- **mtu** (`uint16`, server only): Exchanged MTU.
- **device** (`object`, server only): Device path.
- **link** (`string`, server only): Link type. Possible values: `BR/EDR`, `LE`.
- **prepare-authorize** (`boolean`): True if prepare authorization request.

Possible errors:

- <span style="color:#c00">org.bluez.Error.Failed</span> (possible values: string `0x80` - `0x9f`)
- <span style="color:#c00">org.bluez.Error.InProgress</span>
- <span style="color:#c00">org.bluez.Error.NotPermitted</span>
- <span style="color:#c00">org.bluez.Error.InvalidValueLength</span>
- <span style="color:#c00">org.bluez.Error.NotAuthorized</span>
- <span style="color:#c00">org.bluez.Error.NotSupported</span>
- <span style="color:#c00">org.bluez.Error.ImproperlyConfigured</span>

#### AcquireWrite

Signature: `fd, uint16 AcquireWrite(dict options)` (optional)

Acquire file descriptor and MTU for writing. Only sockets are supported. Using
WriteValue will be locked, causing it to return NotPermitted.

For server, the MTU returned is less than or equal to the negotiated MTU.

For client, this only works with characteristics that have the **WriteAcquired**
property and rely on write-without-response **Flag**.

To release the lock, the client closes the file descriptor. A HUP is generated
if the device is disconnected.

Note: the MTU can only be negotiated once and is symmetric; this method may be
delayed until the MTU exchange completes. The file descriptor is closed during
reconnections because the MTU must be renegotiated.

Possible options:

- **device** (`object`, server only): Device object path.
- **mtu** (`uint16`, server only): Exchanged MTU.
- **link** (`string`, server only): Link type. Possible values: `BR/EDR`, `LE`.

Possible errors:

- <span style="color:#c00">org.bluez.Error.Failed</span>
- <span style="color:#c00">org.bluez.Error.NotSupported</span>

#### AcquireNotify

Signature: `fd, uint16 AcquireNotify(dict options)` (optional)

Acquire file descriptor and MTU for notify. Only sockets are supported.

Using StartNotify will be locked, causing it to return
**org.bluez.Error.NotPermitted**.

For server, the MTU returned is less than or equal to the negotiated MTU.

Only works with characteristics that have **NotifyAcquired** and rely on the
presence of the **"notify"** or **"indicate"** **Flag**, and no other client
has called **StartNotify()**.

Notifications are enabled during this procedure, so **StartNotify()** should
not be called. Notifications are dispatched via the file descriptor; the Value
property is not affected while notify has been acquired.

To release the lock, the client closes the file descriptor. A HUP is generated
if the device is disconnected.

As a client, if indication procedure is used the confirmation is generated
automatically once received. As a server, if the file descriptor is writable
(POLLOUT), then upon receiving a confirmation from the client, one byte (`0x01`)
is written to the file descriptor.

Note: the MTU can only be negotiated once and is symmetric; this method may be
delayed until the MTU exchange completes. The file descriptor is closed during
reconnections because the MTU must be renegotiated.

Possible options:

- **device** (`object`, server only): Device object path.
- **mtu** (`uint16`, server only): Exchanged MTU.
- **link** (`string`, server only): Link type. Possible values: `BR/EDR`, `LE`.

Possible errors:

- <span style="color:#c00">org.bluez.Error.Failed</span>
- <span style="color:#c00">org.bluez.Error.NotSupported</span>
- <span style="color:#c00">org.bluez.Error.NotPermitted</span>

#### StartNotify

Signature: `void StartNotify()`

Starts a notification session from this characteristic if it supports value
notifications or indications.

Possible errors:

- <span style="color:#c00">org.bluez.Error.Failed</span>
- <span style="color:#c00">org.bluez.Error.NotPermitted</span>
- <span style="color:#c00">org.bluez.Error.InProgress</span>
- <span style="color:#c00">org.bluez.Error.NotConnected</span>
- <span style="color:#c00">org.bluez.Error.NotSupported</span>

#### StopNotify

Signature: `void StopNotify()`

Stops or cancels a session previously created by **StartNotify()**.

Notifications from a characteristic are shared between sessions, so calling
StopNotify releases a single session.

Possible errors:

- <span style="color:#c00">org.bluez.Error.Failed</span>

#### Confirm

Signature: `void Confirm()` (noreply, optional, server only)

Confirms value was received.

Possible errors:

- <span style="color:#c00">org.bluez.Error.Failed</span>

### Properties

| Property | Type | Access | Description |
| --- | --- | --- | --- |
| UUID | `string` | `read-only` | 128-bit characteristic UUID. |
| Service | `object` | `read-only` | Object path of the [org.bluez.GattService](org.bluez.GattService.md) the characteristic belongs to. |
| Value | `array{byte}` | `read-only`, `optional` | Cached value of the characteristic. Updated only after a successful read and when a notification or indication is received, in which case PropertiesChanged is emitted. |
| WriteAcquired | `boolean` | `read-only`, `optional` | True if this characteristic has been acquired by any client using AcquireWrite. For client, omitted if `write-without-response` flag is not set. For server, presence indicates AcquireWrite is supported. |
| NotifyAcquired | `boolean` | `read-only`, `optional` | True if this characteristic has been acquired by any client using AcquireNotify. For client, omitted if `notify` flag is not set. For server, presence indicates AcquireNotify is supported. |
| Notifying | `boolean` | `read-only`, `optional` | True if notifications or indications are currently enabled. |
| Flags | `array{string}` | `read-only` | Defines how the characteristic value can be used. See Core spec "Table 3.5: Characteristic Properties bit field" and "Table 3.8: Characteristic Extended Properties bit field". The `x-notify` and `x-indicate` flags restrict access to notifications and indications by imposing write restrictions on a characteristic's client characteristic configuration descriptor. Possible values: `broadcast`, `read`, `write-without-response`, `write`, `notify`, `indicate`, `authenticated-signed-writes`, `extended-properties`, `reliable-write`, `writable-auxiliaries`, `encrypt-read`, `encrypt-write`, `encrypt-notify` (server only), `encrypt-indicate` (server only), `encrypt-authenticated-read`, `encrypt-authenticated-write`, `encrypt-authenticated-notify` (server only), `encrypt-authenticated-indicate` (server only), `secure-read` (server only), `secure-write` (server only), `secure-notify` (server only), `secure-indicate` (server only), `authorize`. |
| Handle | `uint16` | `read-only` (client only) | Characteristic handle. |
| Handle | `uint16` | `read-write`, `optional` (server only) | Characteristic handle. When available in the server, it attempts to use this handle to allocate in the database. Use `0x0000` to auto-allocate; the allocated handle will be set once registered. |
| MTU | `uint16` | `read-only` | Characteristic MTU, valid for both **ReadValue** and **WriteValue**, though either method can use long procedures when supported. |
