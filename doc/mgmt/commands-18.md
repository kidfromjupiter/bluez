# mgmt commands (part 18)

### Commands

#### Read Extended Controller Index List (since 1.9)

**Command Code:** `0x003C`  
**Controller Index:** `<non-controller>`  
**Command Parameters:** none

**Return Parameters:** `Num_Controllers` (2 Octets), `Controller_Index[i]`
(2 Octets), `Controller_Type[i]` (1 Octet), `Controller_Bus[i]` (1 Octet)

This command returns the list of currently known controllers, including
configured, unconfigured, and alternate controllers.

Controllers added or removed after calling this command can be monitored using
Extended Index Added and Extended Index Removed events.

The existing Index Added, Index Removed, Unconfigured Index Added and
Unconfigured Index Removed are no longer sent after this command has been used
at least once.

Instead of calling Read Controller Index List and Read Unconfigured Controller
Index List, this command combines all information and can be used to retrieve
 the controller list.

`Controller_Type` values:

| Value | Description |
| --- | --- |
| 0x00 | Primary Controller (BR/EDR and/or LE) |
| 0x01 | Unconfigured Controller (BR/EDR and/or LE) |
| 0x02 | Alternate MAC/PHY Controller (AMP) |

0x00 and 0x01 indicate a primary BR/EDR and/or LE controller. The difference is
whether configuration is required. Fully configured controllers (even those
supporting configuration changes) are listed as 0x00.

Alternate MAC/PHY controllers are listed as 0x02. They do not support the
configured/unconfigured distinction.

`Controller_Bus` values:

| Value | Description |
| --- | --- |
| 0x00 | Virtual |
| 0x01 | USB |
| 0x02 | PCMCIA |
| 0x03 | UART |
| 0x04 | RS232 |
| 0x05 | PCI |
| 0x06 | SDIO |
| 0x07 | SPI |
| 0x08 | I2C |
| 0x09 | SMD |
| 0x0A | VIRTIO |
| 0x0B | IPC |

Controllers marked as RAW-only operation are currently not listed by this
command.

This command generates a Command Complete event on success or a Command Status
 event on failure.

#### Read Advertising Features (since 1.9)

**Command Code:** `0x003D`  
**Controller Index:** `<controller id>`  
**Command Parameters:** none

**Return Parameters:** `Supported_Flags` (4 Octets), `Max_Adv_Data_Len` (1
Octet), `Max_Scan_Rsp_Len` (1 Octet), `Max_Instances` (1 Octet), `Num_Instances`
(1 Octet), `Instance[]` (1 Octet), `...[]`

This command reads the advertising features supported by the controller and
stack.

`Supported_Flags` bit meanings (for Add Advertising Flags):

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

Bit 0 indicates support for connectable advertising and switching into
connectable mode independent of the global connectable setting. When not
supported, the global connectable setting determines advertising type and
address usage.

Bit 1 indicates support for advertising with discoverable mode enabled. This
reduces `Max_Adv_Data_Len` by 3 octets; flags are managed and added in front of
 user data.

Bit 2 indicates support for advertising with limited discoverable mode enabled,
with the same 3-octet reduction and flag handling as bit 1.

Bit 3 indicates support for automatically keeping the Flags field updated. This
reduces `Max_Adv_Data_Len` by 3 octets. Flags are added in front of user data.
If bits 1 or 2 are used, this is implicitly used even if not marked supported.

Bit 4 indicates support for automatically adding TX Power to advertising data.
This reduces `Max_Adv_Data_Len` by 3 octets and adds TX Power at the end of user
 data. If TX Power is not supported, this bit is not set.

Bit 5 indicates support for automatically adding Appearance to scan response
 data. This reduces `Max_Scan_Rsp_Len` by 4 octets and adds Appearance in front
of user data. If appearance is unsupported, this bit is not set.

Bit 6 indicates support for automatically adding Local Name to scan response
 data. If enough space is available it is added; otherwise a short version or no
name is used. Local Name is added at the end of scan response data.

Bits 7–9 indicate support for secondary channel advertising with LE 1M, LE 2M,
and LE Coded PHY respectively.

Instance identifiers valid range: 1–254. Value 0 is reserved for internal use,
255 for future extensions. `Max_Instances` may be 0 if advertising is not
supported.

`Max_Adv_Data_Len` and `Max_Scan_Rsp_Len` report maximum lengths; flags may
reduce the actual available length. Currently these values are 31.

`Num_Instances` and `Instance[]` report currently occupied instance identifiers.

This command generates a Command Complete event on success or a Command Status
 event on failure.

Possible errors:

- <span style="color:#c00">Invalid Parameters</span>
- <span style="color:#c00">Invalid Index</span>

### Navigation

- [Previous: Commands (Part 17)](commands-17.md)
- [Next: Commands (Part 19)](commands-19.md)
