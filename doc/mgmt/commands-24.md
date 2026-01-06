# mgmt commands (part 24)

### Commands

#### Get Device Flags (since 1.18)

**Command Code:** `0x004F`  
**Controller Index:** `<controller id>`  
**Command Parameters:** `Address` (6 Octets), `Address_Type` (1 Octet)

**Return Parameters:** `Address` (6 Octets), `Address_Type` (1 Octet),
`Supported_Flags` (4 Octets), `Current_Flags` (4 Octets)

This command retrieves additional flags and settings for devices added via Add
Device.

`Address_Type` values:

| Value | Description |
| --- | --- |
| `0x00` | BR/EDR |
| `0x01` | LE Public |
| `0x02` | LE Random |

`Flags` bits:

| Bit | Description |
| --- | --- |
| 0 | Remote Wakeup enabled |
| 1 | Device Privacy Mode enabled |
| 2 | Address Resolution enabled |
| 3 | PAST enabled |

This command generates a Command Complete event on success or a Command Status
 event on failure.

Possible errors:

- <span style="color:#c00">Invalid Parameters</span>
- <span style="color:#c00">Invalid Index</span>

#### Set Device Flags (since 1.18)

**Command Code:** `0x0050`  
**Controller Index:** `<controller id>`  
**Command Parameters:** `Address` (6 Octets), `Address_Type` (1 Octet),
`Current_Flags` (4 Octets)

**Return Parameters:** `Address` (6 Octets), `Address_Type` (1 Octet)

This command configures additional flags and settings for devices added via Add
Device.

`Address_Type` values:

| Value | Description |
| --- | --- |
| `0x00` | BR/EDR |
| `0x01` | LE Public |
| `0x02` | LE Random |

Supported flags can be retrieved via Get Device Flags or the Device Flags
Changed event. Selecting unsupported flags results in Invalid Parameters.

Refer to Get Device Flags for a detailed description of the `Flags` parameters.

This command can be used when the controller is not powered and all settings
will be programmed once powered.

This command generates a Command Complete event on success or a Command Status
 event on failure.

Possible errors:

- <span style="color:#c00">Invalid Parameters</span>
- <span style="color:#c00">Invalid Index</span>

#### Read Advertisement Monitor Features (since 1.18)

**Command Code:** `0x0051`  
**Controller Index:** `<controller id>`  
**Command Parameters:** none

**Return Parameters:** `Supported_Features` (4 Octets), `Enabled_Features`
(4 Octets), `Max_Num_Handles` (2 Octets), `Max_Num_Patterns` (1 Octet),
`Num_Handles` (2 Octets), `Handle[]` (2 Octets), `...[]`

This command reads the advertisement monitor features supported by the
controller and stack. `Supported_Features` lists all related features supported
by the controller, while `Enabled_Features` lists the features currently used by
the kernel.

`Supported_Features` and `Enabled_Features` bits:

| Bit | Description |
| --- | --- |
| 0 | Advertisement content monitoring based on patterns with logic OR |

`Max_Num_Handles` indicates the maximum number of supported advertisement
monitors, which can be lower depending on controller limitations.

`Max_Num_Patterns` indicates the maximum number of supported patterns per
monitor.

`Num_Handles` indicates the number of added monitors and is followed by the list
of `Handle` values.

This command can be used when the controller is not powered.

#### Add Advertisement Patterns Monitor (since 1.18)

**Command Code:** `0x0052`  
**Controller Index:** `<controller id>`  
**Command Parameters:** `Pattern_Count` (1 Octet), `AD_Type[]` (1 Octet),
`Offset[]` (1 Octet), `Length[]` (1 Octet), `Value[]` (31 Octets), `...[]`

**Return Parameters:** `Monitor_Handle` (2 Octets)

This command adds an advertisement monitor whose filtering conditions are
patterns. The kernel triggers scanning if at least one monitor is added. If the
controller supports advertisement filtering, the kernel offloads content
filtering to the controller to reduce power consumption; otherwise the kernel
ignores the monitor content. When more than one pattern is provided, OR logic is
applied between patterns, so any advertisement matching at least one pattern is
considered a match.

A pattern contains the following fields:

| Field | Description |
| --- | --- |
| `AD_Data_Type` | Advertising Data Type (Core Specification Supplement) |
| `Offset` | Start index for pattern matching within AD data |
| `Length` | Pattern value length in bytes |
| `Value` | Pattern value bytes |

Example pattern:

```c
{
    0x16, // Service Data - 16-bit UUID
    0x02, // Skip the UUID part.
    0x04, // Length of the value
    {0x11, 0x22, 0x33, 0x44},
}
```

This command can be used when the controller is not powered and all settings
will be programmed once powered.

Possible errors:

- <span style="color:#c00">Failed</span>
- <span style="color:#c00">Busy</span>
- <span style="color:#c00">No Resources</span>
- <span style="color:#c00">Invalid Parameters</span>

### Navigation

- [Previous: Commands (Part 23)](commands-23.md)
- [Next: Commands (Part 25)](commands-25.md)
