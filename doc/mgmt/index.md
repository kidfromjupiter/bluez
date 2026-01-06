# mgmt

## Bluetooth Management Protocol documentation

| Field | Value |
| --- | --- |
| Version | BlueZ |
| Copyright | Free use of this software is granted under the terms of the GNU Lesser General Public Licenses (LGPL). |
| Date | April 2025 |
| Manual section | 7 |
| Manual group | Linux System Administration |

### Synopsis

Bluetooth management sockets are created by setting the `hci_channel` member
of `struct sockaddr_hci` to `HCI_CHANNEL_CONTROL` (see `hci(7)` in
[hci](../hci.md)) when creating a raw HCI socket:

```c
int mgmt_create(void)
{
    struct sockaddr_hci addr;
    int fd;

    fd = socket(PF_BLUETOOTH, SOCK_RAW | SOCK_CLOEXEC | SOCK_NONBLOCK,
                                                            BTPROTO_HCI);
    if (fd < 0)
        return -errno;

    memset(&addr, 0, sizeof(addr));
    addr.hci_family = AF_BLUETOOTH;
    addr.hci_dev = HCI_DEV_NONE;
    addr.hci_channel = HCI_CHANNEL_CONTROL;

    if (bind(fd, (struct sockaddr *) &addr, sizeof(addr)) < 0) {
        int err = -errno;
        close(fd);
        return err;
    }

    return fd;
}
```

Creating the mgmt socket requires the `CAP_NET_ADMIN` capability (e.g. root).

### Description

This document describes the format of data used for communicating with the
kernel using Bluetooth Management sockets. These sockets are available starting
with Linux kernel version 3.4.

### Kernel Version Changes

The following kernel versions introduced new commands, new events, or important
fixes to the Bluetooth Management API:

