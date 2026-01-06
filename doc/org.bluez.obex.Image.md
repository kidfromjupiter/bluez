# org.bluez.obex.Image

## BlueZ D-Bus OBEX Image API documentation

| Field | Value |
| --- | --- |
| Version | BlueZ |
| Date | August 2024 |
| Manual section | 5 |
| Manual group | Linux System Administration |

### Interface

- **Service:** `org.bluez.obex`
- **Interface:** `org.bluez.obex.Image1` (experimental)
- **Object path:** `[Session object path]`

### Methods

#### Get

Signature: `object, dict Get(string targetfile, string handle, dict description)`

Retrieves the image corresponding to the handle and description (one of the
entries returned by **Properties**) and stores it in a local file.

If the `transform` description property exists, it should be set to one of the
values listed by **Properties** for this description.

If `description` is an empty dict, the native image is retrieved.

Possible errors:

- <span style="color:#c00">org.bluez.obex.Error.InvalidArguments</span>
- <span style="color:#c00">org.bluez.obex.Error.Failed</span>

#### Properties

Signature: `array{dict} Properties(string handle)`

Retrieves image properties corresponding to the handle.

The first dict entry is mandatory and corresponds to `handle` and `name` of the
image. The second dict entry is mandatory and corresponds to the native
description (`type: native`). Additional entries are optional and correspond to
variant descriptions. If the `transform` entry exists, it lists available image
transformations and should be set to one of them before using the description
as a parameter to **Get**.

Possible property values:

- **type** (`string`): Mandatory for each dict. Possible values: `native`,
  `variant`.
- **encoding** (`string`): File encoding format. Possible values: `BMP`, `GIF`,
  `JPEG`, `JPEG2000`, `PNG`, `WBMP`.
- **pixel** (`string`): Size in the form `<width>*<height>`.
- **size** (`uint64`): File size.
- **maxsize** (`uint64`): Maximum file size.
- **transformation** (`string`): Available transformations separated by space.
  Possible values: `crop`, `fill`, `stretch`.

Possible errors:

- <span style="color:#c00">org.bluez.obex.Error.InvalidArguments</span>
- <span style="color:#c00">org.bluez.obex.Error.Failed</span>

#### GetThumbnail

Signature: `object, dict GetThumbnail(string targetfile, string handle)`

Retrieves the image thumbnail corresponding to the handle and stores it in a
local file.

Possible errors:

- <span style="color:#c00">org.bluez.obex.Error.InvalidArguments</span>
- <span style="color:#c00">org.bluez.obex.Error.Failed</span>
