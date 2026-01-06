# org.bluez.BatteryProvider

## BlueZ D-Bus BatteryProvider API documentation

| Field | Value |
| --- | --- |
| Version | BlueZ |
| Date | October 2023 |
| Manual section | 5 |
| Manual group | Linux System Administration |

### Interface

- **Service:** `<client D-Bus address>`
- **Interface:** `org.bluez.BatteryProvider1`
- **Object path:** `{provider_root}/{unique battery object path}`

### Properties

Objects provided on this interface contain the same properties as
[org.bluez.Battery](org.bluez.Battery.md). Additionally, this interface requires
the **Device** property indicating the object path of the device this battery
provides.

| Property | Type | Access | Description |
| --- | --- | --- | --- |
| Device | `object` | `readonly` | The object path of the [org.bluez.Device](org.bluez.Device.md) that has this battery. |
