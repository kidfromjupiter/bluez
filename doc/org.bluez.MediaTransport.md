# org.bluez.MediaTransport

## BlueZ D-Bus MediaTransport API documentation

| Field | Value |
| --- | --- |
| Version | BlueZ |
| Date | July 2024 |
| Manual section | 5 |
| Manual group | Linux System Administration |

### Interface

- **Service:** `org.bluez`
- **Interface:** `org.bluez.MediaTransport1`
- **Object path:** `[variable prefix]/{hci0,hci1,...}/[dev](){BDADDR}/fd#`

### Methods

#### Acquire

Signature: `fd, uint16, uint16 Acquire()`

Acquire transport file descriptor and the MTU for read and write respectively.

Possible errors:

- <span style="color:#c00">org.bluez.Error.NotAuthorized</span>
- <span style="color:#c00">org.bluez.Error.Failed</span>

#### TryAcquire

Signature: `fd, uint16, uint16 TryAcquire()`

Acquire transport file descriptor only if the transport is in `pending` state
when the message is received by BlueZ. Otherwise no request is sent to the
remote device and the function fails with **org.bluez.Error.NotAvailable**.

Possible errors:

- <span style="color:#c00">org.bluez.Error.NotAuthorized</span>
- <span style="color:#c00">org.bluez.Error.Failed</span>
- <span style="color:#c00">org.bluez.Error.NotAvailable</span>

#### Release

Signature: `void Release()`

Releases the file descriptor.

#### Select

Signature: `void Select()`

Applicable only for transports created by a broadcast sink. This moves the
transport from `idle` to `broadcasting`.

This allows callers to select which BISes they wish to sync to via a two-step
process:

1. The user calls the method, changing the transport state to `broadcasting`.
2. The audio server detects the state and automatically acquires it.

Possible errors:

- <span style="color:#c00">org.bluez.Error.NotAuthorized</span>

#### Unselect

Signature: `void Unselect()`

Applicable only for transports created by a broadcast sink. This moves the
transport from `broadcasting` or `active` to `idle`.

This allows callers to terminate the sync to a BIS via a two-step process:

1. The user calls this method, changing the transport state to `idle`.
2. The audio server detects the event and releases the transport.

Possible errors:

- <span style="color:#c00">org.bluez.Error.NotAuthorized</span>

### Properties

| Property | Type | Access | Description |
| --- | --- | --- | --- |
| Device | `object` | `readonly` | [org.bluez.Device](org.bluez.Device.md) object path the transport is connected to. |
| UUID | `string` | `readonly` | UUID of the profile the transport is for. |
| Codec | `byte` | `readonly` | Assigned codec number. Values should match the profile specification indicated by the UUID. |
| Configuration | `array{byte}` | `readonly` | Configuration blob; size and byte order must match. |
| State | `string` | `readonly` | Transport state. Possible values: `idle` (not streaming), `pending` (streaming but not acquired), `broadcasting` (streaming but not acquired; broadcast sink only), `active` (streaming and acquired). |
| Delay | `uint16` | `readwrite`, `optional` | Transport delay in 1/10 ms. Writable only when the transport corresponds to a sink endpoint and is acquired by the sender. |
| Volume | `uint16` | `readwrite`, `optional` | Volume level for the transport. Writable only when the transport is acquired by the sender. Possible values: `0-127` (A2DP), `0-255` (BAP). |
| Endpoint | `object` | `readonly`, `optional`, `experimental` | [org.bluez.MediaEndpoint](org.bluez.MediaEndpoint.md) object path associated with the transport. |
| Location | `uint32` | `readonly`, ISO only, experimental | Transport Audio Location. |
| Metadata | `array{byte}` | `readwrite`, ISO only, experimental | Transport metadata. |
| Links | `array{object}` | `readonly`, `optional`, CIS only, experimental | Linked transport objects associated with the transport. |
| Links | `array{object}` | `readwrite`, BIS only, experimental | For a Broadcast Sink, the BIG sync procedure requires all desired streams to be enumerated from the start. To avoid terminating and recreating BIG sync every time a new transport is selected for acquire, all transports selected via **Select** need to be linked together. When the first transport is acquired via Acquire, all links are included in the BIG sync command. An acquired transport creates and sets fds for all its links; each link must then be acquired separately to get the fd and start receiving audio. |
| QoS | `dict` | `readwrite`, `optional`, ISO only, experimental | QoS configuration. See details below. |

#### QoS (MediaTransport)

Possible values for Unicast:

- **CIG** (`byte`): `0x00` - `0xef` valid ID range, `0xff` auto-allocate.
- **CIS** (`byte`): `0x00` - `0xef` valid ID range, `0xff` auto-allocate.
- **Framing** (`byte`): `0x00` unframed, `0x01` framed.
- **PresentationDelay** (`uint32`): Transport presentation delay (us).
- **TargetLatency** (`byte`): `0x01` low latency, `0x02` balanced latency/reliability, `0x03` high reliability.

Possible values for Broadcast:

- **BIG** (`byte`): Configured QoS BIG.
- **BIS** (`byte`): Configured BIS.
- **SyncFactor** (`byte`): Broadcast sync factor.
- **Packing** (`byte`): Configured packing.
- **Framing** (`byte`): Configured framing.
- **BCode** (`array{byte}`): Encryption/decryption string.
- **encryption** (`byte`): Whether the stream is encrypted.
- **Options** (`byte`): Broadcast options.
- **Skip** (`uint16`): Broadcast skip.
- **SyncTimeout** (`byte`): Broadcast sync timeout.
- **SyncType** (`byte`): Broadcast sync CTE type.
- **MSE** (`byte`): Broadcast MSE.
- **Timeout** (`uint16`): Broadcast timeout.

Possible values for both Unicast and Broadcast:

- **Interval** (`uint32`): ISO interval (us).
- **Latency** (`uint16`): Transport latency (ms).
- **SDU** (`uint16`): Maximum SDU.
- **PHY** (`byte`): bit 0 `LE 1M`, bit 1 `LE 2M`, bit 2 `LE Coded`.
- **Retransmissions** (`byte`): Configured retransmissions.
