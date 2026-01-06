# org.bluez.MediaFolder

## BlueZ D-Bus MediaFolder API documentation

| Field | Value |
| --- | --- |
| Version | BlueZ |
| Date | September 2023 |
| Manual section | 5 |
| Manual group | Linux System Administration |

### Interface

- **Service:** unique name (Target role), `org.bluez` (Controller role)
- **Interface:** `org.bluez.MediaFolder1`
- **Object path:** freely definable (Target role), `[variable prefix]/{hci0,hci1,...}/[dev](){BDRADDR}/player#` (Controller role)

### Methods

#### Search

Signature: `object Search(string value, dict filter)`

Returns a folder object containing the search result.

To list the items found, use the returned folder object and pass it to
**ChangeFolder**.

Possible errors:

- <span style="color:#c00">org.bluez.Error.NotSupported</span>
- <span style="color:#c00">org.bluez.Error.Failed</span>

#### ListItems

Signature: `array{objects, properties} ListItems(dict filter)`

Returns a list of items found.

Possible errors:

- <span style="color:#c00">org.bluez.Error.InvalidArguments</span>
- <span style="color:#c00">org.bluez.Error.NotSupported</span>
- <span style="color:#c00">org.bluez.Error.Failed</span>

#### ChangeFolder

Signature: `void ChangeFolder(object folder)`

Change current folder.

By changing folder, items of the previous folder might be destroyed and have to
be listed again. The exception is the NowPlaying folder, which should always be
present while the player is active.

Possible errors:

- <span style="color:#c00">org.bluez.Error.InvalidArguments</span>
- <span style="color:#c00">org.bluez.Error.NotSupported</span>
- <span style="color:#c00">org.bluez.Error.Failed</span>

### Properties

| Property | Type | Access | Description |
| --- | --- | --- | --- |
| NumberOfItems | `uint32` | `readonly` | Number of items in the folder. |
| Name | `string` | `readonly` | Folder name. Possible values: `/Filesystem/...` (filesystem scope), `/NowPlaying/...` (NowPlaying scope). Note: `/NowPlaying` may not be listed if the player is stopped. Folders created by Search are virtual; once another Search is performed or the folder is changed via ChangeFolder, it will no longer be listed. |

### Filters

| Filter | Type | Description |
| --- | --- | --- |
| Start | `uint32` | Offset of the first item. Default: `0`. |
| End | `uint32` | Offset of the last item. Default: `NumberOfItems`. |
| Attributes | `array{string}` | Item properties to include in the list. Possible values: `title`, `artist`, `album`, `genre`, `number-of-tracks`, `number`, `duration`. Default: all. |
