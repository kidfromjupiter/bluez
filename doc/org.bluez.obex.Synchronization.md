# org.bluez.obex.Synchronization

## BlueZ D-Bus OBEX Synchronization API documentation

| Field | Value |
| --- | --- |
| Version | BlueZ |
| Date | October 2023 |
| Manual section | 5 |
| Manual group | Linux System Administration |

### Interface

- **Service:** `org.bluez.obex`
- **Interface:** `org.bluez.obex.Synchronization1`
- **Object path:** `[Session object path]`

### Methods

#### SetLocation

Signature: `void SetLocation(string location)`

Sets the phonebook object store location for other operations. Call this
before all other operations.

Possible `location` values:

- `int` (`internal`, default): Store in internal memory.
- `sim{#}`: Store in SIM card number #.

Possible errors:

- <span style="color:#c00">org.bluez.obex.Error.InvalidArguments</span>

#### GetPhonebook

Signature: `object, dict GetPhonebook(string targetfile)`

Retrieves an entire Phonebook Object store from the remote device and stores it
in a local file.

If an empty target file is given, a name is automatically generated for the
temporary file.

The returned path represents the newly created transfer. Transfer properties
are returned along with the object path to avoid a `GetProperties` call. See
[org.bluez.obex.Transfer](org.bluez.obex.Transfer.md).

Possible errors:

- <span style="color:#c00">org.bluez.obex.Error.InvalidArguments</span>
- <span style="color:#c00">org.bluez.obex.Error.Failed</span>

#### PutPhonebook

Signature: `object, dict PutPhonebook(string sourcefile)`

Sends an entire Phonebook Object store to the remote device.

The returned path represents the newly created transfer. Transfer properties
are returned along with the object path to avoid a `GetProperties` call. See
[org.bluez.obex.Transfer](org.bluez.obex.Transfer.md).

Possible errors:

- <span style="color:#c00">org.bluez.obex.Error.InvalidArguments</span>
- <span style="color:#c00">org.bluez.obex.Error.Failed</span>
