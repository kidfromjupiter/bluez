# org.bluez.AdvertisementMonitorManager

## BlueZ D-Bus AdvertisementMonitorManager API documentation

| Field | Value |
| --- | --- |
| Version | BlueZ |
| Date | October 2023 |
| Manual section | 5 |
| Manual group | Linux System Administration |

### Interface

- **Service:** `org.bluez`
- **Interface:** `org.bluez.AdvertisementMonitorManager1` (experimental)
- **Object path:** `/org/bluez/{hci0,hci1,...}`

### Methods

#### RegisterMonitor

Signature: `void RegisterMonitor(object application)`

Registers the root path of a hierarchy of advertisement monitors
implementing [org.bluez.AdvertisementMonitor](org.bluez.AdvertisementMonitor.md).

The application object path together with the D-Bus system bus connection ID
define the identification of the application registering advertisement monitors.

Once a root path is registered by a client via this method, the client can
freely expose or unexpose advertisement monitors without re-registering the
root path.

After use, the client should call **UnregisterMonitor** to invalidate the
advertisement monitors.

Possible errors:

- <span style="color:#c00">org.bluez.Error.InvalidArguments</span>
- <span style="color:#c00">org.bluez.Error.AlreadyExists</span>
- <span style="color:#c00">org.bluez.Error.Failed</span>

#### UnregisterMonitor

Signature: `void UnregisterMonitor(object application)`

Unregisters a hierarchy of advertisement monitors that has been previously
registered with **RegisterMonitor**. The object path parameter must match the
same value used on registration.

Upon unregistration, the advertisement monitor(s) should expect to receive the
**Release()** method as the signal that the advertisement monitor(s) has been
deactivated.

Possible errors:

- <span style="color:#c00">org.bluez.Error.InvalidArguments</span>
- <span style="color:#c00">org.bluez.Error.DoesNotExist</span>

### Properties

| Property | Type | Access | Description |
| --- | --- | --- | --- |
| SupportedMonitorTypes | `array{string}` | `read-only` | Supported types of advertisement monitors. Applications should check this before instantiating and exposing [org.bluez.AdvertisementMonitor](org.bluez.AdvertisementMonitor.md).<br><br>Possible values: `or_patterns` (Patterns with OR logic applied; **Patterns** must exist and have at least one pattern). |
| SupportedFeatures | `array{string}` | `read-only` | Features of advertisement monitoring supported by **bluetoothd(8)**. Possible values: `controller-patterns` (offloads patterns to the controller to reduce power consumption when supported). |
