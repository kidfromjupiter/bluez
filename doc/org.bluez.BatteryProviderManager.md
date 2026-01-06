# org.bluez.BatteryProviderManager

## BlueZ D-Bus BatteryProviderManager API documentation

| Field | Value |
| --- | --- |
| Version | BlueZ |
| Date | October 2023 |
| Manual section | 5 |
| Manual group | Linux System Administration |

### Description

A battery provider starts by registering itself with
**RegisterBatteryProvider**, passing an object path as the provider ID. It can
then expose [org.bluez.BatteryProvider](org.bluez.BatteryProvider.md) objects
with paths starting with that provider ID. It can also remove objects at any
time. The objects and properties exposed by battery providers are reflected on
[org.bluez.Battery](org.bluez.Battery.md).

**bluetoothd(8)** will stop monitoring these exposed and removed objects after
**UnregisterBatteryProvider** is called for that provider ID.

### Interface

- **Service:** `org.bluez`
- **Interface:** `org.bluez.BatteryProviderManager1`
- **Object path:** `/org/bluez/{hci0,hci1,...}`

### Methods

#### RegisterBatteryProvider

Signature: `void RegisterBatteryProvider(object provider)`

Registers a battery provider. A registered battery provider can then expose
objects implementing
[org.bluez.BatteryProvider](org.bluez.BatteryProvider.md).

#### UnregisterBatteryProvider

Signature: `void UnregisterBatteryProvider(object provider)`

Unregisters a battery provider previously registered with
**RegisterBatteryProvider**. After unregistration, the
[org.bluez.BatteryProvider](org.bluez.BatteryProvider.md) objects provided by
this client are ignored by **bluetoothd(8)**.
