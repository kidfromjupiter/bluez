# org.bluez.AdminPolicySet

## BlueZ D-Bus AdminPolicySet API documentation

| Field | Value |
| --- | --- |
| Version | BlueZ |
| Date | October 2023 |
| Manual section | 5 |
| Manual group | Linux System Administration |

### Description

This API provides methods to control the behavior of **bluetoothd(8)** as an
administrator for [org.bluez.Adapter](org.bluez.Adapter.md) and
[org.bluez.Device](org.bluez.Device.md) behavior.

Interface AdminPolicySet1 provides methods to set policies. Once a policy is
set successfully, it affects all clients and persists even after restarting
**bluetoothd(8)**. The only way to clear it is to overwrite the policy with the
same method.

### Interface

- **Service:** `org.bluez`
- **Interface:** `org.bluez.AdminPolicySet1` (experimental)
- **Object path:** `[variable prefix]/{hci0,hci1,...}`

### Methods

#### SetServiceAllowList

Signature: `void SetServiceAllowList(array{string} UUIDs)`

Sets the service allowlist by specifying service UUIDs.

When called, **bluetoothd(8)** will block incoming and outgoing connections to
services not in UUIDs for all clients.

Subsequent calls supersede any previously set allowlist values. Calling this
method with an empty array allows any service UUIDs to be used.

The default value is an empty array.

Possible errors:

- <span style="color:#c00">org.bluez.Error.InvalidArguments</span>
- <span style="color:#c00">org.bluez.Error.Failed</span>
