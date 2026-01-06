# org.bluez.Telephony1

## BlueZ D-Bus Telephony Audio Gateway API documentation

| Field | Value |
| --- | --- |
| Version | BlueZ |
| Date | May 2025 |
| Manual section | 5 |
| Manual group | Linux System Administration |

### Interface

- **Service:** `org.bluez`
- **Interface:** `org.bluez.Telephony1` (experimental)
- **Object path:** `[variable prefix]/{hci0,hci1,...}/[dev](){BDADDR}/telephony#`

### Methods

#### Dial

Signature: `object Dial(string uri)`

The URI is comprised of the URI scheme followed by the Caller ID (telephone
number or username), separated by a colon.

Examples of common URI schemes can be found in IANA URI Schemes:
<https://iana.org/assignments/uri-schemes/uri-schemes.xhtml>

This initiates a new outgoing call and returns the object path to the newly
created call. See [org.bluez.Call](org.bluez.Call.md).

For HFP the URI is `tel:` followed by the telephone number.

The telephone number must be a string containing the following characters:
`[0-9+*#,ABCD]{1,80}`. The character set can contain numbers, `+`, `*`, `#`,
`,`, and letters `A` to `D`. No further number validation is performed; it is
assumed the gateway and/or network will perform further validation.

If the telephone number is an empty string, it will try to call the last dialed
number.

If an active call (single or multiparty) exists, it is automatically put on
hold if the dial procedure is successful.

Possible errors:

- <span style="color:#c00">org.bluez.Error.InvalidState</span>
- <span style="color:#c00">org.bluez.Error.InvalidArguments</span>
- <span style="color:#c00">org.bluez.Error.NotSupported</span>
- <span style="color:#c00">org.bluez.Error.Failed</span>

#### SwapCalls

Signature: `void SwapCalls()`

Swaps Active and Held calls. All calls that were Active are now Held, and all
calls that were Held are now Active.

The GSM specification does not allow swapping when Held, Active, and Waiting
calls exist. Some modems implement this anyway; whether this succeeds is
manufacturer-specific.

Possible errors:

- <span style="color:#c00">org.bluez.Error.InvalidState</span>
- <span style="color:#c00">org.bluez.Error.Failed</span>

#### ReleaseAndAnswer

Signature: `void ReleaseAndAnswer()`

Releases currently active call(s) and answers the currently waiting call. If
the current call is multiparty, all parties are released.

Possible errors:

- <span style="color:#c00">org.bluez.Error.InvalidState</span>
- <span style="color:#c00">org.bluez.Error.Failed</span>

#### ReleaseAndSwap

Signature: `void ReleaseAndSwap()`

Releases currently active call(s) and activates any currently held calls. If
the current call is multiparty, all parties are released.

Possible errors:

- <span style="color:#c00">org.bluez.Error.InvalidState</span>
- <span style="color:#c00">org.bluez.Error.Failed</span>

#### HoldAndAnswer

Signature: `void HoldAndAnswer()`

Puts the current call (including multiparty calls) on hold and answers the
currently waiting call. Calling this when a user already has both Active and
Held calls is invalid, since GSM allows only a single Held call at a time.

Possible errors:

- <span style="color:#c00">org.bluez.Error.InvalidState</span>
- <span style="color:#c00">org.bluez.Error.Failed</span>

#### HangupAll

Signature: `void HangupAll()`

Releases all calls except waiting calls. This includes multiparty calls.

Possible errors:

- <span style="color:#c00">org.bluez.Error.InvalidState</span>
- <span style="color:#c00">org.bluez.Error.Failed</span>

#### HangupActive

Signature: `void HangupActive()`

Releases active calls, including multiparty active calls.

Possible errors:

- <span style="color:#c00">org.bluez.Error.InvalidState</span>
- <span style="color:#c00">org.bluez.Error.Failed</span>

#### HangupHeld

Signature: `void HangupHeld()`

Releases held calls except waiting calls. This includes multiparty held calls.

Possible errors:

- <span style="color:#c00">org.bluez.Error.InvalidState</span>
- <span style="color:#c00">org.bluez.Error.Failed</span>

#### CreateMultiparty

Signature: `array{object} CreateMultiparty()`

Joins active and held calls together into a multiparty call. If one call is
already multiparty, the other call is added to the multiparty conversation.
Returns the new list of calls participating in the multiparty call.

There can only be one subscriber-controlled multiparty call according to the
GSM specification.

Possible errors:

- <span style="color:#c00">org.bluez.Error.InvalidState</span>
- <span style="color:#c00">org.bluez.Error.Failed</span>

#### SendTones

Signature: `void SendTones(string tones)`

Sends DTMF tones to the network. The tones have a fixed duration. Tones can be
one of: `0`-`9`, `*`, `#`, `A`, `B`, `C`, `D`.

Possible errors:

- <span style="color:#c00">org.bluez.Error.InvalidState</span>
- <span style="color:#c00">org.bluez.Error.InvalidArgs</span>
- <span style="color:#c00">org.bluez.Error.Failed</span>

### Properties

| Property | Type | Access | Description |
| --- | --- | --- | --- |
| UUID | `string` | `readonly` | UUID of the profile which the Telephony Audio Gateway is for. |
| SupportedURISchemes | `array{string}` | `readonly` | List of supported URI schemes. |
| State | `string` | `readonly` | State of the current connection. Possible values: `connecting` (RFCOMM connection in progress), `slc_connecting` (Service Level Connection in progress), `connected` (RFCOMM and SLC connected), `disconnecting` (object will be destroyed shortly). |
| Service | `boolean` | `readonly` | Network service availability. |
| Signal | `byte` | `readonly` | Network signal level from 0 to 5. |
| Roaming | `boolean` | `readonly` | Network roaming usage. |
| BattChg | `byte` | `readonly` | Battery level from 0 to 5. |
| OperatorName | `string` | `readonly`, `optional` | Operator name. |
| InbandRingtone | `boolean` | `readonly` | In-band ringtone availability. |
