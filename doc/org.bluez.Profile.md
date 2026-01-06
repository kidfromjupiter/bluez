# org.bluez.Profile

## BlueZ D-Bus Profile API documentation

| Field | Value |
| --- | --- |
| Version | BlueZ |
| Date | October 2023 |
| Manual section | 5 |
| Manual group | Linux System Administration |

### Interface

- **Service:** unique name
- **Interface:** `org.bluez.Profile1`
- **Object path:** freely definable

Profiles are registered through
[org.bluez.ProfileManager](org.bluez.ProfileManager.md).

### Methods

#### Release

Signature: `void Release()` (noreply)

Called when **bluetoothd(8)** unregisters the profile.

A profile can use it for cleanup. There is no need to unregister the profile
manually because it has already been unregistered by the time this method is
called.

#### NewConnection

Signature: `void NewConnection(object device, fd, dict fd_properties)`

Called when a new service level connection has been made and authorized.

Possible `fd_properties` values:

- **Version** (`uint16`, optional): Profile version.
- **Features** (`uint16`, optional): Profile features.

Possible errors:

- <span style="color:#c00">org.bluez.Error.Rejected</span>
- <span style="color:#c00">org.bluez.Error.Canceled</span>
