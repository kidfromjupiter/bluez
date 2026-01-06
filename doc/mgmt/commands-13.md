# mgmt commands (part 13)

### Commands

#### Set Scan Parameters (since 1.4)

**Command Code:** `0x002C`  
**Controller Index:** `<controller id>`  
**Command Parameters:** `Interval` (2 Octets), `Window` (2 Octets)

**Return Parameters:** none

This command sets the Low Energy scan parameters used for connection
establishment and passive scanning. It is only supported on controllers with LE
support.

This command generates a Command Complete event on success or a Command Status
 event on failure.

Possible errors:

- <span style="color:#c00">Rejected</span>
- <span style="color:#c00">Not Supported</span>
- <span style="color:#c00">Invalid Parameters</span>
- <span style="color:#c00">Invalid Index</span>

#### Set Secure Connections (since 1.5)

**Command Code:** `0x002D`  
**Controller Index:** `<controller id>`  
**Command Parameters:** `Secure_Connections` (1 Octet)

**Return Parameters:** `Current_Settings` (4 Octets)

This command enables or disables Secure Connections support. Allowed values are
`0x00`, `0x01`, and `0x02`. All other values return Invalid Parameters.

- `0x00`: Disable Secure Connections.
- `0x01`: Enable Secure Connections.
- `0x02`: Enable Secure Connections Only mode.

This command is only available for LE-capable controllers and controllers
supporting core spec 4.1 or greater.

This command can be used when the controller is not powered and all settings
will be programmed once powered.

If the controller does not support Secure Connections, the command fails with
Not Supported.

This command generates a Command Complete event on success or a Command Status
 event on failure.

Possible errors:

- <span style="color:#c00">Not Supported</span>
- <span style="color:#c00">Invalid Parameters</span>
- <span style="color:#c00">Invalid Index</span>

### Next

- [Commands (Part 14)](commands-14.md)