<table>
  <thead>
    <tr>
      <th>Version</th>
      <th>Kernel Version</th>
      <th>Commands</th>
      <th>Events</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>1.1</td>
      <td>3.4</td>
      <td>Set Device ID</td>
      <td></td>
    </tr>
    <tr>
      <td>1.2</td>
      <td>3.7</td>
      <td></td>
      <td>Passkey Notify</td>
    </tr>
    <tr>
      <td>1.3</td>
      <td>3.9</td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td>1.4</td>
      <td>3.13</td>
      <td>Set Advertising</td>
      <td></td>
    </tr>
    <tr>
      <td></td>
      <td></td>
      <td>Set BR/EDR</td>
      <td></td>
    </tr>
    <tr>
      <td></td>
      <td></td>
      <td>Set Static Address</td>
      <td></td>
    </tr>
    <tr>
      <td></td>
      <td></td>
      <td>Set Scan Parameters</td>
      <td></td>
    </tr>
    <tr>
      <td>1.5</td>
      <td>3.15</td>
      <td>Set Secure Connections</td>
      <td>New Identity Resolving Key</td>
    </tr>
    <tr>
      <td></td>
      <td></td>
      <td>Set Debug Keys</td>
      <td>New Signature Resolving Key</td>
    </tr>
    <tr>
      <td></td>
      <td></td>
      <td>Set Privacy</td>
      <td></td>
    </tr>
    <tr>
      <td></td>
      <td></td>
      <td>Load Identity Resolving Keys</td>
      <td></td>
    </tr>
    <tr>
      <td>1.6</td>
      <td>3.16</td>
      <td>Get Connection Information</td>
      <td></td>
    </tr>
    <tr>
      <td>1.7</td>
      <td>3.17</td>
      <td>Get Clock Information</td>
      <td></td>
    </tr>
    <tr>
      <td></td>
      <td></td>
      <td>Add Device</td>
      <td>Device Added</td>
    </tr>
    <tr>
      <td></td>
      <td></td>
      <td>Remove Device</td>
      <td>Device Removed</td>
    </tr>
    <tr>
      <td></td>
      <td></td>
      <td>Load Connection Parameters</td>
      <td>New Connection Parameter</td>
    </tr>
    <tr>
      <td></td>
      <td></td>
      <td>Read Unconfigured Index List</td>
      <td>Unconfigured Index Added</td>
    </tr>
    <tr>
      <td></td>
      <td></td>
      <td>Read Controller Configuration Information</td>
      <td>Unconfigured Index Removed</td>
    </tr>
    <tr>
      <td></td>
      <td></td>
      <td>Set External Configuration</td>
      <td>New Configuration Options</td>
    </tr>
    <tr>
      <td></td>
      <td></td>
      <td>Set Public Address commands</td>
      <td></td>
    </tr>
    <tr>
      <td>1.8</td>
      <td>3.19</td>
      <td>Start Service Discovery</td>
      <td></td>
    </tr>
    <tr>
      <td>1.9</td>
      <td>4.1</td>
      <td>Read Local Out Of Band Extended Data</td>
      <td>Extended Index Added</td>
    </tr>
    <tr>
      <td></td>
      <td></td>
      <td>Read Extended Controller Index List</td>
      <td>Extended Index Removed</td>
    </tr>
    <tr>
      <td></td>
      <td></td>
      <td>Read Advertising Features</td>
      <td>Local Out Of Band Extended Data Updated</td>
    </tr>
    <tr>
      <td></td>
      <td></td>
      <td>Add Advertising</td>
      <td>Advertising Added</td>
    </tr>
    <tr>
      <td></td>
      <td></td>
      <td>Remove Advertising</td>
      <td>Advertising Removed</td>
    </tr>
    <tr>
      <td>1.10</td>
      <td>4.2</td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td>1.11</td>
      <td>4.5</td>
      <td>Get Advertising Size Information</td>
      <td></td>
    </tr>
    <tr>
      <td></td>
      <td></td>
      <td>Start Limited Discovery</td>
      <td></td>
    </tr>
    <tr>
      <td>1.12</td>
      <td>4.6</td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td>1.13</td>
      <td>4.8</td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td>1.14</td>
      <td>4.9</td>
      <td>Read Extended Controller Information</td>
      <td>Extended Controller Information Changed</td>
    </tr>
    <tr>
      <td></td>
      <td></td>
      <td>Set Appearance</td>
      <td></td>
    </tr>
    <tr>
      <td>1.15</td>
      <td>5.5</td>
      <td>Get PHY Configuration</td>
      <td>PHY Configuration Changed</td>
    </tr>
    <tr>
      <td></td>
      <td></td>
      <td>Set PHY Configuration</td>
      <td></td>
    </tr>
    <tr>
      <td></td>
      <td></td>
      <td>Load Blocked Keys</td>
      <td></td>
    </tr>
    <tr>
      <td>1.16</td>
      <td>5.6</td>
      <td>Set Wideband Speech</td>
      <td></td>
    </tr>
    <tr>
      <td>1.17</td>
      <td>5.7</td>
      <td>Read Controller Capabilities</td>
      <td>Experimental Feature Changed</td>
    </tr>
    <tr>
      <td></td>
      <td></td>
      <td>Read Experimental Features Information</td>
      <td></td>
    </tr>
    <tr>
      <td></td>
      <td></td>
      <td>Set Experimental Feature</td>
      <td></td>
    </tr>
    <tr>
      <td>1.18</td>
      <td>5.8</td>
      <td>Read Default System Configuration</td>
      <td>Default System Configuration Changed</td>
    </tr>
    <tr>
      <td></td>
      <td></td>
      <td>Set Default System Configuration</td>
      <td>Default Runtime Configuration Changed</td>
    </tr>
    <tr>
      <td></td>
      <td></td>
      <td>Read Default Runtime Configuration</td>
      <td>Default Runtime Configuration Changed</td>
    </tr>
    <tr>
      <td></td>
      <td></td>
      <td>Set Default Runtime Configuration</td>
      <td>Device Flags Changed</td>
    </tr>
    <tr>
      <td></td>
      <td></td>
      <td>Get Device Flags</td>
      <td>Advertisement Monitor Added</td>
    </tr>
    <tr>
      <td></td>
      <td></td>
      <td>Set Device Flags</td>
      <td>Advertisement Monitor Removed</td>
    </tr>
    <tr>
      <td></td>
      <td></td>
      <td>Read Advertisement Monitor Features</td>
      <td>Controller Suspend</td>
    </tr>
    <tr>
      <td></td>
      <td></td>
      <td>Add Advertisement Patterns Monitor</td>
      <td>Controller Resume</td>
    </tr>
    <tr>
      <td></td>
      <td></td>
      <td>Remove Advertisement Monitor</td>
      <td>Advertisement Monitor Device Found</td>
    </tr>
    <tr>
      <td></td>
      <td></td>
      <td></td>
      <td>Advertisement Monitor Device Lost</td>
    </tr>
    <tr>
      <td>1.19</td>
      <td>5.10</td>
      <td>Add Extended Advertising Parameters</td>
      <td></td>
    </tr>
    <tr>
      <td></td>
      <td></td>
      <td>Add Extended Advertising Data</td>
      <td></td>
    </tr>
    <tr>
      <td>1.20</td>
      <td>5.11</td>
      <td>Add Advertisement Patterns Monitor With RSSI Threshold</td>
      <td></td>
    </tr>
    <tr>
      <td>1.21</td>
      <td>6.0</td>
      <td>Set Mesh Receiver</td>
      <td>Mesh Device Found</td>
    </tr>
    <tr>
      <td></td>
      <td></td>
      <td>Read Mesh Features</td>
      <td>Mesh Packet Transmit Complete</td>
    </tr>
    <tr>
      <td></td>
      <td></td>
      <td>Transmit Mesh Packet</td>
      <td></td>
    </tr>
    <tr>
      <td></td>
      <td></td>
      <td>Cancel Transmit Mesh Packet</td>
      <td></td>
    </tr>
  </tbody>
