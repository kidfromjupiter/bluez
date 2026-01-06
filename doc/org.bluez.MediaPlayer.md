# org.bluez.MediaPlayer

## BlueZ D-Bus MediaPlayer API documentation

| Field | Value |
| --- | --- |
| Version | BlueZ |
| Date | September 2023 |
| Manual section | 5 |
| Manual group | Linux System Administration |

### Interface

- **Service:** `org.bluez` (Controller role)
- **Interface:** `org.bluez.MediaPlayer1`
- **Object path:** `[variable prefix]/{hci0,hci1,...}/[dev](){BDADDR}/player#`

### Methods

#### Play

Signature: `void Play()`

Resume playback.

Possible errors:

- <span style="color:#c00">org.bluez.Error.NotSupported</span>
- <span style="color:#c00">org.bluez.Error.Failed</span>

#### Pause

Signature: `void Pause()`

Pause playback.

Possible errors:

- <span style="color:#c00">org.bluez.Error.NotSupported</span>
- <span style="color:#c00">org.bluez.Error.Failed</span>

#### Stop

Signature: `void Stop()`

Stop playback.

Possible errors:

- <span style="color:#c00">org.bluez.Error.NotSupported</span>
- <span style="color:#c00">org.bluez.Error.Failed</span>

#### Next

Signature: `void Next()`

Next item.

Possible errors:

- <span style="color:#c00">org.bluez.Error.NotSupported</span>
- <span style="color:#c00">org.bluez.Error.Failed</span>

#### Previous

Signature: `void Previous()`

Previous item.

Possible errors:

- <span style="color:#c00">org.bluez.Error.NotSupported</span>
- <span style="color:#c00">org.bluez.Error.Failed</span>

#### FastForward

Signature: `void FastForward()`

Fast forward playback. This action is only stopped when another method in this
interface is called.

Possible errors:

- <span style="color:#c00">org.bluez.Error.NotSupported</span>
- <span style="color:#c00">org.bluez.Error.Failed</span>

#### Rewind

Signature: `void Rewind()`

Rewind playback. This action is only stopped when another method in this
interface is called.

Possible errors:

- <span style="color:#c00">org.bluez.Error.NotSupported</span>
- <span style="color:#c00">org.bluez.Error.Failed</span>

#### Press

Signature: `void Press(byte avc_key)`

Press a specific key to send as passthrough command. The key will be released
automatically. Use **Hold** if you intend to hold down the key.

Possible errors:

- <span style="color:#c00">org.bluez.Error.InvalidArguments</span>
- <span style="color:#c00">org.bluez.Error.NotSupported</span>
- <span style="color:#c00">org.bluez.Error.Failed</span>

#### Hold

Signature: `void Hold(byte avc_key)`

Press and hold a specific key to send as passthrough command. Call **Release**
afterwards. The held key is also released when any other method in this
interface is called.

Possible errors:

- <span style="color:#c00">org.bluez.Error.InvalidArguments</span>
- <span style="color:#c00">org.bluez.Error.NotSupported</span>
- <span style="color:#c00">org.bluez.Error.Failed</span>

#### Release

Signature: `void Release()`

Release the previously held key invoked using **Hold**.

Possible errors:

- <span style="color:#c00">org.bluez.Error.NotSupported</span>
- <span style="color:#c00">org.bluez.Error.Failed</span>

### Properties

| Property | Type | Access | Description |
| --- | --- | --- | --- |
| Equalizer | `string` | `readwrite` | Player equalizer setting. Possible values: `off`, `on`. |
| Repeat | `string` | `readwrite` | Player repeat setting. Possible values: `off`, `singletrack`, `alltracks`, `group`. |
| Shuffle | `string` | `readwrite` | Player shuffle setting. Possible values: `off`, `alltracks`, `group`. |
| Scan | `string` | `readwrite` | Player scan setting. Possible values: `off`, `alltracks`, `group`. |
| Status | `string` | `readonly` | Player status. Possible values: `playing`, `stopped`, `paused`, `forward-seek`, `reverse-seek`, `error`. |
| Position | `uint32` | `readonly` | Playback position in milliseconds. Changing the position may generate additional events sent to the remote device. Position 0 indicates the track is starting; a value greater than or equal to duration indicates the track has ended. If duration is unavailable, set position to the maximum uint32 to signal end. |
| Track | `dict` | `readonly` | Track metadata. Possible values: `Title`, `Artist`, `Album`, `Genre`, `NumberOfTracks`, `TrackNumber`, `Duration`, `ImgHandle` (experimental). `ImgHandle` is available only during the lifetime of an OBEX BIP connection to the ObexPort. |
| Device | `object` | `readonly` | [org.bluez.Device](org.bluez.Device.md) object path. |
| Name | `string` | `readonly` | Player name. |
| Type | `string` | `readonly` | Player type. Possible values: `Audio`, `Video`, `Audio Broadcasting`, `Video Broadcasting`. |
| Subtype | `string` | `readonly` | Player subtype. Possible values: `Audio Book`, `Podcast`. |
| Browsable | `boolean` | `readonly` | If present, indicates the player can be browsed using [org.bluez.MediaFolder](org.bluez.MediaFolder.md). Possible values: `True` (supported and active), `False` (supported but inactive). Note: if supported but inactive, clients can enable it via MediaFolder, but it might interfere with playback of other players. |
| Searchable | `boolean` | `readonly` | If present, indicates the player can be searched using [org.bluez.MediaFolder](org.bluez.MediaFolder.md). Possible values: `True` (supported and active), `False` (supported but inactive). Note: if supported but inactive, clients can enable it via MediaFolder, but it might interfere with playback of other players. |
| Playlist | `object` | `readonly` | Playlist object path. |
| ObexPort | `uint16` | `readonly`, `experimental` | If present, indicates the player can get cover art using BIP over OBEX on this PSM port. |
