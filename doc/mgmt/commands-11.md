# mgmt commands (part 11)

### Commands

#### Confirm Name

**Command Code:** `0x0025`  
**Controller Index:** `<controller id>`  
**Command Parameters:** `Address` (6 Octets), `Address_Type` (1 Octet),
`Name_Known` (1 Octet)

**Return Parameters:** `Address` (6 Octets), `Address_Type` (1 Octet)

This command is only valid during device discovery and is expected for each
Device Found event with the Confirm Name flag set.

Possible values for `Address_Type`:

| Value | Description |
| --- | --- |
| 0x00 | BR/EDR |
| 0x01 | LE Public |
| 0x02 | LE Random |

`Name_Known` should be set to `0x01` if user space knows the device name and
`0x00` if it does not. If set to `0x00`, the kernel performs name resolution for
that device.

This command can only be used when the controller is powered.

This command generates a Command Complete event on success or failure.

Possible errors:

- <span style="color:#c00">Failed</span>
- <span style="color:#c00">Invalid Parameters</span>
- <span style="color:#c00">Invalid Index</span>

#### Block Device

**Command Code:** `0x0026`  
**Controller Index:** `<controller id>`  
**Command Parameters:** `Address` (6 Octets), `Address_Type` (1 Octet)

**Return Parameters:** `Address` (6 Octets), `Address_Type` (1 Octet)

Adds a device to the list of devices that should be blocked from connecting to
 the local controller.

Possible values for `Address_Type`:

| Value | Description |
| --- | --- |
| 0x00 | BR/EDR |
| 0x01 | LE Public |
| 0x02 | LE Random |

For LE devices, blocking takes precedence over auto-connection actions from Add
Device. Blocked devices will not be auto-connected or reported during
background scanning. If the controller is connectable, directed advertising
from blocked devices is ignored.

Connections created from advertising of the controller are dropped if the
remote device is blocked.

This command can be used when the controller is not powered.

This command generates a Command Complete event on success or failure.

Possible errors:

- <span style="color:#c00">Failed</span>
- <span style="color:#c00">Invalid Parameters</span>
- <span style="color:#c00">Invalid Index</span>

#### Unblock Device

**Command Code:** `0x0027`  
**Controller Index:** `<controller id>`  
**Command Parameters:** `Address` (6 Octets), `Address_Type` (1 Octet)

**Return Parameters:** `Address` (6 Octets), `Address_Type` (1 Octet)

Removes a device from the blocked list (added via Block Device).

Possible values for `Address_Type`:

| Value | Description |
| --- | --- |
| 0x00 | BR/EDR |
| 0x01 | LE Public |
| 0x02 | LE Random |

When `Address` is `00:00:00:00:00:00`, all previously blocked devices are
unblocked.

This command can be used when the controller is not powered.

This command generates a Command Complete event on success or failure.

Possible errors:

- <span style="color:#c00">Invalid Parameters</span>
- <span style="color:#c00">Invalid Index</span>

### Next

- [Commands (Part 12)](commands-12.md)

#### Set Device ID

**Command Code:** `0x0028`  
**Controller Index:** `<controller id>`  
**Command Parameters:** `Source` (2 Octets), `Vendor` (2 Octets), `Product` (2
Octets), `Version` (2 Octets)

**Return Parameters:** none

This command can be used when the controller is not powered and all settings
will be programmed once powered.

`Source` selects the organization that assigned the `Vendor` parameter:

| Value | Description |
| --- | --- |
| 0x0000 | Disable Device ID |
| 0x0001 | Bluetooth SIG |
| 0x0002 | USB Implementer's Forum |

The information is put into EIR data. If the controller does not support EIR or
SSP is disabled, this command still succeeds. The information is stored for
later use and survives toggling SSP on and off.

This command generates a Command Complete event on success or a Command Status
 event on failure.

Possible errors:

- <span style="color:#c00">Invalid Parameters</span>
- <span style="color:#c00">Invalid Index</span>
