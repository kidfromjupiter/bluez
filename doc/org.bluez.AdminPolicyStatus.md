# org.bluez.AdminPolicyStatus

## BlueZ D-Bus AdminPolicyStatus API documentation

| Field | Value |
| --- | --- |
| Version | BlueZ |
| Date | October 2023 |
| Manual section | 5 |
| Manual group | Linux System Administration |

### Description

Interface AdminPolicyStatus1 provides readonly properties indicating the
current values of admin policy affecting
[org.bluez.Adapter](org.bluez.Adapter.md) and
[org.bluez.Device](org.bluez.Device.md) objects.

### Interfaces

#### Adapter

- **Service:** `org.bluez`
- **Interface:** `org.bluez.AdminPolicyStatus1` (experimental)
- **Object path:** `[variable prefix]/{hci0,hci1,...}`

#### Device

- **Service:** `org.bluez`
- **Interface:** `org.bluez.AdminPolicyStatus1` (experimental)
- **Object path:** `[variable prefix]/{hci0,hci1,...}/[dev](){BDADDR}`

### Properties

| Property | Type | Access | Description |
| --- | --- | --- | --- |
| ServiceAllowList | `array{string}` | `readonly` (adapter-only) | Current value of the service allowlist. |
| IsAffectedByPolicy | `bool` | `readonly` (device-only) | Indicates if any auto-connect profile in this device is not allowed by admin policy. |
