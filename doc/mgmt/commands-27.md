# mgmt commands (part 27)

### Commands

#### Transmit Mesh Packet (since 1.21)

**Command Code:** `0x0059`  
**Controller Index:** `<controller id>`  
**Command Parameters:** `Addr` (6 Octets), `Addr Type` (1 Octet),
`Instant` (8 Octets), `Delay` (2 Octets), `Count` (1 Octet),
`Data Length` (1 Octet), `Data[]` (variable)

**Return Parameters:** `Handle` (1 Octet)

This command sends a mesh packet as a NONCONN LE advertisement.

`Addr` and `Addr Type` specify the address to use for the outbound advertising
packet. If `BD_ADDR_ANY` and `LE_RANDOM` are set, the kernel creates a single
use non-resolvable address.

`Instant` is used with `Delay` to precisely time sending the advertising packet.
It should be set to the Instant tag from a received Mesh Device Found event. It
is only useful in POLL-RESPONSE situations where a response must be sent within
 a negotiated time window. The `Instant` value should not be interpreted by the
host; only the controller uses it.

If `Delay` is `0x0000`, the packet is sent at the earliest opportunity. If
non-zero and the controller supports delayed delivery, `Instant` and `Delay`
are used to delay the packet. `Delay` is specified in milliseconds.

`Count` must be non-zero and indicates how many times the packet is sent before
transmission completes. If `Delay` is non-zero, `Count` must be 1.

`Data` is an octet array of the AD type and mesh packet.

The command returns immediately and, on success, generates a Mesh Packet
Transmission Complete event after the packet has been sent.

Possible errors:

- <span style="color:#c00">Failed</span>
- <span style="color:#c00">Busy</span>
- <span style="color:#c00">No Resources</span>
- <span style="color:#c00">Invalid Parameters</span>

#### Cancel Transmit Mesh Packet (since 1.21)

**Command Code:** `0x005A`  
**Controller Index:** `<controller id>`  
**Command Parameters:** `Handle` (1 Octet)

**Return Parameters:** none

This command cancels an outbound transmission request.

`Handle` is the value returned by a successful Transmit Mesh Packet command. If
`0x00` is specified, all outstanding send requests are canceled.

For each mesh packet canceled, the Mesh Packet Transmission Complete event is
generated regardless of whether the packet was sent successfully.

Possible errors:

- <span style="color:#c00">Failed</span>
- <span style="color:#c00">Invalid Parameters</span>

#### Send HCI command and wait for event

**Command Code:** `0x005B`  
**Controller Index:** `<controller id>`  
**Command Parameters:** `Opcode` (2 Octets), `Event` (1 Octet),
`Timeout` (1 Octet), `Parameter Length` (2 Octets), `Parameter[]` (variable)

**Return Parameters:** `Response` (1-variable Octets)

This command sends an HCI command and waits for an (optional) event.

`Opcode` specifies the HCI command. Any opcode is supported, including vendor
commands, but unlike Raw/User channel it runs as an HCI command sent by the
kernel. This allows waiting for a specific event as a response.

Setting `Event` to `0x00` waits for either HCI Command Status or HCI Command
Complete.

`Timeout` is specified in seconds. Setting it to `0` uses the default timeout.

Possible errors:

- <span style="color:#c00">Failed</span>
- <span style="color:#c00">Invalid Parameters</span>

### Navigation

- [Previous: Commands (Part 26)](commands-26.md)
