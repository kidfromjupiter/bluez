# mgmt commands (part 16)

### Commands

#### Read Unconfigured Controller Index List (since 1.7)

**Command Code:** `0x0036`  
**Controller Index:** `<non-controller>`  
**Command Parameters:** none

**Return Parameters:** `Num_Controllers` (2 Octets), `Controller_Index[i]`
(2 Octets)

This command returns the list of currently unconfigured controllers.
Unconfigured controllers added after calling this command can be monitored using
Unconfigured Index Added events.

An unconfigured controller can either move to a configured state (Unconfigured
Index Removed event followed by Index Added), or be removed from the system
(Unconfigured Index Removed).

Only controllers that require configuration are listed. A fully configured
controller is not listed even if it supports configuration changes.

This command generates a Command Complete event on success or a Command Status
 event on failure.

#### Read Controller Configuration Information (since 1.7)

**Command Code:** `0x0037`  
**Controller Index:** `<controller id>`  
**Command Parameters:** none

**Return Parameters:** `Manufacturer` (2 Octets), `Supported_Options` (4 Octets),
`Missing_Options` (4 Octets)

This command returns supported configuration options and missing configuration
options.

The missing options must be configured before the controller is fully
configured and ready for standard operation. This command is typically used
right after Read Unconfigured Controller Index List or an Unconfigured Index
Added event.

`Supported_Options` and `Missing_Options` are bitmasks with currently available
bits:

| Bit | Description |
| --- | --- |
| 0 | External configuration |
| 1 | Bluetooth public address configuration |

It is valid to call this command on controllers that do not require
configuration. A fully configured controller may still support configuration
changes.

If all configurations are completed and `Missing_Options` becomes empty, the
controller is announced via Index Added.

This command generates a Command Complete event on success or a Command Status
 event on failure.

Possible errors:

- <span style="color:#c00">Invalid Parameters</span>
- <span style="color:#c00">Invalid Index</span>

#### Set External Configuration (since 1.7)

**Command Code:** `0x0038`  
**Controller Index:** `<controller id>`  
**Command Parameters:** `Configuration` (1 Octet)

**Return Parameters:** `Missing_Options` (4 Octets)

This command changes the external configuration option to mark a controller as
configured or unconfigured.

- `0x00`: Set unconfigured state.
- `0x01`: Set configured state.

It is not mandatory that this configuration option is provided by a controller.
If provided, configuration must happen externally (user channel or vendor
specific methods).

When `Missing_Options` returns zero, the controller switches to configured state
and is announced via Index Added.

Wrongly configured controllers might still cause errors when powering on via
Set Powered.

This command generates a Command Complete event on success or a Command Status
 event on failure.

Possible errors:

- <span style="color:#c00">Rejected</span>
- <span style="color:#c00">Not Supported</span>
- <span style="color:#c00">Invalid Parameters</span>
- <span style="color:#c00">Invalid Index</span>

### Next

- [Commands (Part 17)](commands-17.md)
