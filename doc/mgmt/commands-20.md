# mgmt commands (part 20)

### Commands

#### Remove Advertising (since 1.9)

**Command Code:** `0x003F`  
**Controller Index:** `<controller id>`  
**Command Parameters:** `Instance` (1 Octet)

**Return Parameters:** `Instance` (1 Octet)

This command removes a previously added advertising instance.

When `Instance` is zero, all previously added instances are removed.

Removing advertising with this command is not visible while advertising is
enabled via Set Advertising. Set Advertising takes precedence. Instance changes
are stored and applied after Set Advertising is disabled.

Removing an instance while it is being advertised cancels that instance
immediately, even if it has not yet reached its configured Timeout or Duration.

This command can be used when the controller is not powered and all settings
will be programmed once powered.

This command generates a Command Complete event on success or a Command Status
 event on failure.

Possible errors:

- <span style="color:#c00">Invalid Parameters</span>
- <span style="color:#c00">Invalid Index</span>

#### Get Advertising Size Information (since 1.11)

**Command Code:** `0x0040`  
**Controller Index:** `<controller id>`  
**Command Parameters:** `Instance` (1 Octet), `Flags` (4 Octets)

**Return Parameters:** `Instance` (1 Octet), `Flags` (4 Octets),
`Max_Adv_Data_Len` (1 Octet), `Max_Scan_Rsp_Len` (1 Octet)

Read Advertising Features returns the overall maximum sizes for advertising and
scan response data. That size applies when no `Flags` are used. For certain
flags, the size can decrease. This command provides detailed size information
for a specific `Flags` value.

`Flags` values are defined as:

| Value | Description |
| --- | --- |
| 0 | Switch into Connectable mode |
| 1 | Advertise as Discoverable |
| 2 | Advertise as Limited Discoverable |
| 3 | Add Flags field to Adv_Data |
| 4 | Add TX Power field to Adv_Data |
| 5 | Add Appearance field to Scan_Rsp |
| 6 | Add Local Name in Scan_Rsp |

To get accurate size information, use the same `Flags` values with the Add
Advertising command.

`Max_Adv_Data_Len` and `Max_Scan_Rsp_Len` report the maximum data field lengths
for the given `Flags` value. When `Flags` is zero, these match Read Advertising
Features.

Possible errors:

- <span style="color:#c00">Invalid Parameters</span>
- <span style="color:#c00">Invalid Index</span>

#### Start Limited Discovery (since 1.11)

**Command Code:** `0x0041`  
**Controller Index:** `<controller id>`  
**Command Parameters:** `Address_Type` (1 Octet)

**Return Parameters:** `Address_Type` (1 Octet)

This command starts the limited discovery procedure. A Device Found event is
sent for each discovered device.

`Address_Type` is a bitwise OR of the following bits:

| Bit | Description |
| --- | --- |
| 0 | BR/EDR |
| 1 | LE Public |
| 2 | LE Random |

Examples:

| Value | Description |
| --- | --- |
| `0x01` | BR/EDR |
| `0x06` | LE (public and random) |
| `0x07` | BR/EDR/LE (interleaved discovery) |

Limited discovery uses active scanning for LE and searches for devices with the
limited discoverability flag. On BR/EDR it uses LIAC and filters on the limited
discoverability flag of the class of device.

When the procedure starts, the Discovery event indicates the start similarly to
Start Discovery.

This command can only be used when the controller is powered.

This command generates a Command Complete event on success or failure.

Possible errors:

- <span style="color:#c00">Busy</span>
- <span style="color:#c00">Not Supported</span>
- <span style="color:#c00">Invalid Parameters</span>
- <span style="color:#c00">Not Powered</span>
- <span style="color:#c00">Invalid Index</span>

#### Read Extended Controller Information (since 1.14)

**Command Code:** `0x0042`  
**Controller Index:** `<controller id>`  
**Command Parameters:** none

**Return Parameters:** `Address` (6 Octets), `Bluetooth_Version` (1 Octet),
`Manufacturer` (2 Octets), `Supported_Settings` (4 Octets),
`Current_Settings` (4 Octets), `EIR_Data_Length` (2 Octets),
`EIR_Data` (0-65535 Octets)

This command retrieves the current state and basic information of a controller.
It is typically used right after Read Controller Index List or an Index Added
(or extended) event.

`Address` describes the controller public address. For LE-only controllers it
can be `00:00:00:00:00:00`. In that case, a static address must be configured
with Set Static Address before powering on.

If a public address is set, it is used as the controller identity address. If
no public address is available, the configured static address is used instead.

For dual-mode controllers configured as LE-only, the static address is used
when set; otherwise the public address is used.

`Current_Settings` and `Supported_Settings` bitmask values:

| Bit | Description |
| --- | --- |
| 0 | Powered |
| 1 | Connectable |
| 2 | Fast Connectable |
| 3 | Discoverable |
| 4 | Bondable |
| 5 | Link Level Security (Sec. mode 3) |
| 6 | Secure Simple Pairing |
| 7 | Basic Rate/Enhanced Data Rate |
| 8 | High Speed |
| 9 | Low Energy |
| 10 | Advertising |
| 11 | Secure Connections |
| 12 | Debug Keys |
| 13 | Privacy |
| 14 | Controller Configuration |
| 15 | Static Address |
| 16 | PHY Configuration |
| 17 | Wideband Speech |
| 18 | Connected Isochronous Stream - Central |
| 19 | Connected Isochronous Stream - Peripheral |

`EIR_Data` includes class of device, local name, and other values. Not all
fields are always present (for example LE-only devices do not include class of
 device information).

When `EIR_Data` changes, the Extended Controller Information Changed event
notifies clients of the updated data.

This command generates a Command Complete event on success or a Command Status
 event on failure.

Possible errors:

- <span style="color:#c00">Invalid Parameters</span>
- <span style="color:#c00">Invalid Index</span>

#### Set Appearance (since 1.14)

**Command Code:** `0x0043`  
**Controller Index:** `<controller id>`  
**Command Parameters:** `Appearance` (2 Octets)

**Return Parameters:** none

This command sets the controller appearance value.

This command can be used when the controller is not powered and all settings
will be programmed once powered.

Appearance is persisted across power cycles and only needs to be set once per
controller until removed.

This command generates a Command Complete event on success or a Command Status
 event on failure.

This command is only available for LE capable controllers. It returns Not
Supported otherwise.

Possible errors:

- <span style="color:#c00">Not Supported</span>
- <span style="color:#c00">Invalid Parameters</span>
- <span style="color:#c00">Invalid Index</span>

### Navigation

- [Previous: Commands (Part 19)](commands-19.md)
- [Next: Commands (Part 21)](commands-21.md)
