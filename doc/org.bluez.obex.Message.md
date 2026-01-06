# org.bluez.obex.Message

## BlueZ D-Bus OBEX Message API documentation

| Field | Value |
| --- | --- |
| Version | BlueZ |
| Date | October 2023 |
| Manual section | 5 |
| Manual group | Linux System Administration |

### Interface

- **Service:** `org.bluez.obex`
- **Interface:** `org.bluez.obex.Message1`
- **Object path:** `[Session object path]/message{#}`

### Methods

#### Get

Signature: `object, dict Get(string targetfile, boolean attachment)`

Downloads the message and stores it in the target file.

If an empty target file is given, a temporary file is automatically generated.

The returned path represents the newly created transfer. Transfer properties
are returned along with the object path to avoid a `GetProperties` call. See
[org.bluez.obex.Transfer](org.bluez.obex.Transfer.md).

Possible errors:

- <span style="color:#c00">org.bluez.obex.Error.InvalidArguments</span>
- <span style="color:#c00">org.bluez.obex.Error.Failed</span>

### Properties

| Property | Type | Access | Description |
| --- | --- | --- | --- |
| Folder | `string` | `readonly` | Folder the message belongs to. |
| Subject | `string` | `readonly` | Message subject. |
| Timestamp | `string` | `readonly` | Message timestamp. |
| Sender | `string` | `readonly` | Message sender name. |
| SenderAddress | `string` | `readonly` | Message sender address. |
| ReplyTo | `string` | `readonly` | Message Reply-To address. |
| Recipient | `string` | `readonly` | Message recipient name. |
| RecipientAddress | `string` | `readonly` | Message recipient address. |
| Type | `string` | `readonly` | Message type. Possible values: `email`, `sms-gsm`, `sms-cdma`, `mms`. |
| Size | `uint64` | `readonly` | Message size in bytes. |
| Status | `string` | `readonly` | Message reception status. Possible values: `complete`, `fractioned`, `notification`. |
| Priority | `boolean` | `readonly` | Message priority flag. |
| Read | `boolean` | `read/write` | Message read flag. |
| Deleted | `boolean` | `writeonly` | Message deleted flag. |
| Sent | `boolean` | `readonly` | Message sent flag. |
| Protected | `boolean` | `readonly` | Message protected flag. |
| DeliveryStatus | `string` | `readonly`, `optional` | Message delivery status. Possible values: `delivered`, `sent`, `unknown`. |
| ConversationId | `uint64` | `readonly`, `required` | Message conversation ID sent by the server as a unique conversation identifier. |
| ConversationName | `string` | `readonly`, `optional` | Human-readable name of the conversation. |
| Direction | `string` | `readonly`, `required` | Direction of the message. Possible values: `incoming`, `outgoing`, `outgoingdraft`, `outgoingpending`. |
| AttachmentMimeTypes | `string` | `readonly`, `optional` | MIME type of the attachment. |
