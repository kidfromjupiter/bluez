# org.bluez.Device

## BlueZ D-Bus Device API documentation

| Field | Value |
| --- | --- |
| Version | BlueZ |
| Date | October 2023 |
| Manual section | 5 |
| Manual group | Linux System Administration |

### Interface

- **Service:** `org.bluez`
- **Interface:** `org.bluez.Device1`
- **Object path:** `[variable prefix]/{hci0,hci1,...}/[dev](){BDADDR}`

### Methods

#### Connect

Signature: `void Connect()`

Connects all profiles the remote device supports that can be connected to and
have been flagged as auto-connectable. If only a subset of profiles is already
connected, it will try to connect currently disconnected ones.

If at least one profile was connected successfully, this method indicates
success.

For dual-mode devices only one bearer is connected at a time. The conditions
are in the following order:

1. Connect the disconnected bearer if already connected.
2. Connect the bonded bearer first. If no bearers are bonded or both are
   connected, skip and check the latest seen bearer.
3. Connect the last used bearer. If timestamps match, BR/EDR takes precedence,
   or if **PreferredBearer** is set to a specific bearer then that is used.

Possible errors:

- <span style="color:#c00">org.bluez.Error.NotReady</span>
- <span style="color:#c00">org.bluez.Error.Failed</span>
- <span style="color:#c00">org.bluez.Error.InProgress</span>
- <span style="color:#c00">org.bluez.Error.AlreadyConnected</span>
- <span style="color:#c00">org.bluez.Error.BREDR.ProfileUnavailable</span>

#### Disconnect

Signature: `void Disconnect()`

Disconnects all connected profiles and then terminates the low-level ACL
connection.

ACL connection will be terminated even if some profiles were not disconnected
properly, e.g. due to misbehaving device.

This method can also be used to cancel a preceding Connect call before a reply
has been received.

For non-trusted devices connected over an LE bearer, calling this method
disables incoming connections until Connect is called again.

Possible errors:

- <span style="color:#c00">org.bluez.Error.NotConnected</span>

#### ConnectProfile

Signature: `void ConnectProfile(string uuid)`

Connects a specific profile of this device. The UUID provided is the remote
service UUID for the profile.

Possible errors:

- <span style="color:#c00">org.bluez.Error.Failed</span>
- <span style="color:#c00">org.bluez.Error.InProgress</span>
- <span style="color:#c00">org.bluez.Error.InvalidArguments</span>
- <span style="color:#c00">org.bluez.Error.NotAvailable</span>
- <span style="color:#c00">org.bluez.Error.NotReady</span>

#### DisconnectProfile

Signature: `void DisconnectProfile(string uuid)`

Disconnects a specific profile of this device. The profile needs to be a
registered client profile.

There is no connection tracking for a profile, so as long as the profile is
registered this will always succeed.

Possible errors:

- <span style="color:#c00">org.bluez.Error.Failed</span>
- <span style="color:#c00">org.bluez.Error.InProgress</span>
- <span style="color:#c00">org.bluez.Error.InvalidArguments</span>
- <span style="color:#c00">org.bluez.Error.NotSupported</span>

#### Pair

Signature: `void Pair()`

Connects to the remote device and initiates the pairing procedure, then
proceeds with service discovery.

If the application has registered its own agent, that specific agent is used.
Otherwise it uses the default agent.

Only for applications like a pairing wizard does it make sense to have its own
agent. In almost all other cases the default agent will handle this just fine.

If there is no application agent and no default agent present, this method
fails.

Possible errors:

- <span style="color:#c00">org.bluez.Error.InvalidArguments</span>
- <span style="color:#c00">org.bluez.Error.Failed</span>
- <span style="color:#c00">org.bluez.Error.AlreadyExists</span>
- <span style="color:#c00">org.bluez.Error.AuthenticationCanceled</span>
- <span style="color:#c00">org.bluez.Error.AuthenticationFailed</span>
- <span style="color:#c00">org.bluez.Error.AuthenticationRejected</span>
- <span style="color:#c00">org.bluez.Error.AuthenticationTimeout</span>
- <span style="color:#c00">org.bluez.Error.ConnectionAttemptFailed</span>

#### CancelPairing

Signature: `void CancelPairing()`

Cancels a pairing operation initiated by the **Pair** method.

Possible errors:

- <span style="color:#c00">org.bluez.Error.DoesNotExist</span>
- <span style="color:#c00">org.bluez.Error.Failed</span>

#### GetServiceRecords

Signature: `array{array{byte}} GetServiceRecords()` (experimental)

Returns all currently known BR/EDR service records for the device. Each
individual byte array represents a raw SDP record, as defined by the Bluetooth
Service Discovery Protocol specification.

This method is intended to be used only by compatibility layers like Wine that
need access to raw SDP records to support foreign Bluetooth APIs.

General applications should instead use the Profile API for services-related
functionality. See [org.bluez.Profile](org.bluez.Profile.md).

Possible errors:

- <span style="color:#c00">org.bluez.Error.Failed</span>
- <span style="color:#c00">org.bluez.Error.NotReady</span>
- <span style="color:#c00">org.bluez.Error.NotConnected</span>
- <span style="color:#c00">org.bluez.Error.DoesNotExist</span>

### Signals

