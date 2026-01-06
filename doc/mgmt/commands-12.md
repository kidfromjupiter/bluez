# mgmt commands (part 12)

### Commands

#### Set Advertising (since 1.4)

**Command Code:** `0x0029`  
**Controller Index:** `<controller id>`  
**Command Parameters:** `Advertising` (1 Octet)

**Return Parameters:** `Current_Settings` (4 Octets)

This command enables LE advertising on a controller that supports it. Allowed
values for `Advertising`:

| Value | Description |
| --- | --- |
| 0x00 | Disable advertising |
| 0x01 | Enable advertising |
| 0x02 | Enable advertising in connectable mode (since 1.9) |

Using value `0x01` means that when connectable is disabled, advertising uses
undirected non-connectable packets and a non-resolvable random address. If
connectable is enabled, undirected connectable advertising packets and the
identity address or resolvable private address are used.

LE devices configured via Add Device with Action `0x01` have no effect when
using Advertising value `0x01` since only the connectable setting is used.

To use undirected connectable advertising without changing the connectable
setting, use value `0x02`. This makes the device connectable via LE without
requiring BR/EDR (and/or LE) connectable.

Value `0x02` is the preferred mode when implementing peripheral mode.

Using this command temporarily deactivates any configuration made by Add
Advertising. This command takes precedence. Once Set Advertising with value
`0x00` is issued, any previously made configurations via Add/Remove Advertising
(including changes made while Set Advertising was active) are re-enabled.

A prerequisite is that LE is already enabled; otherwise this command returns
Rejected.

This command generates a Command Complete event on success or a Command Status
 event on failure.

Possible errors:

- <span style="color:#c00">Busy</span>
- <span style="color:#c00">Rejected</span>
- <span style="color:#c00">Not Supported</span>
- <span style="color:#c00">Invalid Parameters</span>
- <span style="color:#c00">Invalid Index</span>

#### Set BR/EDR (since 1.4)

**Command Code:** `0x002A`  
**Controller Index:** `<controller id>`  
**Command Parameters:** `BR/EDR` (1 Octet)

**Return Parameters:** `Current_Settings` (4 Octets)

This command enables or disables BR/EDR support on a dual-mode controller.
Allowed values are `0x00` and `0x01`. All other values return Invalid
Parameters.

A prerequisite is that LE is already enabled; otherwise this command returns
Rejected. Enabling BR/EDR can be done when powered on or off. Disabling BR/EDR
can only be done when powered off (otherwise it returns Rejected). Disabling
BR/EDR automatically disables all other BR/EDR-related settings.

This command generates a Command Complete event on success or a Command Status
 event on failure.

Possible errors:

- <span style="color:#c00">Busy</span>
- <span style="color:#c00">Rejected</span>
- <span style="color:#c00">Not Supported</span>
- <span style="color:#c00">Invalid Parameters</span>
- <span style="color:#c00">Invalid Index</span>

#### Set Static Address (since 1.4)

**Command Code:** `0x002B`  
**Controller Index:** `<controller id>`  
**Command Parameters:** `Address` (6 Octets)

**Return Parameters:** `Current_Settings` (4 Octets)

This command sets the static random address. It is only supported on
controllers with LE support. The static random address is intended to be valid
for the lifetime of the controller or at least until the next power cycle. To
ensure this, setting the address is limited to when the controller is powered
off.

The special BDADDR_ANY address (`00:00:00:00:00:00`) can be used to disable the
static address.

When a controller has a public address (required for dual-mode controllers),
this address is not used. If a dual-mode controller is configured as LE-only
(BR/EDR switched off), the static address is used. When controller information
reports BDADDR_ANY, a static address must be configured first.

If privacy mode is enabled and the controller is single mode LE-only without a
public address, the static random address is used as the identity address.

The Static Address flag in current settings indicates whether the configured
static address is in use.

This command generates a Command Complete event on success or a Command Status
 event on failure.

Possible errors:

- <span style="color:#c00">Rejected</span>
- <span style="color:#c00">Not Supported</span>
- <span style="color:#c00">Invalid Parameters</span>
- <span style="color:#c00">Invalid Index</span>

### Next

- [Commands (Part 13)](commands-13.md)
