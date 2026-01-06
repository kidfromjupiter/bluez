# mgmt commands (part 17)

### Commands

#### Set Public Address (since 1.7)

**Command Code:** `0x0039`  
**Controller Index:** `<controller id>`  
**Command Parameters:** `Address` (6 Octets)

**Return Parameters:** `Missing_Options` (4 Octets)

This command configures the public address. Since a vendor-specific procedure
is required, this command might not be supported by all controllers. It is
likely only embedded controllers support it.

When Bluetooth public address configuration is indicated in the supported
options mask, this command can configure the public address.

Public address can only be configured when the controller is powered off.

For an unconfigured controller, when `Missing_Options` returns an empty mask,
an Index Added event is expected for the fully configured controller.

For a fully configured controller, the current controller index becomes invalid
and an Unconfigured Index Removed event is sent. After the address is changed,
an Index Added event is sent. The controller index may change.

All previously configured parameters and settings are lost when this command
succeeds. The controller must be treated as new. Use this only when you know
what you are doing.

This command generates a Command Complete event on success or a Command Status
 event on failure.

Possible errors:

- <span style="color:#c00">Rejected</span>
- <span style="color:#c00">Not Supported</span>
- <span style="color:#c00">Invalid Parameters</span>
- <span style="color:#c00">Invalid Index</span>

#### Start Service Discovery (since 1.8)

**Command Code:** `0x003A`  
**Controller Index:** `<controller id>`  
**Command Parameters:** `Address_Type` (1 Octet), `RSSI_Threshold` (1 Octet),
`UUID_Count` (2 Octets), `UUID[i]` (16 Octets)

**Return Parameters:** `Address_Type` (1 Octet)

This command starts discovery of remote devices with a specific UUID. A Device
Found event is sent for each discovered device.

`Address_Type` is a bitwise OR of the following bits:

| Bit | Description |
| --- | --- |
| 0 | BR/EDR |
| 1 | LE Public |
| 2 | LE Random |

Examples:

| Value | Description |
| --- | --- |
| 0x01 | BR/EDR |
| 0x06 | LE (public & random) |
| 0x07 | BR/EDR/LE (interleaved discovery) |

Service discovery uses active scanning for LE and searches for UUIDs in
advertising data and scan response data.

Devices with RSSI smaller than `RSSI_Threshold` are not reported. Setting
`RSSI_Threshold` to `127` reports all devices.

UUID list is a logical OR; any matching UUID triggers DeviceFound. An empty
list (`UUID_Count` 0) reports all devices above the RSSI threshold.

If `RSSI_Threshold` is 127 and `UUID_Count` is 0, this behaves exactly like
Start Discovery.

When discovery starts, a Discovery event notifies similar to Start Discovery.

This command can only be used when the controller is powered.

This command generates a Command Complete event on success or failure.

Possible errors:

- <span style="color:#c00">Busy</span>
- <span style="color:#c00">Not Supported</span>
- <span style="color:#c00">Invalid Parameters</span>
- <span style="color:#c00">Not Powered</span>
- <span style="color:#c00">Invalid Index</span>

### Next

- [Commands (Part 18)](commands-18.md)

#### Read Local Out Of Band Extended Data (since 1.9)

**Command Code:** `0x003B`  
**Controller Index:** `<controller id>`  
**Command Parameters:** `Address_Type` (1 Octet)

**Return Parameters:** `Address_Type` (1 Octet), `EIR_Data_Length` (2 Octets),
`EIR_Data` (0-65535 Octets)

This command reads local Out Of Band data encoded as extended inquiry response
or advertising data.

`Address_Type` is a bitwise OR of the following bits:

| Bit | Description |
| --- | --- |
| 0 | BR/EDR |
| 1 | LE Public |
| 2 | LE Random |

Examples:

| Value | Description |
| --- | --- |
| 0x01 | BR/EDR |
| 0x06 | LE (public & random) |
| 0x07 | Reserved (not in use) |

For BR/EDR (Address_Type 1), the returned information may include:

- Class of Device
- Simple Pairing Hash C-192 (optional)
- Simple Pairing Randomizer R-192 (optional)
- Simple Pairing Hash C-256 (optional)
- Simple Pairing Randomizer R-256 (optional)
- Service Class UUID (optional)
- Bluetooth Local Name (optional)

Simple Pairing Hash/Randomizer C-256/R-256 are only included when secure
connections is enabled.

Device Address (BD_ADDR) is not included in `EIR_Data` and must be taken from
controller information.

For LE (Address_Type 6), the returned information contains advertising data.

LE returned information includes:

- LE Bluetooth Device Address
- LE Role
- LE Secure Connections Confirmation Value (optional)
- LE Secure Connections Random Value (optional)
- Appearance (optional)
- Local Name (optional)
- Flags

The LE Secure Connections Confirmation Value and LE Secure Connections Random
Value fields are only included when secure connections is enabled.

The Security Manager TK Value cannot be provided by this command. The Out Of
Band information here is for asymmetric exchanges based on Diffie-Hellman key
exchange. The TK Value is a symmetric random number that must be acquired and
agreed upon differently.

The returned information from BR/EDR and LE controller types are not related to
each other. Once used over an Out Of Band link, a new set of information should
be requested.

When Secure Connections Only mode is enabled, the fields for Simple Pairing
Hash C-192 and Simple Pairing Randomizer R-192 are not returned. Only the
strong secure connections pairing fields are included.

Values returned by this command become invalid when the controller is powered
down. After each power cycle, call this command again to get updated
information.

This command generates a Command Complete event on success or a Command Status
 event on failure.

Possible errors:

- <span style="color:#c00">Busy</span>
- <span style="color:#c00">Not Supported</span>
- <span style="color:#c00">Invalid Parameters</span>
- <span style="color:#c00">Not Powered</span>
- <span style="color:#c00">Invalid Index</span>
