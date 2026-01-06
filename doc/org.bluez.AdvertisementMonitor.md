# org.bluez.AdvertisementMonitor

## BlueZ D-Bus AdvertisementMonitor API documentation

| Field | Value |
| --- | --- |
| Version | BlueZ |
| Date | October 2023 |
| Manual section | 5 |
| Manual group | Linux System Administration |

### Description

This API allows a client to specify a job of monitoring advertisements by
registering the root of a hierarchy and then exposing advertisement monitors
under the root with filtering conditions, RSSI thresholds, and timers.

Once a monitoring job is activated by **bluetoothd(8)**, the client can expect
to be notified about the targeted advertisements regardless of an ongoing
discovery session (see **StartDiscovery** in
[org.bluez.Adapter](org.bluez.Adapter.md)).

### Interface

- **Service:** `org.bluez`
- **Interface:** `org.bluez.AdvertisementMonitor1` (experimental)
- **Object path:** freely definable

### Methods

#### Release

Signature: `void Release()` (noreply)

Called as a signal for a client to perform cleanup when:

- The monitor cannot be activated after it was exposed.
- The monitor has been deactivated.

#### Activate

Signature: `void Activate()` (noreply)

Called after a monitor is exposed to acknowledge activation. After this, the
client can expect calls to **DeviceFound** or **DeviceLost**.

#### DeviceFound

Signature: `void DeviceFound(object device)` (noreply)

Notifies the client that a targeted device was found.

After receiving this call, the client should start monitoring the corresponding
[org.bluez.Device](org.bluez.Device.md) to retrieve changes in RSSI and
advertisement content.

#### DeviceLost

Signature: `void DeviceLost(object device)` (noreply)

Notifies the client that a targeted device was lost.

After receiving this call, the client should stop monitoring the corresponding
[org.bluez.Device](org.bluez.Device.md).

### Properties

| Property | Type | Access | Description |
| --- | --- | --- | --- |
| Type | `string` | `read-only` | The type of the monitor. See **SupportedMonitorTypes** in [org.bluez.AdvertisementMonitorManager](org.bluez.AdvertisementMonitorManager.md). |
| RSSILowThreshold | `int16` | `read-only`, `optional` | Used with **RSSILowTimeout** to determine whether a device becomes out-of-range. Valid range is -127 to 20 (dBm); 127 indicates unset. |
| RSSIHighThreshold | `int16` | `read-only`, `optional` | Used with **RSSIHighTimeout** to determine whether a device becomes in-range. Valid range is -127 to 20 (dBm); 127 indicates unset. |
| RSSILowTimeout | `uint16` | `read-only`, `optional` | Time to consider a device out-of-range. If this many seconds elapses without receiving any signal at least as strong as **RSSILowThreshold**, a currently in-range device is considered lost. Valid range is 1 to 300 seconds; 0 indicates unset. |
| RSSIHighTimeout | `uint16` | `read-only`, `optional` | Time to consider a device in-range. If this many seconds elapses while receiving signals at least as strong as **RSSIHighThreshold**, a currently out-of-range device is considered found. Valid range is 1 to 300 seconds; 0 indicates unset. |
| RSSISamplingPeriod | `uint16` | `read-only`, `optional` | Grouping rules for propagating received advertisement packets to the client.<br><br>Possible values:<br>- `0`: All advertisement packets from in-range devices are propagated.<br>- `255`: Only the first advertisement packet of in-range devices is propagated. If the device becomes lost, then the first packet when found again will also be propagated.<br>- `1` to `254`: Advertisement packets are grouped into 100ms * N time periods. Packets in the same group are reported once, with RSSI averaged.<br><br>Currently unimplemented in user space; forwarded to the kernel only. |
| Patterns | `array{(uint8, uint8, array{byte})}` | `read-only`, `optional` | If **Type** is `"or_patterns"`, this property must exist and contain at least one entry.<br><br>Pattern structure:<br>- `uint8 start_position`: Index in AD data field where the search should start (index 0 is the beginning).<br>- `uint8 AD_data_type`: See the [Bluetooth Assigned Numbers](https://www.bluetooth.com/specifications/assigned-numbers/) (generic-access-profile) for allowed values.<br>- `array{byte} content_of_pattern`: Pattern value, max length 31 bytes. |
