# org.bluez.Agent

## BlueZ D-Bus Agent API documentation

| Field | Value |
| --- | --- |
| Version | BlueZ |
| Date | October 2023 |
| Manual section | 5 |
| Manual group | Linux System Administration |

### Interface

- **Service:** unique name
- **Interface:** `org.bluez.Agent1`
- **Object path:** freely definable

Agents are registered through
[org.bluez.AgentManager](org.bluez.AgentManager.md).

### Methods

#### Release

Signature: `void Release()`

Called when **bluetoothd(8)** unregisters the agent.

An agent can use it for cleanup. There is no need to unregister the agent
manually because it has already been unregistered by the time this method is
called.

#### RequestPinCode

Signature: `string RequestPinCode(object device)`

Called when **bluetoothd(8)** needs a PIN code for authentication.

Return a string of 1-16 characters. The string can be alphanumeric.

Possible errors:

- <span style="color:#c00">org.bluez.Error.Rejected</span>
- <span style="color:#c00">org.bluez.Error.Canceled</span>

#### DisplayPinCode

Signature: `void DisplayPinCode(object device, string pincode)`

Called when **bluetoothd(8)** needs to display a PIN code for
authentication.

An empty reply should be returned. When the PIN code no longer needs to be
shown, the **Cancel** method will be called.

This is used during pairing with keyboards that do not support Bluetooth 2.1
Secure Simple Pairing, in contrast to **DisplayPasskey** which is used for
those that do.

This method will only be called once since older keyboards do not support
typing notification.

Note: the PIN will always be a 6-digit number, zero-padded to 6 digits.

Possible errors:

- <span style="color:#c00">org.bluez.Error.Rejected</span>
- <span style="color:#c00">org.bluez.Error.Canceled</span>

#### RequestPasskey

Signature: `uint32 RequestPasskey(object device)`

Called when **bluetoothd(8)** needs a passkey for authentication.

Return a numeric value between 0 and 999999.

Possible errors:

- <span style="color:#c00">org.bluez.Error.Rejected</span>
- <span style="color:#c00">org.bluez.Error.Canceled</span>

#### DisplayPasskey

Signature: `void DisplayPasskey(object device, uint32 passkey, uint16 entered)`

Called when **bluetoothd(8)** needs to display a passkey for
authentication.

The `entered` parameter indicates the number of keys already typed on the
remote side.

An empty reply should be returned. When the passkey no longer needs to be
shown, the **Cancel** method will be called.

During the pairing process this method might be called multiple times to update
`entered`.

Note: the passkey will always be a 6-digit number, so displays should be
zero-padded if the value has fewer digits.

#### RequestConfirmation

Signature: `void RequestConfirmation(object device, uint32 passkey)`

Called when **bluetoothd(8)** needs confirmation of a passkey for
authentication.

To confirm, return an empty reply or an error if the passkey is invalid.

Note: the passkey will always be a 6-digit number, so displays should be
zero-padded if the value has fewer digits.

Possible errors:

- <span style="color:#c00">org.bluez.Error.Rejected</span>
- <span style="color:#c00">org.bluez.Error.Canceled</span>

#### RequestAuthorization

Signature: `void RequestAuthorization(object device)`

Called to request user authorization for an incoming pairing attempt that
would otherwise trigger the just-works model, or when a user plugged in a
cable-paired device. In the latter case, the device may not yet be connected to
the adapter over Bluetooth.

Possible errors:

- <span style="color:#c00">org.bluez.Error.Rejected</span>
- <span style="color:#c00">org.bluez.Error.Canceled</span>

#### AuthorizeService

Signature: `void AuthorizeService(object device, string uuid)`

Called when **bluetoothd(8)** needs to authorize a connection or service
request.

Possible errors:

- <span style="color:#c00">org.bluez.Error.Rejected</span>
- <span style="color:#c00">org.bluez.Error.Canceled</span>

#### Cancel

Signature: `void Cancel()`

Called to indicate that the agent request failed before a reply was returned.

### Parameters

- `device` is an [org.bluez.Device](org.bluez.Device.md) object path.
