# org.bluez.obex.Client

## BlueZ D-Bus OBEX Client API documentation

| Field | Value |
| --- | --- |
| Version | BlueZ |
| Date | October 2023 |
| Manual section | 5 |
| Manual group | Linux System Administration |

### Interface

- **Service:** `org.bluez.obex`
- **Interface:** `org.bluez.obex.Client1`
- **Object path:** `/org/bluez/obex`

### Methods

#### CreateSession

Signature: `object CreateSession(string destination, dict args)`

Connects to the destination address and creates an OBEX session object which
implements [org.bluez.obex.Session](org.bluez.obex.Session.md).

`args` is a dictionary for optional or type-specific parameters.

Possible `args` values:

- **Target** (`string`): Type of session to create. Possible values: `ftp`,
  `map`, `opp`, `pbap`, `sync`, `bip-avrcp`.
- **Source** (`string`): Local address to use.
- **Channel** (`byte`): Channel to use.
- **PSM** (`uint16`): L2CAP PSM to use.

Possible errors:

- <span style="color:#c00">org.bluez.obex.Error.InvalidArguments</span>
- <span style="color:#c00">org.bluez.obex.Error.Failed</span>

#### RemoveSession

Signature: `void RemoveSession(object session)`

Disconnects and removes a session previously created by **CreateSession**,
aborting any pending transfers.

Possible errors:

- <span style="color:#c00">org.bluez.obex.Error.InvalidArguments</span>
- <span style="color:#c00">org.bluez.obex.Error.NotAuthorized</span>
