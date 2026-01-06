# org.bluez.obex.AgentManager

## BlueZ D-Bus OBEX AgentManager API documentation

| Field | Value |
| --- | --- |
| Version | BlueZ |
| Date | October 2023 |
| Manual section | 5 |
| Manual group | Linux System Administration |

### Interface

- **Service:** `org.bluez.obex`
- **Interface:** `org.bluez.obex.AgentManager1`
- **Object path:** `/org/bluez/obex`

### Methods

#### RegisterAgent

Signature: `void RegisterAgent(object agent)`

Registers an agent (implements
[org.bluez.obex.Agent](org.bluez.obex.Agent.md)) to request user
authorization to accept or reject objects.

The object push service needs to authorize each received object.

Possible errors:

- <span style="color:#c00">org.bluez.obex.Error.AlreadyExists</span>

#### UnregisterAgent

Signature: `void UnregisterAgent(object agent)`

Unregisters the agent previously registered using **RegisterAgent**. The object
path parameter must match the value used on registration.

Possible errors:

- <span style="color:#c00">org.bluez.obex.Error.DoesNotExist</span>
