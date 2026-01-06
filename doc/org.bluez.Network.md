# org.bluez.Network

## BlueZ D-Bus Network API documentation

| Field | Value |
| --- | --- |
| Version | BlueZ |
| Date | October 2023 |
| Manual section | 5 |
| Manual group | Linux System Administration |

### Interface

- **Service:** `org.bluez`
- **Interface:** `org.bluez.Network1`
- **Object path:** `[variable prefix]/{hci0,hci1,...}/[dev](){BDADDR}`

### Methods

#### Connect

Signature: `string Connect(string uuid)`

Connects to the network device and returns the network interface name.

Possible UUID values:

- `panu`, `00001115-0000-1000-8000-00805f9b34fb`: Personal Network User role.
- `nap`, `00001116-0000-1000-8000-00805f9b34fb`: Network Access Point role.
- `gn`, `00001117-0000-1000-8000-00805f9b34fb`: Group Network role.

The connection will be closed and network device released either upon calling
**Disconnect** or when the client disappears from the message bus.

Possible errors:

- <span style="color:#c00">org.bluez.Error.InvalidArguments</span>
- <span style="color:#c00">org.bluez.Error.NotSupported</span>
- <span style="color:#c00">org.bluez.Error.InProgress</span>
- <span style="color:#c00">org.bluez.Error.Failed</span>

#### Disconnect

Signature: `void Disconnect()`

Disconnects from the network device.

To abort a connection attempt in case of client errors or timeouts, it is fine
to call this method.

Possible errors:

- <span style="color:#c00">org.bluez.Error.Failed</span>
- <span style="color:#c00">org.bluez.Error.NotConnected</span>

### Properties

| Property | Type | Access | Description |
| --- | --- | --- | --- |
| Connected | `boolean` | `readonly` | Indicates if the device is connected. |
| Interface | `string` | `readonly`, `optional` | Network interface name when available. |
| UUID | `string` | `readonly`, `optional` | Connection role when available. |
