# mgmt commands (part 6)

### Commands

#### Get Connections

**Command Code:** `0x0015`  
**Controller Index:** `<controller id>`  
**Command Parameters:** none

**Return Parameters:** `Connection_Count` (2 Octets), `Address[]` (6 Octets),
`Address_Type[]` (1 Octet), `...[]`

This command retrieves a list of currently connected devices.

Possible values for `Address_Type`:

| Value | Description |
| --- | --- |
| 0x00 | BR/EDR |
| 0x01 | LE Public |
| 0x02 | LE Random |

For devices using resolvable random addresses with a known identity resolving
key, `Address` and `Address_Type` contain the identity information.

This command can only be used when the controller is powered.

This command generates a Command Complete event on success or a Command Status
 event on failure.

Possible errors:

- <span style="color:#c00">Invalid Parameters</span>
- <span style="color:#c00">Not Powered</span>
- <span style="color:#c00">Invalid Index</span>

#### PIN Code Reply

**Command Code:** `0x0016`  
**Controller Index:** `<controller id>`  
**Command Parameters:** `Address` (6 Octets), `Address_Type` (1 Octet),
`PIN_Length` (1 Octet), `PIN_Code` (16 Octets)

**Return Parameters:** `Address` (6 Octets), `Address_Type` (1 Octet)

This command responds to a PIN Code Request event.

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
- <span style="color:#c00">Invalid Parameters</span>
- <span style="color:#c00">Not Powered</span>
- <span style="color:#c00">Invalid Index</span>

### Next

- [Commands (Part 7)](commands-7.md)

#### PIN Code Negative Reply

**Command Code:** `0x0017`  
**Controller Index:** `<controller id>`  
**Command Parameters:** `Address` (6 Octets), `Address_Type` (1 Octet)

**Return Parameters:** `Address` (6 Octets), `Address_Type` (1 Octet)

This command returns a negative response to a PIN Code Request event.

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
- <span style="color:#c00">Invalid Parameters</span>
- <span style="color:#c00">Not Powered</span>
- <span style="color:#c00">Invalid Index</span>
