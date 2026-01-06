# mgmt commands (part 2)

### Commands

#### Set Discoverable

**Command Code:** `0x0006`  
**Controller Index:** `<controller id>`  
**Command Parameters:** `Discoverable` (1 Octet), `Timeout` (2 Octets)

**Return Parameters:**

| Name | Size |
| --- | --- |
| Current_Settings | 4 Octets |

This command sets the discoverable property of a controller. The allowed
`Discoverable` parameter values are:

| Value | Description |
| --- | --- |
| 0x00 | Not Discoverable |
| 0x01 | General Discoverable |
| 0x02 | Limited Discoverable (since 1.4) |

`Timeout` is in seconds and is only meaningful when `Discoverable` is set to
`0x01` or `0x02`. Providing a timeout with `0x00` returns Invalid Parameters.
For `0x02`, the timeout value is required.

This command is only available for BR/EDR capable controllers (not for
single-mode LE). It returns Not Supported otherwise.

This command can be used when the controller is not powered and all settings
will be programmed once powered.

Using a timeout when the controller is not powered returns Not Powered.

When switching discoverable on while connectable is off, it returns Rejected.

This command generates a Command Complete event on success or a Command Status
 event on failure.

Possible errors:

- <span style="color:#c00">Busy</span>
- <span style="color:#c00">Rejected</span>
- <span style="color:#c00">Not Supported</span>
- <span style="color:#c00">Invalid Parameters</span>
- <span style="color:#c00">Not Powered</span>
- <span style="color:#c00">Invalid Index</span>

#### Set Connectable

**Command Code:** `0x0007`  
**Controller Index:** `<controller id>`  
**Command Parameters:** `Connectable` (1 Octet)

**Return Parameters:**

| Name | Size |
| --- | --- |
| Current_Settings | 4 Octets |

This command sets the connectable property of a controller. Allowed values are
`0x00` and `0x01`. All other values return Invalid Parameters.

This command is available for BR/EDR, LE-only, and dual-mode controllers. For
BR/EDR it changes the page scan setting; for LE controllers it changes the
advertising type. For dual-mode controllers it affects both.

For LE capable controllers the connectable setting takes effect when
advertising is enabled (peripheral) or when directed advertising events are
received (central).

This command can be used when the controller is not powered and all settings
will be programmed once powered.

When switching connectable off, it also switches off discoverable. Switching
connectable back on does not restore a previous discoverable setting.

When switching connectable off, it expires a discoverable setting with a
 timeout.

This setting does not affect known devices from the Add Device command. These
 devices are always allowed to connect.

This command generates a Command Complete event on success or a Command Status
 event on failure.

Possible errors:

- <span style="color:#c00">Busy</span>
- <span style="color:#c00">Not Supported</span>
- <span style="color:#c00">Invalid Parameters</span>
- <span style="color:#c00">Invalid Index</span>

#### Set Fast Connectable

**Command Code:** `0x0008`  
**Controller Index:** `<controller id>`  
**Command Parameters:** `Enable` (1 Octet)

**Return Parameters:**

| Name | Size |
| --- | --- |
| Current_Settings | 4 Octets |

This command sets the controller into a connectable state where the page scan
parameters are configured to favor faster connect times at the expense of
higher power consumption.

Allowed values for `Enable` are `0x00` and `0x01`. All other values return
Invalid Parameters.

This command is only available for BR/EDR capable controllers (not for
single-mode LE). It returns Not Supported otherwise.

This command can be used when the controller is not powered and all settings
will be programmed once powered.

The setting is remembered during power down/up toggles.

This command generates a Command Complete event on success or a Command Status
 event on failure.

Possible errors:

- <span style="color:#c00">Failed</span>
- <span style="color:#c00">Busy</span>
- <span style="color:#c00">Not Supported</span>
- <span style="color:#c00">Invalid Parameters</span>
- <span style="color:#c00">Invalid Index</span>

#### Set Bondable

**Command Code:** `0x0009`  
**Controller Index:** `<controller id>`  
**Command Parameters:** `Bondable` (1 Octet)

**Return Parameters:**

| Name | Size |
| --- | --- |
| Current_Settings | 4 Octets |

This command sets the bondable property of a controller. Allowed values are
`0x00` and `0x01`. All other values return Invalid Parameters.

This command can be used when the controller is not powered and all settings
will be programmed once powered.

Turning bondable on does not automatically switch the controller into
connectable mode; that must be done separately.

The setting is remembered during power down/up toggles.

This command generates a Command Complete event on success or a Command Status
 event on failure.

Possible errors:

- <span style="color:#c00">Invalid Parameters</span>
- <span style="color:#c00">Invalid Index</span>

### Next

- [Commands (Part 3)](commands-3.md)
