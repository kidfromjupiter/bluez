# mgmt commands (part 14)

### Commands

#### Set Debug Keys (since 1.5)

**Command Code:** `0x002E`  
**Controller Index:** `<controller id>`  
**Command Parameters:** `Debug_Keys` (1 Octet)

**Return Parameters:** `Current_Settings` (4 Octets)

This command tells the kernel whether to accept debug keys. Allowed values:

| Value | Description |
| --- | --- |
| 0x00 | Discard keys on disconnect |
| 0x01 | Discard keys on reboot |
| 0x02 | Discard keys on reboot (SSP debug mode, since 1.7) |

With `0x00`, any generated debug key is discarded when the connection ends.

With `0x01`, generated debug keys are kept and can be used for future
connections. Debug keys are always marked non-persistent and should not be
stored; a reboot or switching back to `0x00` deletes them.

With `0x02`, generated debug keys are kept as with `0x01`, and the controller is
put into a mode that generates debug keys for each new pairing. Switching back
to `0x01` or `0x00` disables that mode.

This command generates a Command Complete event on success or a Command Status
 event on failure.

Possible errors:

- <span style="color:#c00">Busy</span>
- <span style="color:#c00">Not Supported</span>
- <span style="color:#c00">Invalid Parameters</span>
- <span style="color:#c00">Invalid Index</span>

#### Set Privacy (since 1.5)

**Command Code:** `0x002F`  
**Controller Index:** `<controller id>`  
**Command Parameters:** `Privacy` (1 Octet), `Identity_Resolving_Key` (16 Octets)

**Return Parameters:** `Current_Settings` (4 Octets)

Enables Low Energy Privacy using resolvable private addresses.

`Privacy` values:

- `0x00`: Disable privacy mode.
- `0x01`: Always use privacy mode (resolvable private address when discoverable
  and when pairing is initiated).
- `0x02`: Limited privacy mode; use resolvable private address except when the
  controller is bondable and discoverable, in which case use identity address.

Exposing the identity address when bondable/discoverable or during initiated
pairing can be a privacy issue. For dual-mode controllers this is less critical
because the public address is exposed over BR/EDR anyway. Exposing identity
address for pairing helps match dual-mode topology during discovery.

If `0x02` is used, the GATT database should expose the Privacy Characteristic so
remote devices can detect privacy usage.

When a controller has a public address (mandatory for dual-mode), it is used as
identity. For single-mode LE without public address, configure a static random
address first. Privacy mode can only be enabled when an identity address is
available.

`Identity_Resolving_Key` is the local key assigned for the local resolvable
private address.

Possible errors:

- <span style="color:#c00">Busy</span>
- <span style="color:#c00">Not Supported</span>
- <span style="color:#c00">Invalid Parameters</span>
- <span style="color:#c00">Invalid Index</span>

#### Load Identity Resolving Keys (since 1.5)

**Command Code:** `0x0030`  
**Controller Index:** `<controller id>`  
**Command Parameters:** `Key_Count` (2 Octets), `Address[]` (6 Octets),
`Address_Type[]` (1 Octet), `Value[]` (16 Octets), `...[]`

**Return Parameters:** none

This command feeds the kernel with currently known identity resolving keys. It
does not need to be called again upon receipt of New Identity Resolving Key
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

This command can be used when the controller is not powered.

This command generates a Command Complete event on success or a Command Status
 event on failure.

Possible errors:

- <span style="color:#c00">Invalid Parameters</span>
- <span style="color:#c00">Invalid Index</span>

#### Get Connection Information (since 1.6)

**Command Code:** `0x0031`  
**Controller Index:** `<controller id>`  
**Command Parameters:** `Address` (6 Octets), `Address_Type` (1 Octet)

**Return Parameters:** `Address` (6 Octets), `Address_Type` (1 Octet), `RSSI`
(1 Octet), `TX_Power` (1 Octet), `Max_TX_Power` (1 Octet)

This command returns connection information.

Possible values for `Address_Type`:

| Value | Description |
| --- | --- |
| 0x00 | Reserved (not in use) |
| 0x01 | LE Public |
| 0x02 | LE Random |

`TX_Power` and `Max_TX_Power` can be set to `127` if unknown. `RSSI` set to
`127` indicates it is not available.

This command generates a Command Complete event on success and on failure. On
failure, only `Address` and `Address_Type` are valid; remaining parameters are
invalid and must be ignored.

Possible errors:

- <span style="color:#c00">Not Connected</span>
- <span style="color:#c00">Not Powered</span>
- <span style="color:#c00">Invalid Parameters</span>
- <span style="color:#c00">Invalid Index</span>

### Next

- [Commands (Part 15)](commands-15.md)

#### Get Clock Information (since 1.7)

**Command Code:** `0x0032`  
**Controller Index:** `<controller id>`  
**Command Parameters:** `Address` (6 Octets), `Address_Type` (1 Octet)

**Return Parameters:** `Address` (6 Octets), `Address_Type` (1 Octet),
`Local_Clock` (4 Octets), `Piconet_Clock` (4 Octets), `Accuracy` (2 Octets)

This command returns local and piconet clock information.

Possible values for `Address_Type`:

| Value | Description |
| --- | --- |
| 0x00 | BR/EDR |
| 0x01 | Reserved (not in use) |
| 0x02 | Reserved (not in use) |

`Accuracy` can be set to `0xffff` to indicate unknown.

If `Address` is `00:00:00:00:00:00`, only `Local_Clock` is valid. `Piconet_Clock`
and `Accuracy` are invalid and should be ignored.

This command generates a Command Complete event on success and on failure. On
failure, only `Address` and `Address_Type` are valid; remaining parameters are
invalid and must be ignored.

Possible errors:

- <span style="color:#c00">Not Connected</span>
- <span style="color:#c00">Not Powered</span>
- <span style="color:#c00">Invalid Parameters</span>
- <span style="color:#c00">Invalid Index</span>
