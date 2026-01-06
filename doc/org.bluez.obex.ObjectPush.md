# org.bluez.obex.ObjectPush

## BlueZ D-Bus OBEX ObjectPush API documentation

| Field | Value |
| --- | --- |
| Version | BlueZ |
| Date | October 2023 |
| Manual section | 5 |
| Manual group | Linux System Administration |

### Interface

- **Service:** `org.bluez.obex`
- **Interface:** `org.bluez.obex.ObjectPush1`
- **Object path:** `[Session object path]`

### Methods

#### SendFile

Signature: `object, dict SendFile(string sourcefile)`

Sends a local file to the remote device.

The returned object path represents the newly created transfer, which can be
used to track progress or failure. The transfer properties are returned along
with the object path to avoid a `GetProperties` call. See
[org.bluez.obex.Transfer](org.bluez.obex.Transfer.md) for available properties.

Possible errors:

- <span style="color:#c00">org.bluez.obex.Error.InvalidArguments</span>
- <span style="color:#c00">org.bluez.obex.Error.Failed</span>

#### PullBusinessCard

Signature: `object, dict PullBusinessCard(string targetfile)`

Requests a business card from a remote device and stores it in a local file.

If an empty target file is given, a name is automatically generated for the
temporary file.

The returned object path represents the newly created transfer. The transfer
properties are returned along with the object path to avoid a `GetProperties`
call. See [org.bluez.obex.Transfer](org.bluez.obex.Transfer.md).

Possible errors:

- <span style="color:#c00">org.bluez.obex.Error.InvalidArguments</span>
- <span style="color:#c00">org.bluez.obex.Error.Failed</span>

#### ExchangeBusinessCards

Signature: `object, dict ExchangeBusinessCards(string clientfile, string targetfile)`

Pushes the client's business card to the remote device and retrieves the remote
business card, storing it in a local file.

If an empty target file is given, a name is automatically generated for the
temporary file.

The returned object path represents the newly created transfer. The transfer
properties are returned along with the object path to avoid a `GetProperties`
call. See [org.bluez.obex.Transfer](org.bluez.obex.Transfer.md).

Possible errors:

- <span style="color:#c00">org.bluez.obex.Error.InvalidArguments</span>
- <span style="color:#c00">org.bluez.obex.Error.Failed</span>
