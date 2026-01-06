# mgmt commands (part 21)

### Commands

#### Get PHY Configuration (since 1.15)

**Command Code:** `0x0044`  
**Controller Index:** `<controller id>`  
**Command Parameters:** none

**Return Parameters:** `Supported_PHYs` (4 Octets), `Configurable_PHYs`
(4 Octets), `Selected_PHYs` (4 Octets)

The PHY parameters are bitmasks with the following bits:

| Bit | Description |
| --- | --- |
| 0 | BR 1M 1-Slot |
| 1 | BR 1M 3-Slot |
| 2 | BR 1M 5-Slot |
| 3 | EDR 2M 1-Slot |
| 4 | EDR 2M 3-Slot |
| 5 | EDR 2M 5-Slot |
| 6 | EDR 3M 1-Slot |
| 7 | EDR 3M 3-Slot |
| 8 | EDR 3M 5-Slot |
| 9 | LE 1M TX |
| 10 | LE 1M RX |
| 11 | LE 2M TX |
| 12 | LE 2M RX |
| 13 | LE Coded TX |
| 14 | LE Coded RX |

If BR/EDR is supported, BR 1M 1-Slot is supported by default and cannot be
 deselected. If LE is supported, LE 1M TX and LE 1M RX are supported by default.

Disabling BR/EDR or LE has no impact on the PHY configuration. The configuration
is remembered over power cycles.

This command generates a Command Complete event on success or a Command Status
 event on failure.

Possible errors:

- <span style="color:#c00">Invalid Parameters</span>
- <span style="color:#c00">Invalid Index</span>

#### Set PHY Configuration (since 1.15)

**Command Code:** `0x0045`  
**Controller Index:** `<controller id>`  
**Command Parameters:** `Selected_PHYs` (4 Octet)

**Return Parameters:** none

This command sets the default PHY configuration for the controller. It is stored
and used for all subsequent scanning and connection initiation.

Supported PHYs can be read using Get PHY Configuration. Selecting unsupported
PHYs or deselecting default PHYs results in Invalid Parameters.

This command can be called at any time to change `Selected_PHYs`.

Refer to Get PHY Configuration for the PHYs parameter definition.

This command generates a Command Complete event on success or a Command Status
 event on failure.

Possible errors:

- <span style="color:#c00">Invalid Parameters</span>
- <span style="color:#c00">Invalid Index</span>

#### Load Blocked Keys (since 1.15)

**Command Code:** `0x0046`  
**Controller Index:** `<controller id>`  
**Command Parameters:** `Key_Count` (2 Octets), `Key_Type[]` (1 Octet),
`Value[]` (16 Octets), `...[]`

**Return Parameters:** none

This command provides the kernel a list of keys known to be vulnerable. If a
pairing procedure produces any of these keys, the kernel will silently drop them
and reject encryption.

`Key_Type` values:

| Value | Description |
| --- | --- |
| `0x00` | Link Key (BR/EDR) |
| `0x01` | Long Term Key (LE) |
| `0x02` | Identity Resolving Key (LE) |

This command can be used when the controller is not powered.

This command generates a Command Complete event on success or a Command Status
 event on failure.

Possible errors:

- <span style="color:#c00">Invalid Parameters</span>
- <span style="color:#c00">Invalid Index</span>

#### Set Wideband Speech (since 1.16)

**Command Code:** `0x0047`  
**Controller Index:** `<controller id>`  
**Command Parameters:** `Wideband_Speech` (1 Octet)

**Return Parameters:** `Current_Settings` (4 Octets)

This command enables or disables Wideband Speech support.

The allowed `Wideband_Speech` values are `0x00` and `0x01`. All other values
return Invalid Parameters. `0x00` disables Wideband Speech, `0x01` enables it.

This command is only available for BR/EDR capable controllers and requires
controller specific support.

This command can be used when the controller is not powered and all settings
will be programmed once powered.

If the controller does not support Wideband Speech, the command fails with Not
Supported.

This command generates a Command Complete event on success or a Command Status
 event on failure.

Possible errors:

- <span style="color:#c00">Busy</span>
- <span style="color:#c00">Not Supported</span>
- <span style="color:#c00">Invalid Parameters</span>
- <span style="color:#c00">Invalid Index</span>

### Navigation

- [Previous: Commands (Part 20)](commands-20.md)
- [Next: Commands (Part 22)](commands-22.md)
