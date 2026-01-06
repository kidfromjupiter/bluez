# mgmt commands (part 10)

### Commands

#### Start Discovery

**Command Code:** `0x0023`  
**Controller Index:** `<controller id>`  
**Command Parameters:** `Address_Type` (1 Octet)

**Return Parameters:** `Address_Type` (1 Octet)

This command starts discovery of remote devices. A Device Found event is sent
for each discovered device.

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

This command can only be used when the controller is powered.

This command generates a Command Complete event on success or failure.

Possible errors:

- <span style="color:#c00">Busy</span>
- <span style="color:#c00">Not Supported</span>
- <span style="color:#c00">Invalid Parameters</span>
- <span style="color:#c00">Not Powered</span>
- <span style="color:#c00">Invalid Index</span>

#### Stop Discovery

**Command Code:** `0x0024`  
**Controller Index:** `<controller id>`  
**Command Parameters:** `Address_Type` (1 Octet)

**Return Parameters:** `Address_Type` (1 Octet)

This command stops the discovery process started using Start Discovery.

This command can only be used when the controller is powered.

This command generates a Command Complete event on success or failure.

Possible errors:

- <span style="color:#c00">Rejected</span>
- <span style="color:#c00">Invalid Parameters</span>
- <span style="color:#c00">Invalid Index</span>

### Next

- [Commands (Part 11)](commands-11.md)
