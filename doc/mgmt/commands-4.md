# mgmt commands (part 4)

### Commands

#### Set Local Name

**Command Code:** `0x000F`  
**Controller Index:** `<controller id>`  
**Command Parameters:** `Name` (249 Octets), `Short_Name` (11 Octets)

**Return Parameters:**

| Name | Size |
| --- | --- |
| Name | 249 Octets |
| Short_Name | 11 Octets |

This command sets the local name of a controller. The command parameters also
include a short name used when the full name does not fit in EIR/AD data.

The name parameters must always end with a NUL byte (failure to do so causes the
command to fail).

This command can be used when the controller is not powered and all settings
will be programmed once powered.

The values of name and short name are remembered when switching the controller
off and back on again. The name and short name only need to be set once when a
new controller is found and will stay until removed.

This command generates a Command Complete event on success or a Command Status
 event on failure.

Possible errors:

- <span style="color:#c00">Invalid Parameters</span>
- <span style="color:#c00">Invalid Index</span>

#### Add UUID

**Command Code:** `0x0010`  
**Controller Index:** `<controller id>`  
**Command Parameters:** `UUID` (16 Octets), `SVC_Hint` (1 Octet)

**Return Parameters:**

| Name | Size |
| --- | --- |
| Class_Of_Device | 3 Octets |

This command adds a UUID to be published in EIR data. The accompanying
`SVC_Hint` parameter tells the kernel whether the service class bits of the
Class of Device value need modifying due to this UUID.

This command can be used when the controller is not powered and all settings
will be programmed once powered.

If the controller is powered off, `0x000000` is returned for the class of
 device parameter. After power on, the new value is announced via class of
 device changed event.

This command generates a Command Complete event on success or a Command Status
 event on failure.

Possible errors:

- <span style="color:#c00">Busy</span>
- <span style="color:#c00">Invalid Parameters</span>
- <span style="color:#c00">Invalid Index</span>

### Next

- [Commands (Part 5)](commands-5.md)

#### Remove UUID

**Command Code:** `0x0011`  
**Controller Index:** `<controller id>`  
**Command Parameters:** `UUID` (16 Octets)

**Return Parameters:**

| Name | Size |
| --- | --- |
| Class_Of_Device | 3 Octets |

This command removes a UUID previously added using Add UUID.

When the UUID parameter is an empty UUID (16 x `0x00`), all previously loaded
UUIDs are removed.

This command can be used when the controller is not powered and all settings
will be programmed once powered.

If the controller is powered off, `0x000000` is returned for the class of
 device parameter. After power on, the new value is announced via class of
 device changed event.

This command generates a Command Complete event on success or a Command Status
 event on failure.

Possible errors:

- <span style="color:#c00">Busy</span>
- <span style="color:#c00">Invalid Parameters</span>
- <span style="color:#c00">Invalid Index</span>
