# mgmt commands (part 22)

### Commands

#### Read Controller Capabilities (since 1.17)

**Command Code:** `0x0048`  
**Controller Index:** `<controller id>`  
**Command Parameters:** none

**Return Parameters:** `Capabilities_Data_Length` (2 Octets),
`Capabilities_Data[]` (0-65535 Octets)

This command retrieves supported capabilities for the controller or host stack.

`Capabilities_Data` uses the same format as `EIR_Data`, but with the namespace
defined below.

`Data Type` values:

| Data Type | Name |
| --- | --- |
| `0x01` | Flags |
| `0x02` | Max Encryption Key Size (BR/EDR) |
| `0x03` | Max Encryption Key Size (LE) |
| `0x04` | Supported Tx Power (LE) |

Flags (data type `0x01`) bits:

| Bit | Description |
| --- | --- |
| 0 | Remote public key validation (BR/EDR) |
| 1 | Remote public key validation (LE) |
| 2 | Encryption key size enforcement (BR/EDR) |
| 3 | Encryption key size enforcement (LE) |

Max Encryption Key Size (data types `0x02` and `0x03`):

When present, this is a 1 Octet value indicating the maximum encryption key
size. If absent, the maximum key size is unknown.

Supported LE Tx Power (data type `0x04`):

When present, this 2 Octet field provides the min and max LE Tx power supported
by the controller, respectively, as reported by the LE Read Transmit Power HCI
command. If this field is not available, it indicates that the LE Read Transmit
Power HCI command was not available.

This command generates a Command Complete event on success or a Command Status
 event on failure.

Possible errors:

- <span style="color:#c00">Invalid Parameters</span>
- <span style="color:#c00">Invalid Index</span>

#### Read Experimental Features Information (since 1.17)

**Command Code:** `0x0049`  
**Controller Index:** `<controller id>` or `<non-controller>`  
**Command Parameters:** none

**Return Parameters:** `Feature_Count` (2 Octets), `UUID[]` (16 Octets),
`Flags[]` (4 Octets), `...[]`

This command retrieves experimental features supported by the host stack.

The `UUID` values are intentionally unstable and may change as features mature.
Mapping a UUID to functionality is out of scope for this document.

`Flags` bits:

| Bit | Description |
| --- | --- |
| 0 | Feature active |
| 1 | Causes change in supported settings |

This command generates a Command Complete event on success or a Command Status
 event on failure.

Possible errors:

- <span style="color:#c00">Invalid Parameters</span>
- <span style="color:#c00">Invalid Index</span>

#### Set Experimental Feature (since 1.17)

**Command Code:** `0x004A`  
**Controller Index:** `<controller id>` or `<non-controller>`  
**Command Parameters:** `UUID` (16 Octets), `Action` (1 Octet)

**Return Parameters:** `UUID` (16 Octets), `Flags` (4 Octets)

This command changes the setting of an experimental feature of the host stack.

The `UUID` must be a supported value returned by Read Experimental Features
Information.

`Action` is UUID-specific, but in most cases it is an on/off toggle:

| Value | Description |
| --- | --- |
| `0x00` | Disable feature |
| `0x01` | Enable feature |

Whether the command can be used when the controller is powered depends on the
feature. See `Flags` from Read Experimental Features Information to determine if
power-down is required.

`Flags` bits:

| Bit | Description |
| --- | --- |
| 0 | Feature active |
| 1 | Supported settings changed |

If a feature causes supported settings to change, re-read controller
information.

If `UUID` is an empty UUID (16 x `0x00`), all experimental features are
deactivated.

This command generates a Command Complete event on success or a Command Status
 event on failure.

Possible errors:

- <span style="color:#c00">Invalid Parameters</span>
- <span style="color:#c00">Not Powered</span>
- <span style="color:#c00">Invalid Index</span>

### Navigation

- [Previous: Commands (Part 21)](commands-21.md)
- [Next: Commands (Part 23)](commands-23.md)
