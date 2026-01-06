# org.bluez.NetworkServer

## BlueZ D-Bus NetworkServer API documentation

| Field | Value |
| --- | --- |
| Version | BlueZ |
| Date | October 2023 |
| Manual section | 5 |
| Manual group | Linux System Administration |

### Interface

- **Service:** `org.bluez`
- **Interface:** `org.bluez.NetworkServer1`
- **Object path:** `/org/bluez/{hci0,hci1,...}`

### Methods

#### Register

Signature: `void Register(string uuid, string bridge)`

Registers server for the provided UUID.

Every new connection to this server will be added to the bridge interface.

Possible UUID values:

- `panu`, `00001115-0000-1000-8000-00805f9b34fb`: Personal Network User role.
- `nap`, `00001116-0000-1000-8000-00805f9b34fb`: Network Access Point role.
- `gn`, `00001117-0000-1000-8000-00805f9b34fb`: Group Network role.

Initially no network server SDP is provided. Only after this method is called
will an SDP record be available and the BNEP server will be ready for incoming
connections.

Possible errors:

- <span style="color:#c00">org.bluez.Error.InvalidArguments</span>
- <span style="color:#c00">org.bluez.Error.AlreadyExists</span>
- <span style="color:#c00">org.bluez.Error.Failed</span>

#### Unregister

Signature: `void Unregister(string uuid)`

Unregisters the server for the provided UUID that was previously registered
with **Register**.

All servers are automatically unregistered when the calling application
terminates.

Possible errors:

- <span style="color:#c00">org.bluez.Error.InvalidArguments</span>
- <span style="color:#c00">org.bluez.Error.Failed</span>
