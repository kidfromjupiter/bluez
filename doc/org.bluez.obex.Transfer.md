# org.bluez.obex.Transfer

## BlueZ D-Bus OBEX Transfer API documentation

| Field | Value |
| --- | --- |
| Version | BlueZ |
| Date | October 2023 |
| Manual section | 5 |
| Manual group | Linux System Administration |

### Interface

- **Service:** `org.bluez.obex`
- **Interface:** `org.bluez.obex.Transfer1`
- **Object path:** `[Session object path]/transfer{#}`

### Methods

#### Cancel

Signature: `void Cancel()`

Cancels the current transfer.

Possible errors:

- <span style="color:#c00">org.bluez.obex.Error.NotAuthorized</span>
- <span style="color:#c00">org.bluez.obex.Error.InProgress</span>
- <span style="color:#c00">org.bluez.obex.Error.Failed</span>

#### Suspend

Signature: `void Suspend()`

Suspends transfer.

Possible errors:

- <span style="color:#c00">org.bluez.obex.Error.NotAuthorized</span>
- <span style="color:#c00">org.bluez.obex.Error.NotInProgress</span> (if **Status** is `queued`)

#### Resume

Signature: `void Resume()`

Resumes transfer previously suspended with **Suspend**.

Possible errors:

- <span style="color:#c00">org.bluez.obex.Error.NotAuthorized</span>
- <span style="color:#c00">org.bluez.obex.Error.NotInProgress</span> (if **Status** is `queued`)

### Properties

| Property | Type | Access | Description |
| --- | --- | --- | --- |
| Status | `string` | `readonly` | Current transfer status. Possible values: `queued`, `active`, `suspended`, `complete`, `error`. |
| Session | `object` | `readonly` | Object path of the [org.bluez.obex.Session](org.bluez.obex.Session.md) this transfer belongs to. |
| Name | `string` | `readonly`, `optional` | Name of the object being transferred. Either **Name** or **Type** (or both) will be present. |
| Type | `string` | `readonly`, `optional` | Type of the object being transferred. Either **Name** or **Type** (or both) will be present. |
| Time | `uint64` | `readonly`, `optional` | Time of the object being transferred if provided by the remote party. |
| Size | `uint64` | `readonly`, `optional` | Size of the object being transferred. If unknown, this property is not present. |
| Transferred | `uint64` | `readonly`, `optional` | Number of bytes transferred. For transfers with **Status** `queued`, this value is not present. |
| Filename | `string` | `readonly`, `optional` | Complete name of the file being received or sent. For incoming object push, this is the proposed default location/name. It can be overridden by **AuthorizePush** in [org.bluez.obex.Agent](org.bluez.obex.Agent.md), and then updated accordingly. |
