# org.bluez.MediaControl

## BlueZ D-Bus MediaControl API documentation

| Field | Value |
| --- | --- |
| Version | BlueZ |
| Date | September 2023 |
| Manual section | 5 |
| Manual group | Linux System Administration |

### Interface

- **Service:** `org.bluez`
- **Interface:** `org.bluez.MediaControl1`
- **Object path:** `[variable prefix]/{hci0,hci1,...}/[dev](){BDADDR}`

### Methods

All methods in this interface are deprecated. Prefer
[org.bluez.MediaPlayer](org.bluez.MediaPlayer.md) for playback control.

#### Play

Signature: `void Play()` (deprecated)

Resume playback.

#### Pause

Signature: `void Pause()` (deprecated)

Pause playback.

#### Stop

Signature: `void Stop()` (deprecated)

Stop playback.

#### Next

Signature: `void Next()` (deprecated)

Next item.

#### Previous

Signature: `void Previous()` (deprecated)

Previous item.

#### VolumeUp

Signature: `void VolumeUp()` (deprecated)

Adjust remote volume one step up.

#### VolumeDown

Signature: `void VolumeDown()` (deprecated)

Adjust remote volume one step down.

#### FastForward

Signature: `void FastForward()` (deprecated)

Fast forward playback. This action stops only when another method in this
interface is called.

#### Rewind

Signature: `void Rewind()` (deprecated)

Rewind playback. This action stops only when another method in this interface
is called.

### Properties

| Property | Type | Access | Description |
| --- | --- | --- | --- |
| Connected | `boolean` | `readonly` | Whether the media control interface is connected. |
| Player | `object` | `readonly`, `optional` | Addressed [org.bluez.MediaPlayer](org.bluez.MediaPlayer.md) object path. |
