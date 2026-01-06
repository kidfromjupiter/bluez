# org.bluez.obex.FileTransfer

## BlueZ D-Bus OBEX FileTransfer API documentation

| Field | Value |
| --- | --- |
| Version | BlueZ |
| Date | October 2023 |
| Manual section | 5 |
| Manual group | Linux System Administration |

### Interface

- **Service:** `org.bluez.obex`
- **Interface:** `org.bluez.obex.FileTransfer1`
- **Object path:** `[Session object path]`

### Methods

#### ChangeFolder

Signature: `void ChangeFolder(string folder)`

Changes the current folder of the remote device.

Possible errors:

- <span style="color:#c00">org.bluez.obex.Error.InvalidArguments</span>
- <span style="color:#c00">org.bluez.obex.Error.Failed</span>

#### CreateFolder

Signature: `void CreateFolder(string folder)`

Creates a new folder on the remote device.

Possible errors:

- <span style="color:#c00">org.bluez.obex.Error.InvalidArguments</span>
- <span style="color:#c00">org.bluez.obex.Error.Failed</span>

#### ListFolder

Signature: `array{dict} ListFolder()`

Returns a dictionary containing information about the current folder content.

Possible return values:

| Field | Type | Description |
| --- | --- | --- |
| Name | `string` | Object name in UTF-8 format. |
| Type | `string` | Either `folder` or `file`. |
| Size | `uint64` | Object size or number of items in folder. |
| Permission | `string` | Group, owner, and other permission. |
| Modified | `uint64` | Last change. |
| Accessed | `uint64` | Last access. |
| Created | `uint64` | Creation date. |

Possible errors:

- <span style="color:#c00">org.bluez.obex.Error.Failed</span>

#### GetFile

Signature: `object, dict GetFile(string targetfile, string sourcefile)`

Copies the contents of the source file (remote device) to the target file (local
filesystem).

If an empty target file is given, a name will be automatically generated for the
temporary file.

The returned path represents the newly created transfer, which can be used to
track success or failure. Transfer properties are returned along with the
object path to avoid a `GetProperties` call. See
[org.bluez.obex.Transfer](org.bluez.obex.Transfer.md).

Possible errors:

- <span style="color:#c00">org.bluez.obex.Error.InvalidArguments</span>
- <span style="color:#c00">org.bluez.obex.Error.Failed</span>

#### PutFile

Signature: `object, dict PutFile(string sourcefile, string targetfile)`

Copies the contents of the source file (local filesystem) to the target file
(on the remote device).

The returned path represents the newly created transfer. Transfer properties
are returned along with the object path to avoid a `GetProperties` call. See
[org.bluez.obex.Transfer](org.bluez.obex.Transfer.md).

Possible errors:

- <span style="color:#c00">org.bluez.obex.Error.InvalidArguments</span>
- <span style="color:#c00">org.bluez.obex.Error.Failed</span>

#### CopyFile

Signature: `void CopyFile(string sourcefile, string targetfile)`

Copies the contents from source file to target file on the remote device.

Possible errors:

- <span style="color:#c00">org.bluez.obex.Error.InvalidArguments</span>
- <span style="color:#c00">org.bluez.obex.Error.Failed</span>

#### MoveFile

Signature: `void MoveFile(string sourcefile, string targetfile)`

Moves a file within the remote device from source file to target file.

Possible errors:

- <span style="color:#c00">org.bluez.obex.Error.InvalidArguments</span>
- <span style="color:#c00">org.bluez.obex.Error.Failed</span>

#### Delete

Signature: `void Delete(string file)`

Deletes the specified file or folder.

Possible errors:

- <span style="color:#c00">org.bluez.obex.Error.InvalidArguments</span>
- <span style="color:#c00">org.bluez.obex.Error.Failed</span>
