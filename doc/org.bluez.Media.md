# org.bluez.Media

## BlueZ D-Bus Media API documentation

| Field | Value |
| --- | --- |
| Version | BlueZ |
| Date | April 2025 |
| Manual section | 5 |
| Manual group | Linux System Administration |

### Interface

- **Service:** `org.bluez`
- **Interface:** `org.bluez.Media1`
- **Object path:** `[variable prefix]/{hci0,hci1,...}`

### Methods

#### RegisterEndpoint

Signature: `void RegisterEndpoint(object endpoint, dict properties)`

Register a local endpoint with the sender. The sender can register as many
endpoints as it likes.

Note: If the sender disconnects, the endpoints are automatically unregistered.

Possible properties:

- **UUID** (`string`): UUID of the profile which the endpoint is for. UUID must
  be in the list of **SupportedUUIDs**.
- **Codec** (`byte`): Assigned number of codec that the endpoint implements.
  Values should match the profile specification indicated by the UUID.
- **Vendor** (`uint32_t`, optional): Vendor-specific Company ID, Codec ID tuple
  implemented by the endpoint. Set this when Vendor Specific Codec (`0xff`) is
  used.
- **Capabilities** (`array{byte}`): Capabilities blob; size and byte order must
  match.
- **Metadata** (`array{byte}`, optional): Metadata blob; size and byte order
  must match.

Possible errors:

- <span style="color:#c00">org.bluez.Error.InvalidArguments</span>
- <span style="color:#c00">org.bluez.Error.NotSupported</span> (interface for the endpoint is disabled)

#### UnregisterEndpoint

Signature: `void UnregisterEndpoint(object endpoint)`

Unregister sender endpoint.

#### RegisterPlayer

Signature: `void RegisterPlayer(object player, dict properties)`

Register a media player object with the sender. The sender can register as many
objects as it likes.

Object must implement at least **org.mpris.MediaPlayer2.Player** as defined in
MPRIS 2.2:

- <http://specifications.freedesktop.org/mpris-spec/latest/>

Note: If the sender disconnects, its objects are automatically unregistered.

Possible errors:

- <span style="color:#c00">org.bluez.Error.InvalidArguments</span>
- <span style="color:#c00">org.bluez.Error.NotSupported</span>

#### UnregisterPlayer

Signature: `void UnregisterPlayer(object player)`

Unregister sender media player.

#### RegisterApplication

Signature: `void RegisterApplication(object root, dict options)`

Register endpoint and player objects within the root object, which must
implement **org.freedesktop.DBus.ObjectManager**.

The application object path together with the D-Bus system bus connection ID
define the identification of the application.

Possible errors:

- <span style="color:#c00">org.bluez.Error.InvalidArguments</span>
- <span style="color:#c00">org.bluez.Error.AlreadyExists</span>

#### UnregisterApplication

Signature: `void UnregisterApplication(object application)`

Unregisters services that were previously registered.

The object path parameter must match the same value used on registration.

Possible errors:

- <span style="color:#c00">org.bluez.Error.InvalidArguments</span>
- <span style="color:#c00">org.bluez.Error.DoesNotExist</span>

### Properties

| Property | Type | Access | Description |
| --- | --- | --- | --- |
| SupportedUUIDs | `array{string}` | `readonly` | List of 128-bit UUIDs representing supported Endpoint registration. |
| SupportedFeatures | `array{string}` | `readonly` | List of supported special features. Possible values: `tx-timestamping` (Bluetooth TX timestamping in media stream sockets is supported by BlueZ and kernel; applications may check kernel support for specific timestamp types via `SIOCETHTOOL`). |