</table>

### Packet Structures

Commands:

```text
0    4    8   12   16   22   24   28   31   35   39   43   47
+-------------------+-------------------+-------------------+
|  Command Code     |  Controller Index |  Parameter Length |
+-------------------+-------------------+-------------------+
|                                                           |
```

Events:

```text
0    4    8   12   16   22   24   28   31   35   39   43   47
+-------------------+-------------------+-------------------+
|  Event Code       |  Controller Index |  Parameter Length |
+-------------------+-------------------+-------------------+
|                                                           |
```

All fields are in little-endian byte order (least significant byte first).

Controller Index can have a special value \<non-controller\> to indicate
that command or event is not related to any controller. Possible values:

<table>
  <thead>
    <tr>
      <th>Value</th>
      <th>Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0x0000-0xFFFE</td>
      <td>&lt;controller id&gt;</td>
    </tr>
    <tr>
      <td>0xFFFF</td>
      <td>&lt;non-controller&gt;</td>
    </tr>
  </tbody>
</table>

### Error Codes

The following values have been defined for use with the Command Status and
Command Complete events:

<table>
  <thead>
    <tr>
      <th>Value</th>
      <th>Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0x00</td>
      <td>Success</td>
    </tr>
    <tr>
      <td>0x01</td>
      <td>Unknown Command</td>
    </tr>
    <tr>
      <td>0x02</td>
      <td>Not Connected</td>
    </tr>
    <tr>
      <td>0x03</td>
      <td>Failed</td>
    </tr>
    <tr>
      <td>0x04</td>
      <td>Connect Failed</td>
    </tr>
    <tr>
      <td>0x05</td>
      <td>Authentication Failed</td>
    </tr>
    <tr>
      <td>0x06</td>
      <td>Not Paired</td>
    </tr>
    <tr>
      <td>0x07</td>
      <td>No Resources</td>
    </tr>
    <tr>
      <td>0x08</td>
      <td>Timeout</td>
    </tr>
    <tr>
      <td>0x09</td>
      <td>Already Connected</td>
    </tr>
    <tr>
      <td>0x0A</td>
      <td>Busy</td>
    </tr>
    <tr>
      <td>0x0B</td>
      <td>Rejected</td>
    </tr>
    <tr>
      <td>0x0C</td>
      <td>Not Supported</td>
    </tr>
    <tr>
      <td>0x0D</td>
      <td>Invalid Parameters</td>
    </tr>
    <tr>
      <td>0x0E</td>
      <td>Disconnected</td>
    </tr>
    <tr>
      <td>0x0F</td>
      <td>Not Powered</td>
    </tr>
    <tr>
      <td>0x10</td>
      <td>Cancelled</td>
    </tr>
    <tr>
      <td>0x11</td>
      <td>Invalid Index</td>
    </tr>
    <tr>
      <td>0x12</td>
      <td>RFKilled</td>
    </tr>
    <tr>
      <td>0x13</td>
      <td>Already Paired</td>
    </tr>
    <tr>
      <td>0x14</td>
      <td>Permission Denied</td>
    </tr>
  </tbody>
</table>

As a general rule, all commands generate the events specified in the commands
section. Invalid lengths or unknown commands always generate a Command Status
response (with Unknown Command or Invalid Parameters status). Sending a command
with an invalid Controller Index value also always generates a Command Status
event with the Invalid Index status code.

### Next

- [Commands (Part 1)](commands-1.md)
- [Commands (Part 2)](commands-2.md)
- [Commands (Part 3)](commands-3.md)
- [Commands (Part 4)](commands-4.md)
- [Commands (Part 5)](commands-5.md)
- [Commands (Part 6)](commands-6.md)
- [Commands (Part 7)](commands-7.md)
- [Commands (Part 8)](commands-8.md)
- [Commands (Part 9)](commands-9.md)
- [Commands (Part 10)](commands-10.md)
- [Commands (Part 11)](commands-11.md)
- [Commands (Part 12)](commands-12.md)
- [Commands (Part 13)](commands-13.md)
- [Commands (Part 14)](commands-14.md)
- [Commands (Part 15)](commands-15.md)
- [Commands (Part 16)](commands-16.md)
- [Commands (Part 17)](commands-17.md)
- [Commands (Part 18)](commands-18.md)
- [Commands (Part 19)](commands-19.md)
- [Commands (Part 20)](commands-20.md)
- [Commands (Part 21)](commands-21.md)
- [Commands (Part 22)](commands-22.md)
- [Commands (Part 23)](commands-23.md)
- [Commands (Part 24)](commands-24.md)
- [Commands (Part 25)](commands-25.md)
- [Commands (Part 26)](commands-26.md)
- [Commands (Part 27)](commands-27.md)
