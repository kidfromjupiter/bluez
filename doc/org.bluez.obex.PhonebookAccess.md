# org.bluez.obex.PhonebookAccess

## BlueZ D-Bus OBEX PhonebookAccess API documentation

| Field | Value |
| --- | --- |
| Version | BlueZ |
| Date | October 2023 |
| Manual section | 5 |
| Manual group | Linux System Administration |

### Interface

- **Service:** `org.bluez.obex`
- **Interface:** `org.bluez.obex.PhonebookAccess1`
- **Object path:** `[Session object path]`

### Methods

#### Select

Signature: `void Select(string location, string phonebook)`

Selects the phonebook object for other operations. Call this before all other
operations.

Possible `location` values:

- `int`, `internal` (default): Store in internal memory.
- `sim{#}`: Store in SIM number.

Possible `phonebook` values:

- `pb`: Store as contact.
- `ich`: Store as incoming call.
- `och`: Store as outgoing call.
- `mch`: Store as missing call.
- `cch`: Store as a combination of incoming, outgoing, and missing calls.
- `spd`: Store as speed dials entry (internal only).
- `fav`: Store as favorites entry (internal only).

Possible errors:

- <span style="color:#c00">org.bluez.obex.Error.InvalidArguments</span>
- <span style="color:#c00">org.bluez.obex.Error.Failed</span>

#### PullAll

Signature: `object, dict PullAll(string targetfile, dict filters)`

Returns the entire phonebook object from the PSE server in plain vCard format
and stores it in a local file.

If an empty target file is given, a name is automatically generated for the
temporary file.

The returned path represents the newly created transfer. Transfer properties
are returned along with the object path to avoid a `GetProperties` call. See
[org.bluez.obex.Transfer](org.bluez.obex.Transfer.md).

Possible filters:

- **Format** (`string`): `vcard21` (default), `vcard30`.
- **Order** (`string`): `""`, `indexed`, `alphanumeric`, `phonetic`.
- **Offset** (`uint16`, default `0`): Offset of the first item.
- **MaxCount** (`uint16`, default `65535`): Maximum number of items.
- **Fields** (`array{string}`, default all): Item vCard fields. See
  **ListFilterFields**.
- **FilterAll** (`array{string}`): Filter items by fields using AND logic; not
  valid with **FilterAny**. See **ListFilterFields**.
- **FilterAny** (`array{string}`): Filter items by fields using OR logic; not
  valid with **FilterAll**. See **ListFilterFields**.
- **ResetNewMissedCalls** (`bool`): Reset new missed call items; use only for
  folders `mch` and `cch`.

Possible errors:

- <span style="color:#c00">org.bluez.obex.Error.InvalidArguments</span>
- <span style="color:#c00">org.bluez.obex.Forbidden</span>

#### List

Signature: `array{string vcard, string name} List(dict filters)`

Returns a vCard-listing array where each entry is a pair of strings containing
 the vCard handle and contact name.

Example:

- `"1.vcf"`, `"John"`

Possible filters:

- **Order** (`string`): `""`, `indexed`, `alphanumeric`, `phonetic`.
- **Offset** (`uint16`): Start offset.
- **MaxCount** (`uint16`): Maximum number of contacts.

Possible errors:

- <span style="color:#c00">org.bluez.obex.Error.InvalidArguments</span>
- <span style="color:#c00">org.bluez.obex.Forbidden</span>

#### Pull

Signature: `object, dict Pull(string vcard, string targetfile, dict filters)`

Retrieves the vCard in the current phonebook object and stores it in a local
file.

If an empty target file is given, a name is automatically generated for the
temporary file.

The returned path represents the newly created transfer. Transfer properties
are returned along with the object path to avoid a `GetProperties` call. See
[org.bluez.obex.Transfer](org.bluez.obex.Transfer.md).

Possible filters:

- **Format** (`string`): `""`, `vcard21`, `vcard30`.
- **Fields** (`array{string}`): See **ListFilterFields**.

Possible errors:

- <span style="color:#c00">org.bluez.obex.Error.InvalidArguments</span>
- <span style="color:#c00">org.bluez.obex.Error.Forbidden</span>
- <span style="color:#c00">org.bluez.obex.Error.Failed</span>

#### Search

Signature: `array{string vcard, string name} Search(string field, string value, dict filters)`

Searches entries matching the given condition and returns a vCard-listing
array, where each entry is a pair of strings containing the vCard handle and
contact name.

Possible `field` values:

- `name` (default): Search by name.
- `number`: Search by number.
- `sound`: Search by sound.

`value` is the string to search for.

Possible filters:

- **Order** (`string`): `""`, `indexed`, `alphanumeric`, `phonetic`.
- **Offset** (`uint16`): Start offset.
- **MaxCount** (`uint16`): Maximum number of contacts.

Possible errors:

- <span style="color:#c00">org.bluez.obex.Error.InvalidArguments</span>
- <span style="color:#c00">org.bluez.obex.Error.Forbidden</span>
- <span style="color:#c00">org.bluez.obex.Error.Failed</span>

#### GetSize

Signature: `uint16 GetSize()`

Returns the number of entries in the selected phonebook object that are in use
(non-NULL entries).

Possible errors:

- <span style="color:#c00">org.bluez.obex.Error.Forbidden</span>
- <span style="color:#c00">org.bluez.obex.Error.Failed</span>

#### UpdateVersion

Signature: `void UpdateVersion()`

Attempts to update **PrimaryCounter** and **SecondaryCounter**.

Possible errors:

- <span style="color:#c00">org.bluez.obex.Error.NotSupported</span>
- <span style="color:#c00">org.bluez.obex.Error.Forbidden</span>
- <span style="color:#c00">org.bluez.obex.Error.Failed</span>

#### ListFilterFields

Signature: `array{string} ListFilterFields()`

Returns all available fields that can be used in **Fields** filters.

Possible return values:

`VERSION`, `FN`, `N`, `PHOTO`, `BDAY`, `ADR`, `LABEL`, `TEL`, `EMAIL`,
`MAILER`, `TZ`, `GEO`, `TITLE`, `ROLE`, `LOGO`, `AGENT`, `ORG`, `NOTE`, `REV`,
`SOUND`, `URL`, `UID`, `KEY`, `NICKNAME`, `CATEGORIES`, `PROID`, `CLASS`,
`SORT-STRING`, `X-IRMC-CALL-DATETIME`, `X-BT-SPEEDDIALKEY`, `X-BT-UCI`,
`X-BT-UID`, `BIT-{#}`.

Possible errors: None.

### Properties

| Property | Type | Access | Description |
| --- | --- | --- | --- |
| Folder | `string` | `readonly` | Current folder. |
| DatabaseIdentifier | `string` | `readonly`, `optional` | 128-bit persistent database identifier. Example: 32-character hex `A1A2A3A4B1B2C1C2D1D2E1E2E3E4E5E6`. |
| PrimaryCounter | `string` | `readonly`, `optional` | 128-bit primary version counter. Example: 32-character hex. |
| SecondaryCounter | `string` | `readonly`, `optional` | 128-bit secondary version counter. Example: 32-character hex. |
| FixedImageSize | `bool` | `readonly`, `optional` | Support for fixed image size. `True` if image is JPEG 300x300 pixels, otherwise `False`. |
