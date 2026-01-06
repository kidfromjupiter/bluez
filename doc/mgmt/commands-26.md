# mgmt commands (part 26)

### Commands

#### Add Advertisement Patterns Monitor With RSSI Threshold (since 1.20)

**Command Code:** `0x0056`  
**Controller Index:** `<controller id>`  
**Command Parameters:** `High_Threshold` (1 Octet), `High_Threshold_Timer`
(2 Octets), `Low_Threshold` (1 Octet), `Low_Threshold_Timer` (2 Octets),
`Sampling_Period` (1 Octet), `Pattern_Count` (1 Octet), `AD_Type[]` (1 Octet),
`Offset[]` (1 Octet), `Length[]` (1 Octet), `Value[]` (31 Octets), `...[]`

**Return Parameters:** `Monitor_Handle` (2 Octets)

This command is essentially the same as Add Advertisement Patterns Monitor, but
with additional RSSI parameters. If the controller supports advertisement
filtering, the advertisement data is filtered with the RSSI parameters; if not,
it behaves like Add Advertisement Patterns Monitor.

Devices are considered in-range if RSSI is greater than `High_Threshold` dBm for
`High_Threshold_Timer` seconds. Devices are considered lost if no advertisements
have RSSI greater than `Low_Threshold` dBm for `Low_Threshold_Timer` seconds.
Only in-range device data is propagated.

`Sampling_Period`:

| Value | Description |
| --- | --- |
| `0x00` | Propagate all advertisements from in-range devices |
| `0xFF` | Propagate only the first advertisement from in-range devices; when lost and found again, propagate the first again |
| `other` | Group advertisements into `100ms * N`; report once per group with averaged RSSI |

Possible errors:

- <span style="color:#c00">Failed</span>
- <span style="color:#c00">Busy</span>
- <span style="color:#c00">No Resources</span>
- <span style="color:#c00">Invalid Parameters</span>

#### Set Mesh Receiver (since 1.21)

**Command Code:** `0x0057`  
**Controller Index:** `<controller id>`  
**Command Parameters:** `Enable` (1 Octet), `Window` (2 Octets), `Period`
(2 Octets), `Num AD Types` (1 Octet), `AD Types[]`

**Return Parameters:** none

This command enables or disables mesh receiving. When enabled, passive scanning
remains enabled for this controller.

`Window` and `Period` configure scan parameters when no other scanning is active.

`Num AD Types` and `AD Types` filter advertising and scan responses by AD type.
Responses that do not contain at least one of the requested AD types are
ignored; otherwise they are delivered with the Mesh Device Found event.

Possible errors:

- <span style="color:#c00">Failed</span>
- <span style="color:#c00">No Resources</span>
- <span style="color:#c00">Invalid Parameters</span>

#### Read Mesh Features (since 1.21)

**Command Code:** `0x0058`  
**Controller Index:** `<controller id>`  
**Command Parameters:** none

**Return Parameters:** `Index` (2 Octets), `Max Handles` (1 Octet),
`Used Handles` (1 Octet), `Handle[]`

This command verifies outbound mesh packet support and reports queue
information.

`Index` identifies the HCI controller this information applies to.

`Max Handles` is the maximum number of packets that can be queued.

`Used Handles` is the number of packets awaiting transmission.

`Handle` is an array of the currently outstanding packets.

Possible errors:

- <span style="color:#c00">Failed</span>
- <span style="color:#c00">No Resources</span>
- <span style="color:#c00">Invalid Parameters</span>

### Navigation

- [Previous: Commands (Part 25)](commands-25.md)
- [Next: Commands (Part 27)](commands-27.md)
