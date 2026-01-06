# mgmt commands (part 9)

### Commands

#### Read Local Out Of Band Data

**Command Code:** `0x0020`  
**Controller Index:** `<controller id>`  
**Command Parameters:** none

**Return Parameters:** `Hash_192` (16 Octets), `Randomizer_192` (16 Octets),
`Hash_256` (16 Octets, optional), `Randomizer_256` (16 Octets, optional)

This command reads local Out of Band data.

This command can only be used when the controller is powered.

If Secure Connections support is enabled, this command returns P-192 hash and
randomizer values as well as P-256 versions of both.

Values returned by this command become invalid when the controller is powered
down. After each power cycle, call this command again to get updated values.

This command generates a Command Complete event on success or a Command Status
 event on failure.

Possible errors:

- <span style="color:#c00">Not Supported</span>
- <span style="color:#c00">Busy</span>
- <span style="color:#c00">Invalid Parameters</span>
- <span style="color:#c00">Not Powered</span>
- <span style="color:#c00">Invalid Index</span>

#### Add Remote Out Of Band Data

**Command Code:** `0x0021`  
**Controller Index:** `<controller id>`  
**Command Parameters:** `Address` (6 Octets), `Address_Type` (1 Octet),
`Hash_192` (16 Octets), `Randomizer_192` (16 Octets), `Hash_256` (16 Octets,
optional), `Randomizer_256` (16 Octets, optional)

**Return Parameters:** `Address` (6 Octets), `Address_Type` (1 Octet)

This command provides Out of Band data for a remote device.

Possible values for `Address_Type`:

| Value | Description |
| --- | --- |
| 0x00 | BR/EDR |
| 0x01 | LE Public |
| 0x02 | LE Random |

Provided Out Of Band data is persistent over power down/up toggles.

This command accepts optional P-256 versions of hash and randomizer. If they
are not provided, they are set to zero.

P-256 values can be provided even if Secure Connections is not enabled, but
in that case they are never used.

To provide only P-256 values, set the P-192 fields to zero. If Secure
Connections is disabled, this is the same as not providing any data at all.

When providing data for remote LE devices, the `Hash_192` and `Randomizer_192`
fields are not used and should be set to zero.

The `Hash_256` and `Randomizer_256` fields can be used for LE secure
connections Out Of Band data. If only LE secure connections data is provided,
`Hash_192` and `Randomizer_192` should be set to zero. There is no support for
providing the Security Manager TK value for LE legacy pairing.

If Secure Connections Only mode is enabled, providing `Hash_192` and
`Randomizer_192` is not allowed and they must be set to zero.

This command can be used when the controller is not powered and all settings
will be programmed once powered.

This command generates a Command Complete event on success or failure.

Possible errors:

- <span style="color:#c00">Failed</span>
- <span style="color:#c00">Invalid Parameters</span>
- <span style="color:#c00">Not Powered</span>
- <span style="color:#c00">Invalid Index</span>

#### Remove Remote Out Of Band Data

**Command Code:** `0x0022`  
**Controller Index:** `<controller id>`  
**Command Parameters:** `Address` (6 Octets), `Address_Type` (1 Octet)

**Return Parameters:** `Address` (6 Octets), `Address_Type` (1 Octet)

This command removes data added using Add Remote Out Of Band Data.

Possible values for `Address_Type`:

| Value | Description |
| --- | --- |
| 0x00 | BR/EDR |
| 0x01 | LE Public |
| 0x02 | LE Random |

When `Address` is `00:00:00:00:00:00`, all previously added data is removed.

This command can be used when the controller is not powered and all settings
will be programmed once powered.

This command generates a Command Complete event on success or failure.

Possible errors:

- <span style="color:#c00">Invalid Parameters</span>
- <span style="color:#c00">Not Powered</span>
- <span style="color:#c00">Invalid Index</span>

### Next

- [Commands (Part 10)](commands-10.md)
