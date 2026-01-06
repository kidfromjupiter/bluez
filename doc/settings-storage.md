# BlueZ settings storage

## Purpose

This document describes the directory structure used for BlueZ settings
storage. It is the primary, up-to-date reference for storage details.

It is intended for developers. Direct access to the storage outside
`bluetoothd` is strongly discouraged.

Adapter and remote device information is read from storage during object
initialization. Writes occur immediately on every value change.

The default storage directory is `/var/lib/bluetooth`. You can adjust this with
`--localstatedir` during configure (default `--localstatedir=/var`).

When running as a service, the storage directory can be set via the
`StateDirectory` environment variable:
<https://www.freedesktop.org/software/systemd/man/systemd.exec.html>

All files use an INI-style format.

## Storage directory structure

The storage root directory can contain an optional `addresses` file used to
manage adapters without pre-allocated addresses. The file format is:

```ini
[Static]
<manufacturer id> = <array of addresses>
```

Each adapter with an assigned address has its own subdirectory under the root,
named by the adapter address, which contains:

- a settings file for the local adapter
- an attributes file containing attributes of supported LE services
- an admin policy file containing current values of admin policies
- a `cache` directory containing:
  - one file per device, named by remote device address, containing device name
- one directory per remote device, named by remote device address, containing:
  - an `info` file
  - an `attributes` file containing attributes of remote LE services
  - a `ccc` file containing persistent Client Characteristic Configuration
    (CCC) descriptor information for GATT characteristics

Directory layout example:

```text
/var/lib/bluetooth/<adapter address>/
    ./settings
    ./attributes
    ./admin_policy_settings
    ./cache/
        ./<remote device address>
        ./<remote device address>
        ...
    ./<remote device address>/
        ./info
        ./attributes
        ./ccc
    ./<remote device address>/
        ./info
        ./attributes
    ...
```

## Settings file format

The settings file contains a single `[General]` group with adapter information:

| Field | Type | Description |
| --- | --- | --- |
| `Alias` | String | Friendly user-provided name advertised for this adapter (overrides the system name). |
| `Discoverable` | Boolean | Discoverability of the adapter. |
| `PairableTimeout` | Integer | Seconds to stay pairable before returning to non-pairable. `0` disables the timer. |
| `DiscoverableTimeout` | Integer | Seconds to stay discoverable before returning to non-discoverable. `0` disables the timer. |

Sample:

```ini
[General]
Name=My PC
Discoverable=false
Pairable=true
DiscoverableTimeout=0
```

## Identity file format

The identity file contains a single `[General]` group with identity
information:

| Field | Type | Description |
| --- | --- | --- |
| `IdentityResolvingKey` | String | 128-bit value of the IRK. |

Sample:

```ini
[General]
IdentityResolvingKey=00112233445566778899aabbccddeeff
```

## Attributes file format

The attributes file lists all attributes supported by the local adapter or
remote device.

Attributes are stored using their handle as the group name (decimal format).
Each group contains:

| Field | Type | Description |
| --- | --- | --- |
| `UUID` | String | 128-bit UUID of the attribute. |
| `Value` | String | Attribute value, hex encoded. |
| `EndGroupHandle` | Integer | End group handle in decimal format. |

Sample:

```ini
[1]
UUID=00002800-0000-1000-8000-00805f9b34fb
Value=0018

[4]
UUID=00002803-0000-1000-8000-00805f9b34fb
Value=020600002A

[6]
UUID=00002a00-0000-1000-8000-00805f9b34fb
Value=4578616D706C6520446576696365
```

## Admin policy file format

The admin policy file stores the current value of each admin policy. The
`[General]` group contains:

| Field | Type | Description |
| --- | --- | --- |
| `ServiceAllowlist` | List of strings | List of service UUIDs allowed by the adapter in 128-bit format, separated by `,`. Default is empty. |

Sample:

```ini
[General]
ServiceAllowlist=
```

## CCC file format

The `ccc` file stores CCC descriptor values for GATT characteristics with
notification or indication enabled by the remote device.

Information is stored using the CCC attribute handle as the group name
(decimal format). Each group contains:

| Field | Type | Description |
| --- | --- | --- |
| `Value` | String | CCC descriptor value encoded in hexadecimal. |

## Cache directory file format

Each file (named by remote device address) can include multiple groups:
`General`, `ServiceRecords`, `Attributes`, `Endpoints`, `NameResolving`.

- **ServiceRecords**: SDP records stored by handle (hex format).
- **Attributes**: GATT database stored by attribute handle (hex). Values are a
  serialized form of all data required to re-create the attribute. Fields are
  separated by `:`.
- **Endpoints**: A2DP remote endpoints stored by SEID (hex). Fields are
  separated by `:`. May contain a `LastUsed` entry with local and remote SEIDs.
- **NameResolving**: Information used to avoid repeated name resolution for
  unresponsive devices.

