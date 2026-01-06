# org.bluez.LEAdvertisement

## BlueZ D-Bus LEAdvertisement API documentation

| Field | Value |
| --- | --- |
| Version | BlueZ |
| Date | October 2023 |
| Manual section | 5 |
| Manual group | Linux System Administration |

### Description

Advertising packets are structured data broadcast on the LE Advertising
channels and available for all devices in range. Because of limited space in
LE Advertising packets, each packet's contents must be carefully controlled.

**bluetoothd(8)** acts as a store for Advertisement Data to be sent. It
constructs the correct Advertisement Data from structured data and configures
the kernel to send the correct advertisement.

### Interface

Specifies Advertisement Data to be broadcast and advertising parameters.
Properties not present will not be included in the data. Required advertisement
data types are always included. All UUIDs are 128-bit versions in the API; 16
or 32-bit versions of the same UUID will be used in the advertising data as
appropriate.

- **Service:** `org.bluez`
- **Interface:** `org.bluez.LEAdvertisement1`
- **Object path:** freely definable

This interface is registered through
[org.bluez.LEAdvertisingManager](org.bluez.LEAdvertisingManager.md).

### Methods

#### Release

Signature: `void Release()` (noreply)

Called when **bluetoothd(8)** removes the advertisement.

A client can use it to do cleanup. There is no need to call
**UnregisterAdvertisement** because when this method gets called it has already
been unregistered.

### Properties

| Property | Type | Access | Description |
| --- | --- | --- | --- |
| Type | `string` | `readonly` | Determines the type of advertising packet requested. Possible values: `broadcast`, `peripheral`. |
| ServiceUUIDs | `array{string}` | `readonly`, `optional` | List of UUIDs to include in the "Service UUID" field of the Advertising Data. |
| ManufacturerData | `dict` | `readonly`, `optional` | Manufacturer Data fields to include in the Advertising Data. Keys are the Manufacturer ID to associate with the data. |
| SolicitUUIDs | `array{string}` | `readonly`, `optional` | List of UUIDs to include in the "Service Solicitation" field of the Advertising Data. |
| ServiceData | `dict` | `readonly`, `optional` | Service Data elements to include in the Advertising Data. Keys are the UUID to associate with the data. |
| Data | `dict` | `readonly`, `optional` | Advertising Data to include. Key is the advertising type and value is the data as byte array. Note: Types already handled by other properties should not be used.<br><br>Possible values: `&lt;type&gt;` → `&lt;byte array&gt;`<br>Example: `&lt;Transport Discovery&gt; &lt;Organization Flags...&gt; 0x26 0x01 0x01...` |
| ScanResponseServiceUUIDs | `array{string}` | `readonly`, `optional`, `experimental` | List of UUIDs to include in the "Service UUID" field of the Scan Response Data. |
| ScanResponseManufacturerData | `dict` | `readonly`, `optional`, `experimental` | Manufacturer Data fields to include in the Scan Response Data. Keys are the Manufacturer ID to associate with the data. |
| ScanResponseSolicitUUIDs | `array{string}` | `readonly`, `optional`, `experimental` | List of UUIDs to include in the "Service Solicitation" field of the Scan Response Data. |
| ScanResponseServiceData | `dict` | `readonly`, `optional`, `experimental` | Service Data elements to include in the Scan Response Data. Keys are the UUID to associate with the data. |
| ScanResponseData | `dict` | `readonly`, `optional`, `experimental` | Scan Response Data to include. Key is the advertising type and value is the data as byte array. |
| Discoverable | `bool` | `readonly`, `optional` | Advertise as general discoverable. When present this overrides the adapter Discoverable property. Note: This property must not be set when **Type** is `broadcast`. |
| DiscoverableTimeout | `uint16` | `readonly`, `optional` | Discoverable timeout in seconds. A value of zero disables the timeout and stays in discoverable/limited mode forever. Note: This property must not be set when **Type** is `broadcast`. |
| Includes | `array{string}` | `readonly`, `optional` | List of features to be included in the advertising packet. Supported values are listed in [org.bluez.LEAdvertisingManager](org.bluez.LEAdvertisingManager.md) **SupportedIncludes**. |
| LocalName | `string` | `readonly`, `optional` | Local name to be used in the advertising report. If too long to fit into the packet it will be truncated. If this property is available, `local-name` cannot be present in **Includes**. |
| Appearance | `uint16` | `readonly`, `optional` | Appearance to be used in the advertising report. Possible values are as found on the GAP Service. |
| Duration | `uint16` | `readonly`, `optional` | Rotation duration of the advertisement in seconds. If there are other applications advertising, the default is 2 seconds. |
| Timeout | `uint16` | `readonly`, `optional` | Timeout of the advertisement in seconds. This defines the lifetime of the advertisement. |
| SecondaryChannel | `string` | `readonly`, `optional` | Secondary channel to be used. Primary channel is always `1M` except when `Coded` is set. Possible values: `1M` (default), `2M`, `Coded`. |
| MinInterval | `uint32` | `readonly`, `optional` | Minimum advertising interval for the advertising set, in milliseconds. Acceptable range: 20ms to 10,485s. If MinInterval is larger than MaxInterval, registration fails. |
| MaxInterval | `uint32` | `readonly`, `optional` | Maximum advertising interval for the advertising set, in milliseconds. Acceptable range: 20ms to 10,485s. If MinInterval is larger than MaxInterval, registration fails. |
| TxPower | `int16` | `readonly`, `optional` | Requested transmission power of this advertising set. Used only if `CanSetTxPower` is enabled on [org.bluez.LEAdvertisingManager](org.bluez.LEAdvertisingManager.md). Values must be in range -127 to +20 dBm. |
