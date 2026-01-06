# org.bluez.MediaEndpoint

## BlueZ D-Bus MediaEndpoint API documentation

| Field | Value |
| --- | --- |
| Version | BlueZ |
| Date | September 2023 |
| Manual section | 5 |
| Manual group | Linux System Administration |

### Interface

- **Service:** unique name (Server role), `org.bluez` (Client role)
- **Interface:** `org.bluez.MediaEndpoint1`
- **Object path:** freely definable (Server role), `[variable prefix]/{hci0,hci1,...}/[dev](){BDADDR}/sep#` (Client role)

### Methods

#### SetConfiguration

Signature: `void SetConfiguration(object transport, dict properties)`

Set configuration for the transport.

- **transport**: Configured transport object.
- **properties**: Configured [org.bluez.MediaTransport](org.bluez.MediaTransport.md) properties.

For client role, transport must be set with a server endpoint object which will
be configured and the properties must contain the following:

- **Capabilities** (`array{byte}`, mandatory): See **Capabilities** property.
- **Metadata** (`array{byte}`, ISO only): See **Metadata** property.
- **QoS** (`dict`, ISO only): See **QoS** property.

Possible errors for A2DP endpoints:

- <span style="color:#c00">org.bluez.Error.A2DP.InvalidCodecType</span>
- <span style="color:#c00">org.bluez.Error.A2DP.NotSupportedCodecType</span>
- <span style="color:#c00">org.bluez.Error.A2DP.InvalidSamplingFrequency</span>
- <span style="color:#c00">org.bluez.Error.A2DP.NotSupportedSamplingFrequency</span>
- <span style="color:#c00">org.bluez.Error.A2DP.InvalidChannelMode</span>
- <span style="color:#c00">org.bluez.Error.A2DP.NotSupportedChannelMode</span>
- <span style="color:#c00">org.bluez.Error.A2DP.InvalidSubbands</span>
- <span style="color:#c00">org.bluez.Error.A2DP.NotSupportedSubbands</span>
- <span style="color:#c00">org.bluez.Error.A2DP.InvalidAllocationMethod</span>
- <span style="color:#c00">org.bluez.Error.A2DP.NotSupportedAllocationMethod</span>
- <span style="color:#c00">org.bluez.Error.A2DP.InvalidMinimumBitpoolValue</span>
- <span style="color:#c00">org.bluez.Error.A2DP.NotSupportedMinimumBitpoolValue</span>
- <span style="color:#c00">org.bluez.Error.A2DP.InvalidMaximumBitpoolValue</span>
- <span style="color:#c00">org.bluez.Error.A2DP.NotSupportedMaximumBitpoolValue</span>
- <span style="color:#c00">org.bluez.Error.A2DP.InvalidLayer</span>
- <span style="color:#c00">org.bluez.Error.A2DP.NotSupportedLayer</span>
- <span style="color:#c00">org.bluez.Error.A2DP.NotSupportedCRC</span>
- <span style="color:#c00">org.bluez.Error.A2DP.NotSupportedMPF</span>
- <span style="color:#c00">org.bluez.Error.A2DP.NotSupportedVBR</span>
- <span style="color:#c00">org.bluez.Error.A2DP.InvalidBitRate</span>
- <span style="color:#c00">org.bluez.Error.A2DP.NotSupportedBitRate</span>
- <span style="color:#c00">org.bluez.Error.A2DP.InvalidObjectType</span>
- <span style="color:#c00">org.bluez.Error.A2DP.NotSupportedObjectType</span>
- <span style="color:#c00">org.bluez.Error.A2DP.InvalidChannels</span>
- <span style="color:#c00">org.bluez.Error.A2DP.NotSupportedChannels</span>
- <span style="color:#c00">org.bluez.Error.A2DP.InvalidVersion</span>
- <span style="color:#c00">org.bluez.Error.A2DP.NotSupportedVersion</span>
- <span style="color:#c00">org.bluez.Error.A2DP.NotSupportedMaximumSUL</span>
- <span style="color:#c00">org.bluez.Error.A2DP.InvalidBlockLength</span>
- <span style="color:#c00">org.bluez.Error.A2DP.InvalidCPType</span>
- <span style="color:#c00">org.bluez.Error.A2DP.InvalidCPFormat</span>
- <span style="color:#c00">org.bluez.Error.A2DP.InvalidCodecParameter</span>
- <span style="color:#c00">org.bluez.Error.A2DP.NotSupportedCodecParameter</span>
- <span style="color:#c00">org.bluez.Error.A2DP.InvalidDRC</span>
- <span style="color:#c00">org.bluez.Error.A2DP.NotSupportedDRC</span>

#### SelectConfiguration

Signature: `array{byte} SelectConfiguration(array{byte} capabilities)`

Select preferable configuration from supported capabilities.

Returns a configuration which can be used to set up a transport. See
[org.bluez.MediaTransport](org.bluez.MediaTransport.md) for possible values.

Note: There is no need to cache the selected configuration. On success, the
configuration is sent back as the parameter of **SetConfiguration**.

#### SelectProperties

Signature: `dict SelectProperties(dict capabilities)`

Select BAP unicast configuration from supported capabilities:

