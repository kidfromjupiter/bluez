# org.bluez.DeviceSet

## BlueZ D-Bus DeviceSet API documentation

| Field | Value |
| --- | --- |
| Version | BlueZ |
| Date | September 2023 |
| Manual section | 5 |
| Manual group | Linux System Administration |

### Interface

- **Service:** `org.bluez`
- **Interface:** `org.bluez.DeviceSet1`
- **Object path:** `[variable prefix]/{hci0,hci1,...}/[set](){sirk}`

### Methods

#### Connect

Signature: `void Connect()` (experimental)

Connects all **devices** members of the set. Each member is connected in
sequence as they were added or loaded, following the same procedure as
[org.bluez.Device](org.bluez.Device.md) `Connect()`.

Possible errors:

- <span style="color:#c00">org.bluez.Error.NotReady</span>
- <span style="color:#c00">org.bluez.Error.Failed</span>
- <span style="color:#c00">org.bluez.Error.InProgress</span>
- <span style="color:#c00">org.bluez.Error.AlreadyConnected</span>

#### Disconnect

Signature: `void Disconnect()` (experimental)

Disconnects all **devices** members of the set. Each member is disconnected in
sequence as they were connected, following the same procedure as
[org.bluez.Device](org.bluez.Device.md) `Disconnect()`.

Possible errors:

- <span style="color:#c00">org.bluez.Error.NotConnected</span>

### Properties

| Property | Type | Access | Description |
| --- | --- | --- | --- |
| Adapter | `object` | `readonly`, `experimental` | The object path of the [org.bluez.Adapter](org.bluez.Adapter.md) the set belongs to. |
| AutoConnect | `bool` | `read-write`, `experimental` | Indicates if the **devices** members of the set should be automatically connected once any member is connected. |
| Devices | `array(object)` | `read-only`, `experimental` | List of [org.bluez.Device](org.bluez.Device.md) objects that are members of the set. |
| Size | `byte` | `read-only`, `experimental` | Set members size. |
