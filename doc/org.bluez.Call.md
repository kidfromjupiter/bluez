# org.bluez.Call1

## BlueZ D-Bus Telephony Call API documentation

| Field | Value |
| --- | --- |
| Version | BlueZ |
| Date | May 2025 |
| Manual section | 5 |
| Manual group | Linux System Administration |

### Interface

- **Service:** `org.bluez`
- **Interface:** `org.bluez.Call1` (experimental)
- **Object path:** `[variable prefix]/{hci0,hci1,...}/[dev](){BDADDR}/telephony#/call#`

### Methods

#### Answer

Signature: `void Answer()`

Answers an incoming call. Only valid if the state of the call is `incoming`.

Possible errors:

- <span style="color:#c00">org.bluez.Error.InvalidState</span>
- <span style="color:#c00">org.bluez.Error.Failed</span>

#### Hangup

Signature: `void Hangup()`

Hangs up the call.

For an incoming call, the call is hung up using ATH or equivalent. For a
waiting call, the remote party is notified using the User Determined User Busy
(UDUB) condition, generally implemented using CHLD=0.

The GSM specification does not allow releasing a held call when a waiting call
exists. This is because 27.007 allows CHLD=1X to operate only on active calls.
Hence a held call cannot be hung up without affecting the state of the incoming
call (e.g. using other CHLD alternatives). Most manufacturers provide vendor
extensions that allow the state of a held call to be modified using CHLD=1X or
similar. Bluetooth HFP specifies the classic 27.007 behavior and does not allow
CHLD=1X to modify the state of held calls.

Releasing a particular party of a held multiparty call might not be possible
on some implementations. Applications should structure their UI accordingly.

Releasing active calls does not produce side effects. The state of held or
waiting calls is not affected. As an exception, when a single active call and a
waiting call are present, releasing the active call results in the waiting call
transitioning to `incoming`.

Possible errors:

- <span style="color:#c00">org.bluez.Error.InvalidState</span>
- <span style="color:#c00">org.bluez.Error.Failed</span>

### Properties

| Property | Type | Access | Description |
| --- | --- | --- | --- |
| LineIdentification | `string` | `readonly` | Line Identification information returned by the network, if present. For incoming calls this is effectively CLIP. For outgoing calls this holds the dialed number or COLP if received by the audio gateway. A special `withheld` value means the remote party refused to provide caller ID and the "override category" option was not provisioned. |
| IncomingLine | `string` | `readonly`, `optional` | Called Line Identification information returned by the network. Only available for incoming calls and indicates the local subscriber number dialed by the remote party. Useful for multi-line subscriptions. |
| Name | `string` | `readonly` | Name Identification information returned by the network, if present. |
| Multiparty | `boolean` | `readonly` | Indicates whether the call is part of a multiparty call. Notifications are sent when a call becomes part of, or leaves, a multiparty call. |
| State | `string` | `readonly` | State of the current call. Possible values: `active`, `held`, `dialing`, `alerting`, `incoming`, `waiting`, `response_and_hold`, `disconnected` (object will be destroyed shortly). |
