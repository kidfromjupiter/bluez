# org.bluez.obex.MessageAccess

## BlueZ D-Bus OBEX MessageAccess API documentation

| Field | Value |
| --- | --- |
| Version | BlueZ |
| Date | October 2023 |
| Manual section | 5 |
| Manual group | Linux System Administration |

### Interface

- **Service:** `org.bluez.obex`
- **Interface:** `org.bluez.obex.MessageAccess1`
- **Object path:** `[Session object path]`

### Methods

#### SetFolder

Signature: `void SetFolder(string name)`

Set working directory for the current session.

Possible `name` values: directory name or `..[/dir]`.

Possible errors:

- <span style="color:#c00">org.bluez.obex.Error.InvalidArguments</span>
- <span style="color:#c00">org.bluez.obex.Error.Failed</span>

#### ListFolders

Signature: `array{dict} ListFolders(dict filter)`

Returns a dictionary containing information about the current folder content.

Possible filters:

- **Offset** (`uint16`, default `0`): Offset of the first item.
- **MaxCount** (`uint16`, default `1024`): Maximum number of items.

Possible return values:

- **Name** (`string`): Folder name.

Possible errors:

- <span style="color:#c00">org.bluez.obex.Error.InvalidArguments</span>
- <span style="color:#c00">org.bluez.obex.Error.Failed</span>

#### ListFilterFields

Signature: `array{string} ListFilterFields()`

Returns all available fields that can be used in **Fields** filters.

Possible values:

`subject`, `timestamp`, `sender`, `sender-address`, `recipient`,
`recipient-address`, `type`, `size`, `status`, `text`, `attachment`,
`priority`, `read`, `sent`, `protected`, `replyto`.

Possible errors: None.

#### ListMessages

Signature: `array{object, dict} ListMessages(string folder, dict filter)`

Returns an array containing message objects found in the given subfolder of the
current folder, or in the current folder if `folder` is empty.

Possible filters:

- **Offset** (`uint16`, default `0`): Offset of the first item.
- **MaxCount** (`uint16`, default `1024`): Maximum number of items.
- **SubjectLength** (`byte`, default `256`): Maximum length of the Subject
  property.
- **Fields** (`array{string}`): Message fields. Default is all values. See
  **ListFilterFields**.
- **Types** (`array{string}`): Filter messages by type. Possible values: `sms`,
  `email`, `mms`.
- **PeriodBegin** (`string`): Start period in `YYYYMMDDTHHMMSS` format.
- **PeriodEnd** (`string`): End period in `YYYYMMDDTHHMMSS` format.
- **Read** (`boolean`): Filter by read flag (`True` for read, `False` for
  unread).
- **Recipient** (`string`): Filter by recipient address.
- **Sender** (`string`): Filter by sender address.
- **Priority** (`boolean`): Filter by priority flag (`True` for high,
  `False` for non-high).

Each message is represented by an object path implementing
[org.bluez.obex.Message](org.bluez.obex.Message.md), followed by a dictionary of
its properties.

#### UpdateInbox

Signature: `void UpdateInbox()`

Requests the remote to update its inbox.

Possible errors:

- <span style="color:#c00">org.bluez.obex.Error.Failed</span>

#### PushMessage

Signature: `object, dict PushMessage(string sourcefile, string folder, dict args)`

Transfers a message (in bMessage format) to the remote device.

The message is transferred either to the given subfolder of the current folder
or to the current folder if `folder` is empty.

Possible `args`: `Transparent`, `Retry`, `Charset`.

The returned path represents the newly created transfer. Transfer properties
are returned along with the object path to avoid a `GetProperties` call. See
[org.bluez.obex.Transfer](org.bluez.obex.Transfer.md).

Possible errors:

- <span style="color:#c00">org.bluez.obex.Error.InvalidArguments</span>
- <span style="color:#c00">org.bluez.obex.Error.Failed</span>

### Properties

| Property | Type | Access | Description |
| --- | --- | --- | --- |
| SupportedTypes | `array{string}` | `readonly` | Supported message types. Possible values: `EMAIL` (email), `SMS_GSM` (GSM short messages), `SMS_CDMA` (CDMA short messages), `MMS` (MMS messages), `IM` (instant messaging). |
