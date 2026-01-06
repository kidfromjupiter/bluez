# org.bluez.obex.Agent

## BlueZ D-Bus OBEX Agent API documentation

| Field | Value |
| --- | --- |
| Version | BlueZ |
| Date | October 2023 |
| Manual section | 5 |
| Manual group | Linux System Administration |

### Interface

- **Service:** unique name
- **Interface:** `org.bluez.obex.Agent1`
- **Object path:** freely definable

Agents are registered through
[org.bluez.obex.AgentManager](org.bluez.obex.AgentManager.md).

### Methods

#### Release

Signature: `void Release()`

Called when **obexd(8)** unregisters the agent.

An agent can use it for cleanup. There is no need to unregister the agent
manually because it has already been unregistered by the time this method is
called.

#### AuthorizePush

Signature: `string AuthorizePush(object transfer)`

Called when **obexd(8)** needs to accept or reject a Bluetooth object push
request.

Return the full path (including the filename) or the folder name suffixed with
`/` where the object should be stored.

The transfer object (see [org.bluez.obex.Transfer](org.bluez.obex.Transfer.md))
contains a **Filename** property with the default location and name that can be
returned.

Possible errors:

- <span style="color:#c00">org.bluez.obex.Error.Rejected</span>
- <span style="color:#c00">org.bluez.obex.Error.Canceled</span>

#### Cancel

Signature: `void Cancel()`

Called to indicate that the agent request failed before a reply was returned.
Cancels the previous request.
