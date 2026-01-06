# org.bluez.Bearer.BREDR

## BlueZ D-Bus Bearer BREDR API documentation

| Field | Value |
| --- | --- |
| Version | BlueZ |
| Date | July 2025 |
| Manual section | 5 |
| Manual group | Linux System Administration |

### Interface

- **Service:** `org.bluez`
- **Interface:** `org.bluez.Bearer.BREDR1`
- **Object path:** `[variable prefix]/{hci0,hci1,...}/[dev](){BDADDR}`

### Methods

#### Connect

Signature: `void Connect()` (experimental)

Connects all BR/EDR profiles the remote device supports that can be connected
and have been flagged as auto-connectable. If only a subset of profiles is
already connected, it will try to connect currently disconnected ones.

If at least one profile was connected successfully this method indicates
success.

Possible errors:

- <span style="color:#c00">org.bluez.Error.NotReady</span>
- <span style="color:#c00">org.bluez.Error.Failed</span>
- <span style="color:#c00">org.bluez.Error.InProgress</span>
- <span style="color:#c00">org.bluez.Error.AlreadyConnected</span>

#### Disconnect

Signature: `void Disconnect()` (experimental)

Disconnects all connected profiles and then terminates the low-level ACL
connection.

ACL connection will be terminated even if some profiles were not disconnected
properly, e.g. due to misbehaving device.

This method can also be used to cancel a preceding Connect call before a reply
has been received.

For non-trusted devices connected over BR/EDR, calling this method will disable
incoming connections until Connect is called again.

Possible errors:

- <span style="color:#c00">org.bluez.Error.NotConnected</span>

### Signals

#### Disconnected

Signature: `void Disconnected(string reason, string message)` (experimental)

Emitted when a device is disconnected, with the reason for the disconnection.

This can be used by client applications to reconnect after a timeout or unknown
disconnection, or to connect to another device depending on policy.

Possible reasons:

- `org.bluez.Reason.Unknown`
- `org.bluez.Reason.Timeout`<br>Connection timeout. The link supervision timeout has expired for a
  connection or the synchronization timeout has expired for a broadcast.
- `org.bluez.Reason.Local`<br>Connection terminated by local host. The local device terminated the
  connection, terminated synchronization with a broadcaster, or terminated
  broadcasting packets.
- `org.bluez.Reason.Remote`<br>Connection terminated by remote host. This can be due to the user
  terminating the connection or stopping broadcasting packets, low resources,
  or the remote device about to power off.
- `org.bluez.Reason.Authentication`<br>Connection terminated due to an authentication failure.
- `org.bluez.Reason.Suspend`<br>Connection terminated by local host for suspend.

### Properties

| Property | Type | Access | Description |
| --- | --- | --- | --- |
| Adapter | `object` | `readonly`, `experimental` | The object path of the [org.bluez.Adapter](org.bluez.Adapter.md) the device belongs to. |
| Paired | `boolean` | `readonly`, `experimental` | Indicates if the remote device is paired to the BR/EDR bearer. Pairing means devices exchange information to establish an encrypted connection. |
| Bonded | `boolean` | `readonly`, `experimental` | Indicates if the remote device is bonded to the BR/EDR bearer. Bonded means pairing information has been stored and will be persisted. |
| Connected | `boolean` | `readonly`, `experimental` | Indicates if the remote device is currently connected to the BR/EDR bearer. PropertiesChanged signals indicate changes to this status. |