#### Disconnected

Signature: `void Disconnected(string reason, string message)`

Emitted when a device is disconnected, with the reason for the
disconnection.

This can be used by client applications to try to reconnect after a timeout or
unknown disconnection, or to connect to another device depending on policy.

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
| Address | `string` | `readonly` | The Bluetooth device address of the remote device. |
| AddressType | `string` | `readonly` | The Bluetooth device Address Type. For dual-mode and BR/EDR-only devices this defaults to "public". Single-mode LE devices may have either value.<br><br>If the remote device uses privacy, then before pairing this represents the address type used for connection and the Identity Address after pairing.<br><br>Possible values:<br>- `public`: Public address<br>- `random`: Random address |
| Name | `string` | `readonly`, `optional` | The Bluetooth remote name.<br><br>This value is only present for completeness. Prefer the **Alias** property when displaying the device name. If **Alias** is unset, it reflects this value. |
| Icon | `string` | `readonly`, `optional` | Proposed icon name according to the freedesktop.org icon naming specification. |
| Class | `uint32` | `readonly`, `optional` | The Bluetooth class of device of the remote device. |
| Appearance | `uint16` | `readonly`, `optional` | External appearance of device, as found on GAP service. |
| UUIDs | `array{string}` | `readonly`, `optional` | List of 128-bit UUIDs that represent the available remote services. |
| Paired | `boolean` | `readonly` | Indicates if the remote device is paired. Paired means the pairing process where devices exchange information to establish an encrypted connection has completed. |
| Bonded | `boolean` | `readonly` | Indicates if the remote device is bonded. Bonded means the information exchanged during pairing has been stored and will be persisted. |
| Connected | `boolean` | `readonly` | Indicates if the remote device is currently connected.<br><br>A PropertiesChanged signal indicates changes to this status. |
| Trusted | `boolean` | `readwrite` | Indicates if the remote is seen as trusted. This setting can be changed by the application. |
| Blocked | `boolean` | `readwrite` | If set to true, any incoming connections from the device will be immediately rejected.<br><br>Any device drivers will also be removed and no new ones will be probed while the device is blocked. |
| WakeAllowed | `boolean` | `readwrite` | If set to true, this device is allowed to wake the host from system suspend. |
| Alias | `string` | `readwrite` | The name alias for the remote device. The alias can be used to have a different friendly name for the remote device.<br><br>If no alias is set, it returns the remote device name. Setting an empty string as alias resets it to the remote device name. |
| Adapter | `object` | `readonly` | The object path of the [org.bluez.Adapter](org.bluez.Adapter.md) the device belongs to. |
| LegacyPairing | `boolean` | `readonly` | Set to true if the device only supports the pre-2.1 pairing mechanism.<br><br>This property is useful during device discovery to anticipate whether legacy or simple pairing will occur if pairing is initiated.<br><br>Note: This property can exhibit false-positives in the case of Bluetooth 2.1 (or newer) devices that have disabled Extended Inquiry Response support. |
| CablePairing | `boolean` | `readonly` | Set to true if the device was cable paired and does not support canonical bonding with encryption (e.g. the Sixaxis gamepad). If true, BlueZ will establish a connection without enforcing encryption. |
| Modalias | `string` | `readonly`, `optional` | Remote Device ID information in modalias format used by the kernel and udev. |
| RSSI | `int16` | `readonly`, `optional` | Received Signal Strength Indicator of the remote device (inquiry or advertising). |
| TxPower | `int16` | `readonly`, `optional` | Advertised transmitted power level (inquiry or advertising). |
| ManufacturerData | `dict` | `readonly`, `optional` | Manufacturer-specific advertisement data. Keys are 16-bit Manufacturer IDs followed by their byte array value. |
| ServiceData | `dict` | `readonly`, `optional` | Service advertisement data. Keys are UUIDs in string format followed by their byte array value. |
| ServicesResolved | `bool` | `readonly` | Indicates whether or not service discovery has been resolved. |
| AdvertisingFlags | `array{byte}` | `readonly` | The Advertising Data Flags of the remote device. |
| AdvertisingData | `dict` | `readonly` | The Advertising Data of the remote device. Keys are 1-byte AD Type followed by data as byte array.<br><br>Note: Only types considered safe to be handled by applications are exposed.<br><br>Possible values: `&lt;type&gt;` → `&lt;byte array&gt;`<br>Example: `&lt;Transport Discovery&gt; &lt;Organization Flags...&gt; 0x26 0x01 0x01...` |
| Sets | `array{object, dict}` | `readonly`, `experimental` | The object paths of the sets the device belongs to followed by a dictionary which can contain the following:<br>- `byte Rank`: Rank of the device in the Set. |
| PreferredBearer | `string` | `readwrite`, `optional`, `experimental` | Indicates the preferred bearer when initiating a connection, only available for dual-mode devices.<br><br>When changing from "bredr" to "le", the device will be removed from the auto-connect list so it won't automatically be connected when advertising.<br><br>Note: Changes only take effect when the device is disconnected.<br><br>Possible values:<br>- `last-used`: Connect to last used bearer first (default).<br>- `bredr`: Connect to BR/EDR first.<br>- `le`: Connect to LE first.<br>- `last-seen`: Connect to last seen bearer first. |
