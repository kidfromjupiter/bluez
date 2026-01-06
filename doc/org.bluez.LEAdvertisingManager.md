# org.bluez.LEAdvertisingManager

## BlueZ D-Bus LEAdvertisingManager API documentation

| Field | Value |
| --- | --- |
| Version | BlueZ |
| Date | October 2023 |
| Manual section | 5 |
| Manual group | Linux System Administration |

### Interface

The Advertising Manager allows external applications to register
Advertisement Data that should be broadcast to devices. Advertisement Data
must follow the API for LE Advertisement Data described in
[org.bluez.LEAdvertisement](org.bluez.LEAdvertisement.md).

- **Service:** `org.bluez`
- **Interface:** `org.bluez.LEAdvertisingManager1`
- **Object path:** `/org/bluez/{hci0,hci1,...}`

### Methods

#### RegisterAdvertisement

Signature: `void RegisterAdvertisement(object advertisement, dict options)`

Registers an advertisement object to be sent over the LE Advertising channel.

The service must implement
[org.bluez.LEAdvertisement](org.bluez.LEAdvertisement.md).

Possible errors:

- <span style="color:#c00">org.bluez.Error.InvalidArguments</span> (invalid or conflicting properties)
- <span style="color:#c00">org.bluez.Error.AlreadyExists</span> (already registered)
- <span style="color:#c00">org.bluez.Error.InvalidLength</span> (data packet too long)
- <span style="color:#c00">org.bluez.Error.NotPermitted</span> (maximum number of instances reached)

#### UnregisterAdvertisement

Signature: `void UnregisterAdvertisement(object advertisement)`

Unregisters an advertisement that has been previously registered using
**RegisterAdvertisement**. The object path parameter must match the same value
used on registration.

Possible errors:

- <span style="color:#c00">org.bluez.Error.InvalidArguments</span>
- <span style="color:#c00">org.bluez.Error.DoesNotExist</span>

### Properties

| Property | Type | Access | Description |
| --- | --- | --- | --- |
| ActiveInstances | `byte` | `readonly` | Number of active advertising instances. |
| SupportedInstances | `byte` | `readonly` | Number of available advertising instances. |
| SupportedIncludes | `array{string}` | `readonly` | List of supported system includes. Possible values: `tx-power`, `appearance`, `local-name`, `rsi`. |
| SupportedSecondaryChannels | `array{string}` | `readonly` | Supported secondary channels. Possible values: `1M`, `2M`, `Coded`. |
| SupportedCapabilities | `dict` | `readonly` | Advertising-related controller capabilities. Possible values: `MaxAdvLen` (byte), `MaxScnRspLen` (byte), `MinTxPower` (int16, dBm), `MaxTxPower` (int16, dBm). |
| SupportedFeatures | `array{string}` | `readonly`, `optional` | Supported platform features. If none are available, the array is empty. Possible values: `CanSetTxPower` (platform can specify tx power per instance), `HardwareOffload` (multiple advertising offloaded to controller). |
