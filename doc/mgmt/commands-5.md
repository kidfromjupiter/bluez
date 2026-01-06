# mgmt commands (part 5)

### Commands

#### Load Link Keys

**Command Code:** `0x0012`  
**Controller Index:** `<controller id>`  
**Command Parameters:** `Debug_Keys` (1 Octet), `Key_Count` (2 Octets), `Address[]` (6 Octets), `Address_Type[]` (1 Octet), `Key_Type[]` (1 Octet), `Value[]` (16 Octets), `PIN_Length[]` (1 Octet), `...[]`

**Return Parameters:** none

This command feeds the kernel with currently known link keys. The command does
not need to be called again upon receipt of New Link Key events since the kernel
updates its list automatically.

The `Debug_Keys` parameter indicates whether debug keys are accepted. Allowed
values are `0x00` and `0x01`. All other values return Invalid Parameters.

Usage of `Debug_Keys` is deprecated and replaced with Set Debug Keys. Setting
`Debug_Keys` via Load Link Keys has the same effect as Set Debug Keys and
applies to all keys in the system.

Possible values for `Address_Type`:

| Value | Description |
| --- | --- |
| 0 | BR/EDR |
| 1 | Reserved (not in use) |
| 2 | Reserved (not in use) |

Public and random LE addresses are not valid and will be rejected.

Currently defined `Key_Type` values:

| Value | Description |
| --- | --- |
| 0x00 | Combination key |
| 0x01 | Local Unit key |
| 0x02 | Remote Unit key |
| 0x03 | Debug Combination key |
| 0x04 | Unauthenticated Combination key from P-192 |
| 0x05 | Authenticated Combination key from P-192 |
| 0x06 | Changed Combination key |
| 0x07 | Unauthenticated Combination key from P-256 |
| 0x08 | Authenticated Combination key from P-256 |

This command can be used when the controller is not powered.

This command generates a Command Complete event on success or a Command Status
 event on failure.

Possible errors:

- <span style="color:#c00">Invalid Parameters</span>
- <span style="color:#c00">Invalid Index</span>

#### Load Long Term Keys

**Command Code:** `0x0013`  
**Controller Index:** `<controller id>`  
**Command Parameters:** `Key_Count` (2 Octets), `Address[]` (6 Octets), `Address_Type[]` (1 Octet), `Key_Type[]` (1 Octet), `Central[]` (1 Octet), `Encryption_Size[]` (1 Octet), `Encryption_Diversifier[]` (2 Octets), `Random_Number[]` (8 Octets), `Value[]` (16 Octets), `...[]`

**Return Parameters:** none

This command feeds the kernel with currently known (SMP) Long Term Keys. The
command does not need to be called again upon receipt of New Long Term Key
events since the kernel updates its list automatically.

Possible values for `Address_Type`:

| Value | Description |
| --- | --- |
| 0x00 | Reserved (not in use) |
| 0x01 | LE Public |
| 0x02 | LE Random |

The provided `Address` and `Address_Type` are the identity of a device: either
its public address or static random address.

Unresolvable and resolvable random addresses are not valid and will be rejected.

Currently defined `Key_Type` values:

| Value | Description |
| --- | --- |
| 0x00 | Unauthenticated key |
| 0x01 | Authenticated key |

This command can be used when the controller is not powered.

This command generates a Command Complete event on success or a Command Status
 event on failure.

Possible errors:

- <span style="color:#c00">Invalid Parameters</span>
- <span style="color:#c00">Invalid Index</span>

#### Disconnect

**Command Code:** `0x0014`  
**Controller Index:** `<controller id>`  
**Command Parameters:** `Address` (6 Octets), `Address_Type` (1 Octet)

**Return Parameters:** `Address` (6 Octets), `Address_Type` (1 Octet)

This command forces disconnection of a currently connected device.

Possible values for `Address_Type`:

| Value | Description |
| --- | --- |
| 0x00 | BR/EDR |
| 0x01 | LE Public |
| 0x02 | LE Random |

This command can only be used when the controller is powered.

This command generates a Command Complete event on success or failure.

Possible errors:

- <span style="color:#c00">Not Connected</span>
- <span style="color:#c00">Busy</span>
- <span style="color:#c00">Invalid Parameters</span>
- <span style="color:#c00">Not Powered</span>
- <span style="color:#c00">Invalid Index</span>

### Next

- [Commands (Part 6)](commands-6.md)
