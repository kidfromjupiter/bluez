# org.bluez.MediaItem

## BlueZ D-Bus MediaItem API documentation

| Field | Value |
| --- | --- |
| Version | BlueZ |
| Date | September 2023 |
| Manual section | 5 |
| Manual group | Linux System Administration |

### Interface

- **Service:** unique name (Target role), `org.bluez` (Controller role)
- **Interface:** `org.bluez.MediaItem1`
- **Object path:** freely definable (Target role), `[variable prefix]/{hci0,hci1,...}/[dev](){BDRADDR}/player#/item#` (Controller role)

### Methods

#### Play

Signature: `void Play()`

Play item.

Possible errors:

- <span style="color:#c00">org.bluez.Error.NotSupported</span>
- <span style="color:#c00">org.bluez.Error.Failed</span>

#### AddtoNowPlaying

Signature: `void AddtoNowPlaying()`

Add item to the now playing list.

Possible errors:

- <span style="color:#c00">org.bluez.Error.NotSupported</span>
- <span style="color:#c00">org.bluez.Error.Failed</span>

### Properties

| Property | Type | Access | Description |
| --- | --- | --- | --- |
| Player | `object` | `readonly` | [org.bluez.MediaPlayer](org.bluez.MediaPlayer.md) object path the item belongs to. |
| Name | `string` | `readonly` | Item displayable name. |
| Type | `string` | `readonly` | Item type. Possible values: `video`, `audio`, `folder`. |
| FolderType | `string` | `readonly`, `optional` | Folder type. Available if **Type** is `folder`. Possible values: `mixed`, `titles`, `albums`, `artists`. |
| Playable | `boolean` | `readonly`, `optional` | Indicates if the item can be played. Available if **Type** is `folder`. |
| Metadata | `dict` | `readonly` | Item metadata. Possible values (available if **Type** is `audio` or `video`): `Title`, `Artist`, `Album`, `Genre`, `NumberOfTracks`, `Number`, `Duration`. |
