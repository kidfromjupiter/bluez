# org.bluez.Adapter

## BlueZ D-Bus Adapter API documentation

| Field | Value |
| --- | --- |
| Version | BlueZ |
| Date | October 2023 |
| Manual section | 5 |
| Manual group | Linux System Administration |

### Interface

- **Service:** `org.bluez`
- **Interface:** `org.bluez.Adapter1`
- **Object path:** `[variable prefix]/{hci0,hci1,...}`

### Methods

#### StartDiscovery

Signature: `void StartDiscovery()`

Starts a device discovery session which may include starting an inquiry and/or
scanning procedures and remote device name resolving.

Use **StopDiscovery** to release the sessions acquired.

This process starts creating [org.bluez.Device](org.bluez.Device.md) objects as
new devices are discovered.

During discovery, an RSSI delta-threshold is imposed.

Each client can request a single device discovery session per adapter.

Possible errors:

- <span style="color:#c00">org.bluez.Error.NotReady</span>
- <span style="color:#c00">org.bluez.Error.Failed</span>
- <span style="color:#c00">org.bluez.Error.InProgress</span>

#### StopDiscovery

Signature: `void StopDiscovery()`

Stops the device discovery session started by **StartDiscovery**.

A discovery procedure is shared between all discovery sessions. Calling
StopDiscovery only releases a single session and discovery stops when all
sessions from all clients have finished.

Possible errors:

- <span style="color:#c00">org.bluez.Error.NotReady</span>
- <span style="color:#c00">org.bluez.Error.Failed</span>
- <span style="color:#c00">org.bluez.Error.NotAuthorized</span>

#### RemoveDevice

Signature: `void RemoveDevice(object device)`

Removes the remote device object at the given path including cached
information such as bonding information.

Possible errors:

- <span style="color:#c00">org.bluez.Error.InvalidArguments</span>
- <span style="color:#c00">org.bluez.Error.Failed</span>

#### SetDiscoveryFilter

Signature: `void SetDiscoveryFilter(dict filter)`

Sets the device discovery filter for the caller. When this method is called
with no filter parameter, the filter is removed.

Possible filter values:

- **UUIDs** (`array{string}`): Filter by service UUIDs, empty means match any
  UUID. When a remote device is found that advertises any UUID from UUIDs, it
  will be reported if:
  - **Pathloss** and **RSSI** are both empty.
  - Only **Pathloss** is set, the device advertises TX power, and computed
    pathloss is less than the Pathloss parameter.
  - Only **RSSI** is set and received RSSI is higher than the RSSI parameter.
- **RSSI** (`int16`): RSSI threshold value. PropertiesChanged signals will be
  emitted for already existing Device objects with updated RSSI value. If one
  or more discovery filters have been set, the RSSI delta-threshold imposed by
  StartDiscovery by default will not be applied.
- **Pathloss** (`uint16`): Pathloss threshold value. PropertiesChanged signals
  will be emitted for already existing Device objects with updated Pathloss
  value.
- **Transport** (`string`, default `"auto"`): Transport parameter determines
  the type of scan. Possible values:
  - `auto`: Interleaved scan, use LE, BR/EDR, or both depending on what is
    enabled.
  - `bredr`: BR/EDR inquiry only.
  - `le`: LE scan only.
- **DuplicateData** (`bool`, default `false`): Disables duplicate detection of
  advertisement data. When enabled, PropertiesChanged signals will be generated
  for ManufacturerData and ServiceData every time they are discovered.
- **Discoverable** (`bool`, default `false`): Make adapter discoverable while
  discovering. If the adapter is already discoverable, setting this filter has
  no effect.
- **Pattern** (`string`, default `none`): Discover devices where the pattern
  matches either the prefix of the address or device name, which limits the
  number of device objects created during discovery. When set, this disregards
  device discoverable flags. Note: the pattern matching is ignored if there are
  other clients that do not set any pattern, as it works like a logical OR.
  Also, setting an empty string `""` pattern matches any device found.
- **AutoConnect** (`bool`, default `false`): Connect to discovered devices
  automatically if a Pattern has been set and it matches the device address or
  name and it is connectable.

When a discovery filter is set, Device objects will be created as new devices
with matching criteria are discovered regardless of whether they are
connectable or discoverable, enabling listening to non-connectable and
non-discoverable devices.

When multiple clients call SetDiscoveryFilter, their filters are internally
merged, and notifications about new devices are sent to all clients. Each
client must check that device updates actually match its filter.

When SetDiscoveryFilter is called multiple times by the same client, the last
filter passed is active for that client.

SetDiscoveryFilter can be called before StartDiscovery. It is useful when a
client will create the first discovery session, to ensure that the proper scan
starts right after the StartDiscovery call.

Possible errors:

- <span style="color:#c00">org.bluez.Error.NotReady</span>
- <span style="color:#c00">org.bluez.Error.NotSupported</span>
- <span style="color:#c00">org.bluez.Error.Failed</span>

