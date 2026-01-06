# org.bluez.ProfileManager

## BlueZ D-Bus ProfileManager API documentation

| Field | Value |
| --- | --- |
| Version | BlueZ |
| Date | October 2023 |
| Manual section | 5 |
| Manual group | Linux System Administration |

### Interface

- **Service:** `org.bluez`
- **Interface:** `org.bluez.ProfileManager1`
- **Object path:** `/org/bluez`

### Methods

#### RegisterProfile

Signature: `void RegisterProfile(object profile, string uuid, dict options)`

Registers a profile agent.

The object path defines the path of the profile that will be called when there
is a connection and must implement
[org.bluez.Profile](org.bluez.Profile.md).

If an application disconnects from the bus, all its registered profiles will be
removed.

Possible UUID values:

- `0000111f-0000-1000-8000-00805f9b34fb`: HFP AG. Default profile Version is
  1.7, profile Features is `0b001001`, and RFCOMM channel is 13. Authentication
  is required.
- `0000111e-0000-1000-8000-00805f9b34fb`: HFP HS. Default profile Version is
  1.7, profile Features is `0b000000`, and RFCOMM channel is 7. Authentication
  is required.
- `00001112-0000-1000-8000-00805f9b34fb`: HSP AG. Default profile Version is
  1.2, RFCOMM channel is 12, and authentication is required. Does not support
  any Features; option is ignored.
- `00001108-0000-1000-8000-00805f9b34fb`: HSP HS. Default profile Version is
  1.2, profile Features is `0b0`, and RFCOMM channel is 6. Authentication is
  required. Features is a one-bit value specifying capability of Remote Audio
  Volume Control (default off).
- `<vendor UUID>`: Vendor-defined UUID. No defaults; options must be set.

Possible options values:

- **Name** (`string`): Human readable name for the profile.
- **Service** (`string`): Primary service class UUID (if different from the
  actual profile UUID).
- **Role** (`string`): For asymmetric profiles without UUIDs to uniquely
  identify each side. Possible values: `client`, `server`.
- **Channel** (`uint16`): RFCOMM channel number used for client and server
  UUIDs. Used in the SDP record if applicable.
- **PSM** (`uint16`): PSM number used for client and server UUIDs. Used in the
  SDP record if applicable.
- **RequireAuthentication** (`boolean`): Pairing is required before connections
  are established. No devices will be connected if not paired.
- **RequireAuthorization** (`boolean`): Request authorization before any
  connection is established.
- **AutoConnect** (`boolean`): For a client UUID, force connection of RFCOMM or
  L2CAP channels when a remote device is connected.
- **ServiceRecord** (`string`): Provide a manual SDP record.
- **Version** (`uint16`): Profile version (for SDP record).
- **Features** (`uint16`): Profile features (for SDP record).

Possible errors:

- <span style="color:#c00">org.bluez.Error.InvalidArguments</span>
- <span style="color:#c00">org.bluez.Error.AlreadyExists</span>

#### UnregisterProfile

Signature: `void UnregisterProfile(object profile)`

Unregisters a profile previously registered using **RegisterProfile**. The
object path parameter must match the same value used on registration.

Possible errors:

- <span style="color:#c00">org.bluez.Error.DoesNotExist</span>
