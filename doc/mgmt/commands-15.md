# mgmt commands (part 15)

### Commands

#### Add Device (since 1.7)

**Command Code:** `0x0033`  
**Controller Index:** `<controller id>`  
**Command Parameters:** `Address` (6 Octets), `Address_Type` (1 Octet),
`Action` (1 Octet)

**Return Parameters:** `Address` (6 Octets), `Address_Type` (1 Octet)

This command adds a device to the action list. The action list allows scanning
for devices and enables incoming connections from known devices.

Possible values for `Address_Type`:

| Value | Description |
| --- | --- |
| 0x00 | BR/EDR |
| 0x01 | LE Public |
| 0x02 | LE Random |

Possible values for `Action`:

| Value | Description |
| --- | --- |
| 0x00 | Background scan for device |
| 0x01 | Allow incoming connection |
| 0x02 | Auto-connect remote device |

With `Action` 0x00, when the device is found, a Device Found event is sent.
This action is only valid for LE Public and LE Random address types.

With `Action` 0x01, the device is allowed to connect. For BR/EDR this means an
incoming connection. For LE Public and LE Random, a connection is established
for directed advertising; on success a Device Connected event is sent.

With `Action` 0x02, when the device is found it will be connected and a Device
Connected event is sent on success. This action is only valid for LE Public and
LE Random address types.

When a device is blocked using Block Device, it can still be added here, but
all actions are ignored until the device is unblocked.

Devices added with Action 0x01 are allowed to connect even if connectable is
off. This acts as a list of known trusted devices.

This command can be used when the controller is not powered and all settings
will be programmed once powered.

This command generates a Command Complete event on success or failure.

Possible errors:

- <span style="color:#c00">Failed</span>
- <span style="color:#c00">Invalid Parameters</span>
- <span style="color:#c00">Invalid Index</span>

#### Remove Device (since 1.7)

**Command Code:** `0x0034`  
**Controller Index:** `<controller id>`  
**Command Parameters:** `Address` (6 Octets), `Address_Type` (1 Octet)

**Return Parameters:** `Address` (6 Octets), `Address_Type` (1 Octet)

This command removes a device from the action list added via Add Device.

Possible values for `Address_Type`:

| Value | Description |
| --- | --- |
| 0x00 | BR/EDR |
| 0x01 | LE Public |
| 0x02 | LE Random |

When `Address` is `00:00:00:00:00:00`, all previously added devices are removed.

This command can be used when the controller is not powered and all settings
will be programmed once powered.

This command generates a Command Complete event on success or failure.

Possible errors:

- <span style="color:#c00">Invalid Parameters</span>
- <span style="color:#c00">Invalid Index</span>

#### Load Connection Parameters (since 1.7)

**Command Code:** `0x0035`  
**Controller Index:** `<controller id>`  
**Command Parameters:** `Param_Count` (2 Octets), `Address[]` (6 Octets),
`Address_Type[]` (1 Octet), `Min_Connection_Interval[]` (2 Octets),
`Max_Connection_Interval[]` (2 Octets), `Connection_Latency[]` (2 Octets),
`Supervision_Timeout[]` (2 Octets), `...[]`

**Return Parameters:** none

This command loads connection parameters for multiple devices into the kernel.
Currently this is only supported on controllers with Low Energy support.

Possible values for `Address_Type`:

| Value | Description |
| --- | --- |
| 0x00 | BR/EDR |
| 0x01 | LE Public |
| 0x02 | LE Random |

The provided `Address` and `Address_Type` are the identity of a device: either
its public address or static random address.

`Min_Connection_Interval`, `Max_Connection_Interval`, `Connection_Latency`, and
`Supervision_Timeout` should be configured as described in Core 4.1 spec, Vol 2,
7.8.12.

This command can be used when the controller is not powered.

This command generates a Command Complete event on success or a Command Status
 event on failure.

Possible errors:

- <span style="color:#c00">Invalid Parameters</span>
- <span style="color:#c00">Invalid Index</span>
- <span style="color:#c00">Not Supported</span>

### Next

- [Commands (Part 16)](commands-16.md)
