# org.bluez.Battery

## BlueZ D-Bus Battery API documentation

| Field | Value |
| --- | --- |
| Version | BlueZ |
| Date | October 2023 |
| Manual section | 5 |
| Manual group | Linux System Administration |

### Interface

- **Service:** `org.bluez`
- **Interface:** `org.bluez.Battery1`
- **Object path:** `[variable prefix]/{hci0,hci1,...}/[dev](){BDADDR}`

### Properties

| Property | Type | Access | Description |
| --- | --- | --- | --- |
| Percentage | `byte` | `readonly` | The percentage of battery left as an unsigned 8-bit integer. |
| Source | `string` | `readonly`, `optional` | Describes where the battery information comes from. This property is informational only and may be useful for debugging purposes. Providers from [org.bluez.BatteryProvider](org.bluez.BatteryProvider.md) may set this to indicate where the battery report comes from (for example, `HFP 1.7`, `HID`, or the profile UUID). |
