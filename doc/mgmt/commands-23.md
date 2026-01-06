# mgmt commands (part 23)

### Commands

#### Read Default System Configuration (since 1.18)

**Command Code:** `0x004B`  
**Controller Index:** `<controller id>`  
**Command Parameters:** none

**Return Parameters:** `Parameter_Type[]` (2 Octet), `Value_Length[]` (1 Octet),
`Value[]` (0-255 Octets), `...[]`

This command reads a list of default controller parameters.

`Parameter_Type` values:

| Value | Description |
| --- | --- |
| `0x0000` | BR/EDR Page Scan Type |
| `0x0001` | BR/EDR Page Scan Interval |
| `0x0002` | BR/EDR Page Scan Window |
| `0x0003` | BR/EDR Inquiry Scan Type |
| `0x0004` | BR/EDR Inquiry Scan Interval |
| `0x0005` | BR/EDR Inquiry Scan Window |
| `0x0006` | BR/EDR Link Supervision Timeout |
| `0x0007` | BR/EDR Page Timeout |
| `0x0008` | BR/EDR Min Sniff Interval |
| `0x0009` | BR/EDR Max Sniff Interval |
| `0x000A` | LE Advertisement Min Interval |
| `0x000B` | LE Advertisement Max Interval |
| `0x000C` | LE Multi Advertisement Rotation Interval |
| `0x000D` | LE Scanning Interval for auto connect |
| `0x000E` | LE Scanning Window for auto connect |
| `0x000F` | LE Scanning Interval for wake scenarios |
| `0x0010` | LE Scanning Window for wake scenarios |
| `0x0011` | LE Scanning Interval for discovery |
| `0x0012` | LE Scanning Window for discovery |
| `0x0013` | LE Scanning Interval for adv monitoring |
| `0x0014` | LE Scanning Window for adv monitoring |
| `0x0015` | LE Scanning Interval for connect |
| `0x0016` | LE Scanning Window for connect |
| `0x0017` | LE Min Connection Interval |
| `0x0018` | LE Max Connection Interval |
| `0x0019` | LE Connection Latency |
| `0x001A` | LE Connection Supervision Timeout |
| `0x001B` | LE Autoconnect Timeout |
| `0x001D` | LE Allow List Scanning Duration for adv monitoring |
| `0x001E` | LE No Filter Scanning Duration for adv monitoring |
| `0x001F` | LE Enable Interleave Scan for adv monitoring |
| `0x0020` | BR/EDR Connection Idle Timeout |

This command can be used at any time. It returns a list of supported default
parameters with their current values.

This command generates a Command Complete event on success or a Command Status
 event on failure.

Possible errors:

- <span style="color:#c00">Invalid Parameters</span>
- <span style="color:#c00">Invalid Index</span>

#### Set Default System Configuration (since 1.18)

**Command Code:** `0x004C`  
**Controller Index:** `<controller id>`  
**Command Parameters:** `Parameter_Type[]` (2 Octet), `Value_Length[]` (1 Octet),
`Value[]` (0-255 Octets), `...[]`

**Return Parameters:** none

This command sets a list of default controller parameters.

See Read Default System Configuration for the supported `Parameter_Type` values.

This command can be used when the controller is not powered and all supported
parameters will be programmed once powered.

When providing unsupported or invalid values, no parameter value is changed and
all values are discarded.

This command generates a Command Complete event on success or a Command Status
 event on failure.

Possible errors:

- <span style="color:#c00">Rejected</span>
- <span style="color:#c00">Not Supported</span>
- <span style="color:#c00">Invalid Parameters</span>
- <span style="color:#c00">Invalid Index</span>

#### Read Default Runtime Configuration (since 1.18)

**Command Code:** `0x004D`  
**Controller Index:** `<controller id>`  
**Command Parameters:** none

**Return Parameters:** `Parameter_Type[]` (2 Octet), `Value_Length[]` (1 Octet),
`Value[]` (0-255 Octets), `...[]`

This command reads a list of default runtime parameters.

Currently no `Parameter_Type` values are defined and an empty list is returned.

This command can be used at any time. It returns a list of supported default
parameters with their current values.

This command generates a Command Complete event on success or a Command Status
 event on failure.

Possible errors:

- <span style="color:#c00">Invalid Parameters</span>
- <span style="color:#c00">Invalid Index</span>

#### Set Default Runtime Configuration (since 1.18)

**Command Code:** `0x004E`  
**Controller Index:** `<controller id>`  
**Command Parameters:** `Parameter_Type[]` (2 Octet), `Value_Length[]` (1 Octet),
`Value[]` (0-255 Octets), `...[]`

**Return Parameters:** none

This command sets a list of default runtime parameters.

See Read Default Runtime Configuration for supported `Parameter_Type` values.

This command can be used at any time and changes the runtime default. Changes do
not apply to existing connections or currently active operations.

When providing unsupported or invalid values, no parameter value is changed and
all values are discarded.

This command generates a Command Complete event on success or a Command Status
 event on failure.

Possible errors:

- <span style="color:#c00">Rejected</span>
- <span style="color:#c00">Not Supported</span>
- <span style="color:#c00">Invalid Parameters</span>
- <span style="color:#c00">Invalid Index</span>

### Navigation

- [Previous: Commands (Part 22)](commands-22.md)
- [Next: Commands (Part 24)](commands-24.md)
