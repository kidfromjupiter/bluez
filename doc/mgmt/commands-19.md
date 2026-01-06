# mgmt commands (part 19)

### Commands

#### Add Advertising (since 1.9)

**Command Code:** `0x003E`  
**Controller Index:** `<controller id>`  
**Command Parameters:** `Instance` (1 Octet), `Flags` (4 Octets), `Duration`
(2 Octets), `Timeout` (2 Octets), `Adv_Data_Len` (1 Octet), `Scan_Rsp_Len`
(1 Octet), `Adv_Data` (0-255 Octets), `Scan_Rsp` (0-255 Octets)

**Return Parameters:** `Instance` (1 Octet)

This command configures an advertising instance that can be used to switch a
Bluetooth Low Energy controller into advertising mode.

Advertising information added with this command is not visible immediately if
advertising is enabled via Set Advertising. Set Advertising takes precedence.
Instance information is stored and will be advertised once Set Advertising is
disabled.

The Instance identifier is a value between 1 and the number of supported
instances. The value 0 is reserved.

`Flags` control advertising type. Defined flags:

| Bit | Description |
| --- | --- |
| 0 | Switch into Connectable mode |
| 1 | Advertise as Discoverable |
| 2 | Advertise as Limited Discoverable |
| 3 | Add Flags field to Adv_Data |
| 4 | Add TX Power field to Adv_Data |
| 5 | Add Appearance field to Scan_Rsp (since 1.14) |
| 6 | Add Local Name in Scan_Rsp (since 1.14) |
| 7 | Secondary Channel with LE 1M (since 1.15) |
| 8 | Secondary Channel with LE 2M (since 1.15) |
| 9 | Secondary Channel with LE Coded (since 1.15) |

When the connectable flag is set, the controller uses undirected connectable
advertising. The connectable global setting can be overridden this way, useful
for making LE connectable without changing BR/EDR connectable. This is similar
 to mode `0x02` from Set Advertising.

When the connectable flag is not set, advertising follows the global connectable
setting. Non-connectable/scannable advertising uses a non-resolvable random
address; when the system is connectable, the identity address or resolvable
private address is used.

Using the connectable flag is useful for peripheral mode when BR/EDR (and/or LE)
 is controlled by Add Device. This allows making the peripheral connectable
without affecting the global connectable setting.

If `Scan_Rsp_Len` is zero, connectable flag is not set, and global connectable
is off, non-connectable advertising is used. If `Scan_Rsp_Len` is larger than
zero under the same conditions, scannable advertising is used. This supports
lower air traffic for broadcaster role devices.

Secondary channel flags can advertise in the corresponding PHYs. These bits are
mutually exclusive; setting multiple results in Invalid Parameter. Choosing LE
1M or LE 2M results in extended advertising on the primary channel with LE 1M
and the selected PHY on the secondary channel. Choosing LE Coded uses extended
advertising on both primary and secondary channels with LE Coded. Choosing none
results in legacy advertising.

`Duration` configures the length of an Instance in seconds. `0` means a default
value is chosen (2 seconds). If only one instance is added, `Duration` is
ignored. With multiple instances, a round-robin schedule is used.

`Timeout` configures the lifetime of an Instance in seconds. `0` means no
expiration. With a timeout, the instance is removed automatically when the
 timeout passes. Powering down a controller invalidates all instances; adding a
 new instance with a timeout is not possible when powered down.

When a Timeout is provided, `Duration` subtracts from the actual Timeout. For
example, Timeout 5 and Duration 2 schedules the instance three times (2 + 2 +
1 seconds). Other instances do not influence the Timeout.

Re-adding an existing instance updates its configuration.

An instance added or changed while another instance is being advertised will
not be visible immediately; it will be visible when scheduled by the round-robin
advertising algorithm.

Changes to an instance currently being advertised cancel that instance and
switch to the next. The changes are visible next time the instance is scheduled.
If only a single instance is active, changes are visible immediately.

A prerequisite is that LE is already enabled; otherwise this command returns
Rejected.

This command can be used when the controller is not powered and all settings
will be programmed once powered.

This command generates a Command Complete event on success or a Command Status
 event on failure.

Possible errors:

- <span style="color:#c00">Failed</span>
- <span style="color:#c00">Rejected</span>
- <span style="color:#c00">Not Supported</span>
- <span style="color:#c00">Invalid Parameters</span>
- <span style="color:#c00">Invalid Index</span>

### Navigation

- [Previous: Commands (Part 18)](commands-18.md)
- [Next: Commands (Part 20)](commands-20.md)
