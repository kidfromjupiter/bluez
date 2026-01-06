# mgmt commands (part 25)

### Commands

#### Remove Advertisement Monitor (since 1.18)

**Command Code:** `0x0053`  
**Controller Index:** `<controller id>`  
**Command Parameters:** `Monitor_Handle` (2 Octets)

**Return Parameters:** `Monitor_Handle` (2 Octets)

This command removes advertisement monitor(s). The kernel removes the monitor
with `Monitor_Handle` and updates LE scanning.

When `Monitor_Handle` is zero, all previously added handles are removed.

Removing a monitor while it is being added is ignored.

This command can be used when the controller is not powered and all settings
will be programmed once powered.

Possible errors:

- <span style="color:#c00">Failed</span>
- <span style="color:#c00">Busy</span>

#### Add Extended Advertising Parameters (since 1.19)

**Command Code:** `0x0054`  
**Controller Index:** `<controller id>`  
**Command Parameters:** `Instance` (1 Octet), `Flags` (4 Octets),
`Duration` (2 Octets), `Timeout` (2 Octets), `MinInterval` (4 Octets),
`MaxInterval` (4 Octets), `TxPower` (1 Octet)

**Return Parameters:** `Instance` (1 Octet), `TxPower` (1 Octet),
`MaxAdvDataLen` (1 Octet), `MaxScanRspLen` (1 Octet)

This command configures parameters for a Bluetooth Low Energy advertising
instance. It should be followed by Add Extended Advertising Data to complete
and enable the instance.

Advertising information added with this command is not visible immediately if
advertising is enabled via Set Advertising. Set Advertising takes precedence.
Instance information is stored and will be advertised once Set Advertising is
disabled.

The `Instance` identifier is a value between 1 and the number of supported
instances. The value 0 is reserved.

`Flags` control the advertising type:

| Bit | Description |
| --- | --- |
| 0 | Switch into Connectable mode |
| 1 | Advertise as Discoverable |
| 2 | Advertise as Limited Discoverable |
| 3 | Add Flags field to Adv_Data |
| 4 | Add TX Power field to Adv_Data |
| 5 | Add Appearance field to Scan_Rsp |
| 6 | Add Local Name in Scan_Rsp |
| 7 | Secondary Channel with LE 1M |
| 8 | Secondary Channel with LE 2M |
| 9 | Secondary Channel with LE Coded |
| 10 | Indicate tx power can be specified |
| 11 | Indicate HW supports the advertising offload |
| 12 | The Duration parameter should be used |
| 13 | The Timeout parameter should be used |
| 14 | The Interval parameters should be used |
| 15 | The Tx Power parameter should be used |
| 16 | The advertisement will contain a scan response |

When the connectable flag is set, the controller uses undirected connectable
advertising. The connectable global setting can be overwritten this way, useful
for making LE connectable without changing BR/EDR connectable. This is similar
 to mode `0x02` from Set Advertising.

When the connectable flag is not set, advertising follows the global
connectable setting. Non-connectable/scannable advertising uses a non-resolvable
random address; when the system is connectable, the identity address or
resolvable private address is used.

Using the connectable flag is useful for peripheral mode when BR/EDR (and/or LE)
 is controlled by Add Device. This allows making the peripheral connectable
without affecting the global connectable setting.

Secondary channel flags are mutually exclusive; setting multiple results in
Invalid Parameters. Choosing LE 1M or LE 2M uses extended advertising on the
primary channel with LE 1M and the selected PHY on the secondary channel.
Choosing LE Coded uses extended advertising on both primary and secondary
channels with LE Coded. Choosing none results in legacy advertising.

The flags also indicate which parameters are intentionally set, so the kernel
can provide defaults for unspecified fields.

`Duration` configures the length of an instance in seconds. The default is
2 seconds.

If only one advertising instance is added, `Duration` is ignored. With multiple
instances, a round-robin schedule is used.

`Timeout` configures the lifetime of an instance in seconds. `0` means no
expiration. With a timeout, the instance is removed automatically when the
timeout expires. Powering down invalidates all instances; adding a new instance
with a timeout is not possible when powered down.

When a timeout is provided, `Duration` subtracts from the actual timeout. For
example, Timeout 5 and Duration 2 schedules the instance three times (2 + 2 + 1
seconds). Other instances do not influence the timeout.

`MinInterval` and `MaxInterval` define the advertising interval in 0.625ms
slots. `MaxInterval` must be greater than or equal to `MinInterval`, and both
must be in range `0x000020` to `0xFFFFFF`, or registration fails.

`TxPower` is only used if the controller supports it, which is indicated by the
`CanSetTxPower` member of Read Advertising Features.

The acceptable range for requested Tx Power is [-127, +20] dBm. The controller
selects a power value up to the requested one and returns the value chosen. If
 the requested Tx Power is outside the valid range, registration fails.

When bit 16 is enabled, it indicates the subsequent Add Extended Advertising
Data will contain a scan response and the parameters should set a scannable PDU
type.

Re-adding an existing instance updates its configuration. In this case, no
Advertising Added event is generated. If the update fails, the instance is
removed and an Advertising Removed event is generated.

An instance added or changed while another instance is being advertised is not
visible immediately; it appears when scheduled by the round-robin algorithm.

Changes to an instance currently being advertised cancel that instance and
switch to the next. Changes are visible next time the instance is scheduled. If
only a single instance is active, changes are visible immediately.

`MaxAdvDataLen` indicates the maximum payload for Add Extended Advertising Data
after accounting for selected flags. `MaxScanRspLen` indicates the maximum scan
response length.

LE must already be enabled and the controller must be powered; otherwise this
command returns Rejected.

This command generates a Command Complete event on success or a Command Status
 event on failure.

Possible errors:

- <span style="color:#c00">Failed</span>
- <span style="color:#c00">Rejected</span>
- <span style="color:#c00">Not Supported</span>
- <span style="color:#c00">Invalid Parameters</span>
- <span style="color:#c00">Busy</span>

#### Add Extended Advertising Data (since 1.19)

**Command Code:** `0x0055`  
**Controller Index:** `<controller id>`  
**Command Parameters:** `Instance` (1 Octet), `Advertising Data Length`
(1 Octet), `Scan Response Length` (1 Octet),
`Advertising Data` (0-255 Octets), `Scan Response` (0-255 Octets)

**Return Parameters:** `Instance` (1 Octet)

This command updates advertising data for an existing advertising instance
created with Add Extended Advertising Parameters.

If extended advertising is available, the kernel sets advertising data, scan
response data, and enables the instance via HCI. If extended advertising is not
available, the kernel updates the data and schedules the instance either
immediately or as part of the software round-robin queue.

If `Scan_Rsp_Len` is zero and the connectable flag is not set and global
connectable is off, non-connectable advertising is used. If `Scan_Rsp_Len` is
larger than zero and connectable flag is not set and global advertising is off,
scannable advertising is used. This supports lower air traffic for broadcaster
role devices.

If the `Instance` does not match a known instance, or the advertising data or
scan response data are unrecognized, Invalid Parameters is returned.

If a Set LE or Advertising command is still in progress, Busy is returned.

If the controller is not powered, Rejected is returned.

This command generates a Command Complete event on success or a Command Status
 event on failure.

Possible errors:

- <span style="color:#c00">Failed</span>
- <span style="color:#c00">Rejected</span>
- <span style="color:#c00">Invalid Parameters</span>
- <span style="color:#c00">Busy</span>

### Navigation

- [Previous: Commands (Part 24)](commands-24.md)
- [Next: Commands (Part 26)](commands-26.md)
