# org.bluez.AgentManager

## BlueZ D-Bus AgentManager API documentation

| Field | Value |
| --- | --- |
| Version | BlueZ |
| Date | October 2023 |
| Manual section | 5 |
| Manual group | Linux System Administration |

### Interface

- **Service:** `org.bluez`
- **Interface:** `org.bluez.AgentManager1`
- **Object path:** `/org/bluez`

### Methods

#### RegisterAgent

Signature: `void RegisterAgent(object agent, string capability)`

Registers a pairing agent.

The object path defines the path of the agent that will be called when user
input is needed and must implement
[org.bluez.Agent](org.bluez.Agent.md).

Every application can register its own agent, and for all actions triggered by
that application its agent is used.

Applications are not required to register an agent. If an application chooses
not to register an agent, the default agent is used, which is usually the
correct approach. Only applications like a pairing wizard should register their
own agent.

An application can only register one agent. Multiple agents per application are
not supported.

Possible capability values:

- `""`: Fallback to `KeyboardDisplay`.
- `DisplayOnly`
- `DisplayYesNo`
- `KeyboardOnly`
- `NoInputNoOutput`
- `KeyboardDisplay`

Possible errors:

- <span style="color:#c00">org.bluez.Error.InvalidArguments</span>
- <span style="color:#c00">org.bluez.Error.AlreadyExists</span>

#### UnregisterAgent

Signature: `void UnregisterAgent(object agent)`

Unregisters an agent that has been previously registered using
**RegisterAgent**. The object path parameter must match the same value used
at registration.

Possible errors:

- <span style="color:#c00">org.bluez.Error.DoesNotExist</span>

#### RequestDefaultAgent

Signature: `void RequestDefaultAgent(object agent)`

Requests to make the application agent the default agent. The application is
required to register an agent.

Special permission might be required to become the default agent.

Possible errors:

- <span style="color:#c00">org.bluez.Error.DoesNotExist</span>