- **Endpoint** (`object`)
- **Capabilities** (`array{byte}`)
- **Metadata** (`array{byte}`)
- **Locations** (`uint32`)
- **ChannelAllocation** (`uint32_t`)
- **QoS** (`dict`)
- **Framing** (`byte`)
- **PHY** (`byte`)
- **MaximumLatency** (`uint16`)
- **MinimumDelay** (`uint32`)
- **MaximumDelay** (`uint32`)
- **PreferredMinimumDelay** (`uint32`)
- **PreferredMaximumDelay** (`uint32`)

See **MediaEndpoint Properties** below for possible values.

Returns a configuration which can be used to set up a transport:

- **Capabilities** (`array{byte}`)
- **Metadata** (`array{byte}`, optional)
- **QoS** (`dict`)

See **SetConfiguration** for possible values.

Note: There is no need to cache the selected properties. On success, the
configuration is sent back as the parameter of **SetConfiguration**.

#### ClearConfiguration

Signature: `void ClearConfiguration(object transport)`

Clear transport configuration.

**Server role** (ISO only): close the stream associated with the given
transport. If the path given is the path of this endpoint, all its streams are
closed.

#### Reconfigure

Signature: `void Reconfigure(dict properties)` (ISO only)

Reconfigure a BAP unicast endpoint. This closes all existing streams of the
endpoint and restarts the configuration selection flow, which triggers
*SelectProperties* and allows the sound server to modify the configuration.

Arguments in `properties`:

- **Defer** (`boolean`, optional): If true, mark endpoint for reconfiguration
  but postpone it until a non-deferred **Reconfigure** is called on an endpoint
  of the same device. This is required when reconfiguring source and sink
  streams intended to be combined into the same CIG (possibly bidirectional
  CIS).

#### Release

Signature: `void Release()`

Called when **bluetoothd(8)** unregisters the endpoint.

An endpoint can use it to do cleanup. There is no need to unregister the
endpoint manually because it has already been unregistered by the time this
method is called.

### MediaEndpoint Properties

| Property | Type | Access | Description |
| --- | --- | --- | --- |
| UUID | `string` | `readonly`, `optional` | UUID of the profile which the endpoint is for. |
| Codec | `byte` | `readonly`, `optional` | Assigned number of codec that the endpoint implements. Values should match the profile specification indicated by the UUID. |
| Vendor | `uint32_t` | `readonly`, `optional` | Vendor-specific Company ID, Codec ID tuple implemented by the endpoint. Set when Vendor Specific Codec (`0xff`) is used. |
| Capabilities | `array{byte}` | `readonly`, `optional` | Capabilities blob; size and byte order must match. |
| Metadata | `array{byte}` | `readonly`, `optional` | Metadata blob; size and byte order must match. |
| Device | `object` | `readonly`, `optional` | [org.bluez.Device](org.bluez.Device.md) object path the endpoint belongs to. |
| DelayReporting | `bool` | `readonly`, `optional` | Indicates if endpoint supports Delay Reporting. |
| Locations | `uint32` | `readonly`, `optional`, ISO only, experimental | Indicates endpoint supported locations. |
| SupportedContext | `uint16` | `readonly`, `optional`, ISO only, experimental | Indicates endpoint supported audio context. |
| Context | `uint16` | `readonly`, `optional`, ISO only, experimental | Indicates endpoint available audio context. |
| QoS | `dict` | `readonly`, `optional`, ISO only, experimental | QoS capabilities. See details below. |
| SupportedFeatures | `dict` | `readonly`, ISO only, experimental | Supported features per service UUID. See details below. |

#### QoS (MediaEndpoint)

Values:

- **Framing** (`byte`): `0x00` unframed PDUs supported, `0x01` unframed PDUs
  not supported.
- **PHY** (`byte`): bit 0 `LE 1M`, bit 1 `LE 2M`, bit 2 `LE Coded`.
- **Retransmissions** (`byte`): Preferred number of retransmissions.
- **MaximumLatency** (`uint16`): Maximum latency.
- **MinimumDelay** (`uint32`): Minimum presentation delay.
- **MaximumDelay** (`uint32`): Maximum presentation delay.
- **PreferredMinimumDelay** (`uint32`): Preferred minimum presentation delay.
- **PreferredMaximumDelay** (`uint32`): Preferred maximum presentation delay.

#### SupportedFeatures (MediaEndpoint)

Dictionary representing supported features for different services. Keys are
service UUIDs; values are arrays of strings.

Services:

- `00001855-0000-1000-8000-00805f9b34fb` (TMAP Features): `cg`, `ct`, `ums`,
  `umr`, `bms`, `bmr`.
- `00001858-0000-1000-8000-00805f9b34fb` (GMAP Features): `ugg`, `ugt`, `bgs`,
  `bgr`, `ugg-multiplex`, `ugg-96kbps-source`, `ugg-multisink`, `ugt-source`,
  `ugt-80kbps-source`, `ugt-sink`, `ugt-64kbps-sink`, `ugt-multiplex`,
  `ugt-multisink`, `ugt-multisource`, `bgs-96kbps`, `bgr-multisink`,
  `bgr-multiplex`.
