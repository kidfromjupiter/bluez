# mgmt commands (part 7)

### Commands

#### Set IO Capability

**Command Code:** `0x0018`  
**Controller Index:** `<controller id>`  
**Command Parameters:** `IO_Capability` (1 Octet)

**Return Parameters:** none

This command sets the IO Capability used for pairing. The command accepts both
SSP and SMP values.

Possible values for `IO_Capability`:

| Value | Description |
| --- | --- |
| 0x00 | DisplayOnly |
| 0x01 | DisplayYesNo |
| 0x02 | KeyboardOnly |
| 0x03 | NoInputNoOutput |
| 0x04 | KeyboardDisplay |

Passing `0x04` (KeyboardDisplay) causes the kernel to convert it to `0x01`
(DisplayYesNo) for BR/EDR connections (KeyboardDisplay is specific to SMP).

This command can be used when the controller is not powered.

This command generates a Command Complete event on success or a Command Status
 event on failure.

Possible errors:

- <span style="color:#c00">Invalid Parameters</span>
- <span style="color:#c00">Invalid Index</span>

#### Pair Device

**Command Code:** `0x0019`  
**Controller Index:** `<controller id>`  
**Command Parameters:** `Address` (6 Octets), `Address_Type` (1 Octet),
`IO_Capability` (1 Octet)

**Return Parameters:** `Address` (6 Octets), `Address_Type` (1 Octet)

This command triggers pairing with a remote device. The `IO_Capability`
parameter temporarily overrides the global IO Capability (set via Set IO
Capability) for this pairing event only.

Possible values for `Address_Type`:

| Value | Description |
| --- | --- |
| 0x00 | BR/EDR |
| 0x01 | LE Public |
| 0x02 | LE Random |

Possible values for `IO_Capability`:

| Value | Description |
| --- | --- |
| 0x00 | DisplayOnly |
| 0x01 | DisplayYesNo |
| 0x02 | KeyboardOnly |
| 0x03 | NoInputNoOutput |
| 0x04 | KeyboardDisplay |

Passing `0x04` (KeyboardDisplay) causes the kernel to convert it to `0x01`
(DisplayYesNo) for BR/EDR connections (KeyboardDisplay is specific to SMP).

The `Address` and `Address_Type` return the identity address if known. If a
resolvable random address is provided and an identity resolving key is
available, the return parameters provide the resolved address.

To track which resolvable random address maps to which identity address, the
New Identity Resolving Key event is sent before the Command Complete event.

This command can only be used when the controller is powered.

This command generates a Command Complete event on success or failure.

Reject status is used when the requested transport is not enabled. Not Supported
is used if the controller is not capable of the requested transport.

Possible errors:

- <span style="color:#c00">Rejected</span>
- <span style="color:#c00">Not Supported</span>
- <span style="color:#c00">Connect Failed</span>
- <span style="color:#c00">Busy</span>
- <span style="color:#c00">Invalid Parameters</span>
- <span style="color:#c00">Not Powered</span>
- <span style="color:#c00">Invalid Index</span>
- <span style="color:#c00">Already Paired</span>

#### Cancel Pair Device

**Command Code:** `0x001A`  
**Controller Index:** `<controller id>`  
**Command Parameters:** `Address` (6 Octets), `Address_Type` (1 Octet)

**Return Parameters:** `Address` (6 Octets), `Address_Type` (1 Octet)

The `Address` and `Address_Type` parameters should match what was given to a
preceding Pair Device command.

Possible values for `Address_Type`:

| Value | Description |
| --- | --- |
| 0x00 | BR/EDR |
| 0x01 | LE Public |
| 0x02 | LE Random |

This command can only be used when the controller is powered.

This command generates a Command Complete event on success or failure.

Possible errors:

- <span style="color:#c00">Invalid Parameters</span>
- <span style="color:#c00">Not Powered</span>
- <span style="color:#c00">Invalid Index</span>

#### Unpair Device

**Command Code:** `0x001B`  
**Controller Index:** `<controller id>`  
**Command Parameters:** `Address` (6 Octets), `Address_Type` (1 Octet),
`Disconnect` (1 Octet)

**Return Parameters:** `Address` (6 Octets), `Address_Type` (1 Octet)

Removes all keys associated with the remote device.

Possible values for `Address_Type`:

| Value | Description |
| --- | --- |
| 0x00 | BR/EDR |
| 0x01 | LE Public |
| 0x02 | LE Random |

The `Disconnect` parameter tells the kernel whether to forcefully disconnect any
existing connections to the device. In practice it should always be `1` except
for special GAP qualification test cases where key removal without disconnecting
is needed.

When unpairing, link key, long term key, and identity resolving key (if
provided) are purged. For devices using resolvable random addresses with a known
identity resolving key, the device will no longer be resolved afterward.

This command can only be used when the controller is powered.

This command generates a Command Complete event on success or failure.

Possible errors:

- <span style="color:#c00">Not Paired</span>
- <span style="color:#c00">Invalid Parameters</span>
- <span style="color:#c00">Not Powered</span>
- <span style="color:#c00">Invalid Index</span>

### Next

- [Commands (Part 8)](commands-8.md)