#### GetDiscoveryFilters

Signature: `array{string} GetDiscoveryFilters()`

Returns available filters that can be given to **SetDiscoveryFilter**.

Possible errors: None.

#### ConnectDevice

Signature: `object ConnectDevice(dict properties)` (experimental)

Connects to a device without performing General Discovery.

The connection mechanism is similar to Connect on
[org.bluez.Device](org.bluez.Device.md),
except this method returns success when the physical connection is established
and you can specify bearer to connect with a parameter.

After this method returns, service discovery will continue and any supported
profile will be connected. There is no need to call Connect on Device1 after
this call. If connection was successful, this method returns the object path to
the created [org.bluez.Device](org.bluez.Device.md) object or an existing device
object.

Possible properties values:

- **Address** (`string`, mandatory): The Bluetooth device address of the remote
  device.
- **AddressType** (`string`, default `"BR/EDR"`): The Bluetooth device Address
  Type used for initial connection. Possible values:
  - `public`: Public address.
  - `random`: Random address.

Possible errors:

- <span style="color:#c00">org.bluez.Error.InvalidArguments</span>
- <span style="color:#c00">org.bluez.Error.AlreadyExists</span>
- <span style="color:#c00">org.bluez.Error.NotSupported</span>
- <span style="color:#c00">org.bluez.Error.NotReady</span>
- <span style="color:#c00">org.bluez.Error.Failed</span>

### Properties

| Property | Type | Access | Description |
| --- | --- | --- | --- |
| Address | `string` | `readonly` | The Bluetooth device address. |
| AddressType | `string` | `readonly` | The Bluetooth Address Type. For dual-mode and BR/EDR-only adapters this defaults to `public`. Single-mode LE adapters may have either value. With privacy enabled, this contains the Identity Address type and not the address type used for connection.<br><br>Possible values: `public` (public address), `random` (random address). |
| Name | `string` | `readonly` | The Bluetooth system name (pretty hostname). This is either a static system default or controlled by an external daemon providing pretty hostname configuration. |
| Alias | `string` | `readwrite` | The Bluetooth friendly name. If no alias is set, it returns the system provided name. Setting an empty string resets it back to the system name. Only use this if the local name needs to be different from the pretty hostname. |
| Class | `uint32` | `readonly` | The Bluetooth class of device. This value is automatically configured by DMI/ACPI information or provided as static configuration. |
| Connectable | `boolean` | `readwrite` | Sets the adapter to connectable or non-connectable. This is a global setting and should be used only by the settings application. Setting this to false also sets Discoverable to false (and does not automatically revert when Connectable is set back to true). This affects only incoming connections. |
| Powered | `boolean` | `readwrite` | Switches an adapter on or off. This also sets the appropriate connectable state of the controller. The value is not persistent and resets to false after restart or unplugging the adapter. |
| PowerState | `string` | `readonly`, `experimental` | The power state of an adapter, indicating whether it is turning off/on or is on/off. Possible values: `on`, `off`, `off-enabling`, `on-disabling`, `off-blocked`. |
| Discoverable | `boolean` | `readwrite` (default `false`) | Switches an adapter to discoverable or non-discoverable. This is a global setting and should be used only by the settings application. If DiscoverableTimeout is non-zero, it resets to false after the timer expires. If the adapter is switched off, setting this value fails. PropertiesChanged signals update this when Powered changes. |
| Pairable | `boolean` | `readwrite` (default `true`) | Switches an adapter to pairable or non-pairable. This is a global setting and should be used only by the settings application. Affects only incoming pairing requests. |
| PairableTimeout | `uint32` | `readwrite` (default `0`) | Pairable timeout in seconds. A value of zero disables the timeout and stays in pairable mode indefinitely. |
| DiscoverableTimeout | `uint32` | `readwrite` (default `180`) | Discoverable timeout in seconds. A value of zero disables the timeout and stays in discoverable/limited mode indefinitely. |
| Discovering | `boolean` | `readonly` | Indicates that a device discovery procedure is active. |
| UUIDs | `array{string}` | `readonly` | List of 128-bit UUIDs that represent the available local services. |
| Modalias | `string` | `readonly`, `optional` | Local Device ID information in modalias format used by the kernel and udev. |
| Roles | `array{string}` | `readonly` | List of supported roles. Possible values: `central`, `peripheral`, `central-peripheral`. |
| ExperimentalFeatures | `array{string}` | `readonly`, `optional` | List of 128-bit UUIDs that represent the experimental features currently enabled. |
| Manufacturer | `uint16` | `readonly` | The manufacturer of the device, as a uint16 company identifier defined by the Core Bluetooth Specification. |
| Version | `byte` | `readonly` | The Bluetooth version supported by the device, as a core version code defined by the Core Bluetooth Specification. |
