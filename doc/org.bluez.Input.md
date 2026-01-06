# org.bluez.Input

## BlueZ D-Bus Input API documentation

| Field | Value |
| --- | --- |
| Version | BlueZ |
| Date | October 2023 |
| Manual section | 5 |
| Manual group | Linux System Administration |

### Interface

- **Service:** `org.bluez`
- **Interface:** `org.bluez.Input1`
- **Object path:** `[variable prefix]/{hci0,hci1,...}/[dev](){BDADDR}`

### Properties

| Property | Type | Access | Description |
| --- | --- | --- | --- |
| ReconnectMode | `string` | `readonly` | Connectability mode of the HID device as defined by the HID Profile specification (Section 5.4.2). This mode is based on HIDReconnectInitiate (Section 5.3.4.6) and HIDNormallyConnectable (Section 5.3.4.14), which define the following values:<br>- `none`: Device and host are not required to automatically restore the connection.<br>- `host`: Bluetooth HID host restores connection.<br>- `device`: Bluetooth HID device restores connection.<br>- `any`: Bluetooth HID device attempts to restore the lost connection, but the Bluetooth HID Host may also restore the connection. |