`[General]` group fields:

| Field | Type | Description |
| --- | --- | --- |
| `Name` | String | Remote device friendly name. |
| `ShortName` | String | Remote device shortened name. |

`[ServiceRecords]` group fields:

| Field | Type | Description |
| --- | --- | --- |
| `<0x...>` | String | SDP record, hex encoded. |

`[Attributes]` group value format:

- Primary service: `2800:end_handle:uuid`
- Secondary service: `2801:end_handle:uuid`
- Included service: `2802:start_handle:end_handle:uuid`
- Characteristic: `2803:value_handle:properties:uuid`
- Descriptor: `value:uuid` or `uuid`

Sample Attributes section:

```ini
[Attributes]
0001=2800:0005:1801
0002=2803:0003:20:2a05
0014=2800:001c:1800
0015=2803:0016:02:2a00
0017=2803:0018:02:2a01
0019=2803:001a:02:2aa6
0028=2800:ffff:0000180d-0000-1000-8000-00805f9b34fb
0029=2803:002a:10:00002a37-0000-1000-8000-00805f9b34fb
002b=2803:002c:02:00002a38-0000-1000-8000-00805f9b34fb
002d=2803:002e:08:00002a39-0000-1000-8000-00805f9b34fb
```

`[Endpoints]` group format:

```text
<xx>:<xx>:<xx>::<xx...>
```

- First field: endpoint type
- Followed by codec type and delay reporting
- Then capabilities, hex encoded

`LastUsed` entry format:

```text
LastUsed:<xx>:<xx>
```

Fields are local and remote SEIDs in hex.

`[NameResolving]` group fields:

| Field | Type | Description |
| --- | --- | --- |
| `FailedTime` | Integer | Last time name resolution failed, measured from an arbitrary fixed point in the past. |

## Info file format

The `info` file can include multiple groups related to a remote device:
`General`, `DeviceID`, `LinkKey`, `LongTermKey`, `PeripheralLongTermKey`,
`ConnectionParameters`, `LocalSignatureKey`, `RemoteSignatureKey`, and
`ServiceChanged`.

`[General]` group fields:

| Field | Type | Description |
| --- | --- | --- |
| `Name` | String | Remote device friendly name. |
| `Alias` | String | Alias name. |
| `Class` | String | Device class in hex (for example `0x000000`). |
| `Appearance` | String | Device appearance in hex (for example `0x0000`). |
| `SupportedTechnologies` | List of strings | Technologies supported by the device, separated by `;` (`BR/EDR` or `LE`). |
| `AddressType` | String | Address type (`static` or `public`). |
| `Trusted` | Boolean | True if the remote device is trusted. |
| `Blocked` | Boolean | True if the remote device is blocked. |
| `Services` | List of strings | Service UUIDs advertised by the remote device in 128-bit format, separated by `;`. |
| `PreferredBearer` | String | Preferred bearer for the remote device. |
| `LastUsedBearer` | String | Last used bearer for the remote device. |

`[DeviceID]` group fields:

| Field | Type | Description |
| --- | --- | --- |
| `Source` | Integer | Assigner of Device ID. |
| `Vendor` | Integer | Device vendor. |
| `Product` | Integer | Device product. |
| `Version` | Integer | Device version. |

`[LinkKey]` group fields:

| Field | Type | Description |
| --- | --- | --- |
| `Key` | String | Key in hex format. |
| `Type` | Integer | Link key type. |
| `PINLength` | Integer | PIN length. |

`[LongTermKey]` group fields:

| Field | Type | Description |
| --- | --- | --- |
| `Key` | String | Long term key in hex format. |
| `Authenticated` | Boolean | True if the remote device was authenticated. |
| `EncSize` | Integer | Encryption size. |
| `EDiv` | Integer | Encrypted diversifier. |
| `Rand` | Integer | Randomizer. |

`[PeripheralLongTermKey]` group:

- Same fields as `[LongTermKey]`, except for peripheral keys.

`[ConnectionParameters]` group fields:

| Field | Type | Description |
| --- | --- | --- |
| `MinInterval` | Integer | Minimum connection interval. |
| `MaxInterval` | Integer | Maximum connection interval. |
| `Latency` | Integer | Connection latency. |
| `Timeout` | Integer | Supervision timeout. |

`[LocalSignatureKey]` and `[RemoteSignatureKey]` group fields:

| Field | Type | Description |
| --- | --- | --- |
| `Key` | String | Key in hex format. |
| `Counter` | Integer | Signing counter. |
| `Authenticated` | Boolean | True if the key is authenticated. |

`[ServiceChanged]` section fields:

| Field | Type | Description |
| --- | --- | --- |
| `CCC_LE` | Integer | CCC value for LE transport. |
| `CCC_BR/EDR` | Integer | CCC value for BR/EDR transport. |
