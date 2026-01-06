# mgmt commands (part 1)

### Commands

#### Read Management Version Information

**Command Code:** `0x0001`  
**Controller Index:** `<non-controller>`  
**Command Parameters:** none

**Return Parameters:**

| Name | Size |
| --- | --- |
| Version | 1 Octet |
| Revision | 2 Octets |

This command returns the Management version and revision. Besides, being
informational the information can be used to determine whether certain
behavior has changed or bugs fixed when interacting with the kernel.

This command generates a Command Complete event on success or a Command Status
event on failure.

#### Read Management Supported Commands

**Command Code:** `0x0002`  
**Controller Index:** `<non-controller>`  
**Command Parameters:** none

**Return Parameters:**

| Name | Size |
| --- | --- |
| Num_Of_Commands | 2 Octets |
| Num_Of_Events | 2 Octets |
| Command[] | 2 Octets |
| ...[] | ... |
| Event[] | 2 Octets |
| ...[] | ... |

This command returns the list of supported Management commands and
events.

The commands Read Management Version Information and Read management
Supported Commands are not included in this list. Both commands are
always supported and mandatory.

The events Command Status and Command Complete are not included in this
list. Both are implicit and mandatory.

This command generates a Command Complete event on success or a Command Status
event on failure.

#### Read Controller Index List

**Command Code:** `0x0003`  
**Controller Index:** `<non-controller>`  
**Command Parameters:** none

**Return Parameters:**

| Name | Size |
| --- | --- |
| Num_Controllers | 2 Octets |
| Controller_Index[] | 2 Octets |

This command returns the list of currently known controllers.
Controllers added or removed after calling this command can be monitored
using the Index Added and Index Removed events.

This command generates a Command Complete event on success or a Command Status
event on failure.

#### Read Controller Information

**Command Code:** `0x0004`  
**Controller Index:** `<controller id>`  
**Command Parameters:** none

**Return Parameters:**

| Name | Size |
| --- | --- |
| Address | 6 Octets |
| Bluetooth_Version | 1 Octet |
| Manufacturer | 2 Octets |
| Supported_Settings | 4 Octets |
| Current_Settings | 4 Octets |
| Class_Of_Device | 3 Octets |
| Name | 249 Octets |
| Short_Name | 11 Octets |

This command is used to retrieve the current state and basic information
of a controller. It is typically used right after getting the response
to the Read Controller Index List command or an Index Added event.

The Address parameter describes the controllers public address and it
can be expected that it is set. However in case of single mode Low
Energy only controllers it can be 00:00:00:00:00:00. To power on the
controller in this case, it is required to configure a static address
using Set Static Address command first.

If the public address is set, then it will be used as identity address
for the controller. If no public address is available, then the
configured static address will be used as identity address.

In the case of a dual-mode controller with public address that is
configured as Low Energy only device (BR/EDR switched off), the static
address is used when set and public address otherwise.

If no short name is set the Short_Name parameter will be empty (begin
with a nul byte).

Current_Settings and Supported_Settings is a bitmask with currently the
following available bits:

<table>
  <thead>
    <tr>
      <th>Bit</th>
      <th>Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>Powered</td>
    </tr>
    <tr>
      <td>1</td>
      <td>Connectable</td>
    </tr>
    <tr>
      <td>2</td>
      <td>Fast Connectable</td>
    </tr>
    <tr>
      <td>3</td>
      <td>Discoverable</td>
    </tr>
    <tr>
      <td>4</td>
      <td>Bondable</td>
    </tr>
    <tr>
      <td>5</td>
      <td>Link Level Security (Sec. mode 3)</td>
    </tr>
    <tr>
      <td>6</td>
      <td>Secure Simple Pairing</td>
    </tr>
    <tr>
      <td>7</td>
      <td>Basic Rate/Enhanced Data Rate</td>
    </tr>
    <tr>
      <td>8</td>
      <td>High Speed</td>
    </tr>
    <tr>
      <td>9</td>
      <td>Low Energy</td>
    </tr>
    <tr>
      <td>10</td>
      <td>Advertising</td>
    </tr>
    <tr>
      <td>11</td>
      <td>Secure Connections</td>
    </tr>
    <tr>
      <td>12</td>
      <td>Debug Keys</td>
    </tr>
    <tr>
      <td>13</td>
      <td>Privacy</td>
    </tr>
    <tr>
      <td>14</td>
      <td>Controller Configuration</td>
    </tr>
    <tr>
      <td>15</td>
      <td>Static Address</td>
    </tr>
    <tr>
      <td>16</td>
      <td>PHY Configuration</td>
    </tr>
    <tr>
      <td>17</td>
      <td>Wideband Speech</td>
    </tr>
    <tr>
      <td>18</td>
      <td>Connected Isochronous Stream - Central</td>
    </tr>
    <tr>
      <td>19</td>
      <td>Connected Isochronous Stream - Peripheral</td>
    </tr>
    <tr>
      <td>20</td>
      <td>Isochronous Broadcaster</td>
    </tr>
    <tr>
      <td>21</td>
      <td>Synchronized Receiver</td>
    </tr>
    <tr>
      <td>22</td>
      <td>LL Privacy</td>
    </tr>
    <tr>
      <td>23</td>
      <td>PAST Sender</td>
    </tr>
    <tr>
      <td>24</td>
      <td>PAST Receiver</td>
    </tr>
  </tbody>
</table>

This command generates a Command Complete event on success or a Command Status
event on failure.

Possible errors:

- <span style="color:#c00">Invalid Parameters</span>
- <span style="color:#c00">Invalid Index</span>

#### Set Powered

**Command Code:** `0x0005`  
**Controller Index:** `<controller id>`  
**Command Parameters:** `Powered` (1 Octet)

**Return Parameters:**

| Name | Size |
| --- | --- |
| Current_Settings | 4 Octets |

This command is used to power on or off a controller. The allowed
Powered command parameter values are 0x00 and 0x01. All other values
will return Invalid Parameters.

If discoverable setting is activated with a timeout, then switching the
controller off will expire this timeout and disable discoverable.

Settings programmed via Set Advertising and Add/Remove Advertising while
the controller was powered off will be activated when powering the
controller on.

Switching the controller off will permanently cancel and remove all
advertising instances with a timeout set, i.e. time limited advertising
instances are not being remembered across power cycles. Advertising
Removed events will be issued accordingly.

This command generates a Command Complete event on success or a Command Status
event on failure.

Possible errors:

- <span style="color:#c00">Busy</span>
- <span style="color:#c00">Invalid Parameters</span>
- <span style="color:#c00">Invalid Index</span>

### Next

- [Commands (Part 2)](commands-2.md)
