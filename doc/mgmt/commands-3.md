# mgmt commands (part 3)

### Commands

#### Set Link Security

**Command Code:** `0x000A`  
**Controller Index:** `<controller id>`  
**Command Parameters:** `Link_Security` (1 Octet)

**Return Parameters:**

| Name | Size |
| --- | --- |
| Current_Settings | 4 Octets |

This command enables or disables link level security for a controller (Security
Mode 3). Allowed values for `Link_Security` are `0x00` and `0x01`. All other
values return Invalid Parameters.

This command is only available for BR/EDR capable controllers (not for
single-mode LE). It returns Not Supported otherwise.

This command can be used when the controller is not powered and all settings
will be programmed once powered.

This command generates a Command Complete event on success or a Command Status
 event on failure.

Possible errors:

- <span style="color:#c00">Busy</span>
- <span style="color:#c00">Not Supported</span>
- <span style="color:#c00">Invalid Parameters</span>
- <span style="color:#c00">Invalid Index</span>

### Next

- [Commands (Part 4)](commands-4.md)

#### Set Secure Simple Pairing

**Command Code:** `0x000B`  
**Controller Index:** `<controller id>`  
**Command Parameters:** `Secure_Simple_Pairing` (1 Octet)

**Return Parameters:**

| Name | Size |
| --- | --- |
| Current_Settings | 4 Octets |

This command enables or disables Secure Simple Pairing support. Allowed values
for `Secure_Simple_Pairing` are `0x00` and `0x01`. All other values return
Invalid Parameters.

This command is only available for BR/EDR capable controllers supporting core
spec 2.1 or greater (not for single-mode LE or pre-2.1 controllers).

This command can be used when the controller is not powered and all settings
will be programmed once powered.

If the controller does not support Secure Simple Pairing, the command fails
with Not Supported.

This command generates a Command Complete event on success or a Command Status
 event on failure.

Possible errors:

- <span style="color:#c00">Busy</span>
- <span style="color:#c00">Not Supported</span>
- <span style="color:#c00">Invalid Parameters</span>
- <span style="color:#c00">Invalid Index</span>

#### Set High Speed

**Command Code:** `0x000C`  
**Controller Index:** `<controller id>`  
**Command Parameters:** `High_Speed` (1 Octet)

**Return Parameters:**

| Name | Size |
| --- | --- |
| Current_Settings | 4 Octets |

This command enables or disables Bluetooth High Speed support. Allowed values
for `High_Speed` are `0x00` and `0x01`. All other values return Invalid
Parameters.

This command is only available for BR/EDR capable controllers (not for
single-mode LE).

This command can be used when the controller is not powered and all settings
will be programmed once powered.

High Speed requires Secure Simple Pairing to be enabled first. High Speed is
not possible for connections without Secure Simple Pairing.

When switching Secure Simple Pairing off, High Speed is switched off as well.
Switching Secure Simple Pairing back on does not re-enable High Speed; it must
be enabled manually.

This command generates a Command Complete event on success or a Command Status
 event on failure.

Possible errors:

- <span style="color:#c00">Not Supported</span>
- <span style="color:#c00">Invalid Parameters</span>
- <span style="color:#c00">Invalid Index</span>

#### Set Low Energy

**Command Code:** `0x000D`  
**Controller Index:** `<controller id>`  
**Command Parameters:** `Low_Energy` (1 Octet)

**Return Parameters:**

| Name | Size |
| --- | --- |
| Current_Settings | 4 Octets |

This command enables or disables Low Energy support. Allowed values for
`Low_Energy` are `0x00` and `0x01`. All other values return Invalid Parameters.

This command is only available for LE-capable controllers and returns Not
Supported otherwise.

This command can be used when the controller is not powered and all settings
will be programmed once powered.

If the kernel subsystem does not support LE or the controller does not either,
the command fails.

Disabling LE permanently disables and removes all advertising instances
configured with Add Advertising. Advertising Removed events are issued
accordingly.

This command generates a Command Complete event on success or a Command Status
 event on failure.

Possible errors:

- <span style="color:#c00">Busy</span>
- <span style="color:#c00">Not Supported</span>
- <span style="color:#c00">Invalid Parameters</span>
- <span style="color:#c00">Invalid Index</span>

#### Set Device Class

**Command Code:** `0x000E`  
**Controller Index:** `<controller id>`  
**Command Parameters:** `Major_Class` (1 Octet), `Minor_Class` (1 Octet)

**Return Parameters:**

| Name | Size |
| --- | --- |
| Class_Of_Device | 3 Octets |

This command sets the major and minor device class for BR/EDR capable
controllers.

This command also implicitly disables caching of pending CoD and EIR updates.

This command is only available for BR/EDR capable controllers (not for
single-mode LE).

This command can be used when the controller is not powered and all settings
will be programmed once powered.

If the controller is powered off, `0x000000` is returned for the class of
device. After power on, the new value is announced via a class of device
changed event.

This command generates a Command Complete event on success or a Command Status
 event on failure.

Possible errors:

- <span style="color:#c00">Busy</span>
- <span style="color:#c00">Not Supported</span>
- <span style="color:#c00">Invalid Parameters</span>
- <span style="color:#c00">Invalid Index</span>
