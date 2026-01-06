# org.bluez.GattManager

## BlueZ D-Bus GattManager API documentation

| Field | Value |
| --- | --- |
| Version | BlueZ |
| Date | October 2023 |
| Manual section | 5 |
| Manual group | Linux System Administration |

### Description

The GATT Manager allows external applications to register GATT services and
profiles.

Registering a profile allows applications to subscribe to *remote/client*
services.

Registering a service allows applications to publish a *local/server* GATT
service, which then becomes available to remote devices. A GATT service is
represented by a D-Bus object hierarchy where the root node corresponds to a
service and the child nodes represent characteristics and descriptors that
belong to that service. Each node must implement one of the following
interfaces based on the attribute it represents:

- [org.bluez.GattService](org.bluez.GattService.md)
- [org.bluez.GattCharacteristic](org.bluez.GattCharacteristic.md)
- [org.bluez.GattDescriptor](org.bluez.GattDescriptor.md)

Each node must also implement the standard D-Bus Properties interface to expose
its properties. These objects collectively represent a GATT service definition.

To make service registration simple, **bluetoothd(8)** requires that all
objects that belong to a GATT service be grouped under a D-Bus Object Manager
that solely manages the objects of that service. Hence, the standard
DBus.ObjectManager interface must be available on the root service path. An
example application hierarchy containing two separate GATT services may look
like this:

```text
-> /com/example
  |   - org.freedesktop.DBus.ObjectManager
  |
  -> /com/example/service0
  | |   - org.freedesktop.DBus.Properties
  | |   - org.bluez.GattService1
  | |
  | -> /com/example/service0/char0
  | |     - org.freedesktop.DBus.Properties
  | |     - org.bluez.GattCharacteristic1
  | |
  | -> /com/example/service0/char1
  |   |   - org.freedesktop.DBus.Properties
  |   |   - org.bluez.GattCharacteristic1
  |   |
  |   -> /com/example/service0/char1/desc0
  |       - org.freedesktop.DBus.Properties
  |       - org.bluez.GattDescriptor1
  |
  -> /com/example/service1
    |   - org.freedesktop.DBus.Properties
    |   - org.bluez.GattService1
    |
   -> /com/example/service1/char0
        - org.freedesktop.DBus.Properties
        - org.bluez.GattCharacteristic1
```

When a service is registered, **bluetoothd(8)** will automatically obtain
information about all objects using the service's Object Manager. Once a
service has been registered, the objects of a service should not be removed. If
**bluetoothd(8)** receives an InterfacesRemoved signal from a service's Object
Manager, it will immediately unregister the service. Similarly, if the
application disconnects from the bus, all of its registered services will be
automatically unregistered. InterfacesAdded signals will be ignored.

### Interface

- **Service:** `org.bluez`
- **Interface:** `org.bluez.GattManager1`
- **Object path:** `[variable prefix]/{hci0,hci1,...}`

### Methods

#### RegisterApplication

Signature: `void RegisterApplication(object application, dict options)`

Registers a local GATT services hierarchy (GATT Server) and/or GATT profiles
(GATT Client).

The application object path together with the D-Bus system bus connection ID
define the identification of the application registering a GATT-based service
([org.bluez.GattService](org.bluez.GattService.md)) and/or profile
([org.bluez.GattProfile](org.bluez.GattProfile.md)).

Possible errors:

- <span style="color:#c00">org.bluez.Error.InvalidArguments</span>
- <span style="color:#c00">org.bluez.Error.AlreadyExists</span>

#### UnregisterApplication

Signature: `void UnregisterApplication(object application)`

Unregisters services and/or profiles previously registered using
**RegisterApplication**. The object path parameter must match the same value
used on registration.

Possible errors:

- <span style="color:#c00">org.bluez.Error.InvalidArguments</span>
- <span style="color:#c00">org.bluez.Error.DoesNotExist</span>
