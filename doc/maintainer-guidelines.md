# Maintainer guidelines

This document is intended for BlueZ maintainers. It provides guidelines for
patch review and commit access.

## Rule 1: Keep the Git tree clean and linear

The `bluetooth.git`, `bluetooth-next.git`, and `bluez.git` trees are not private
playgrounds. Their history must stay clean and linear.

- No merges
- No branches
- No tags

If testing or feature work is required, clone the tree and work in your own
copy. The master trees are off limits.

To avoid accidental mistakes, set the following in global `~/.gitconfig` or
local `.git/config`:

```ini
[merge]
    ff = only
```

Violations of this rule are not acceptable. If in doubt, ask a seasoned
maintainer.

## Rule 2: Enforce clean commit messages

Commit messages must be clean and follow a consistent style.

- Wrap lines at 72 characters for readability in terminals.
- Full names and email addresses are required; no nicknames or aliases.
- Do not use Outlook-style `lastname, firstname`.

Ensure the committer uses a valid identity in global `~/.gitconfig` or local
`.git/config`:

```ini
[user]
    name = Peter Mustermann
    email = peter@mustermann.de
```

For `bluez.git`, commit messages must not include `Signed-off-by` signatures.
For `bluetooth.git` and `bluetooth-next.git`, Signed-off-by signatures are
required.

Tags like `Change-Id` from Gerrit are never acceptable; maintainers must strip
them before committing.

Violations create a mess that cannot be reversed. If in doubt, ask a seasoned
maintainer.

## Rule 3: Enforce correct coding style

Coding style follows the kernel coding style with exceptions documented in
`doc/coding-style.md`.

To prevent trivial whitespace errors, use:

```ini
[apply]
    whitespace = error
```

It can also help to run `checkpatch.pl` from a kernel tree. One way is to add
this to `.git/hooks/pre-commit` and `.git/hooks/pre-applypatch`:

```sh
exec git diff --cached | ~/src/linux/scripts/checkpatch.pl -q \
    --no-tree --no-signoff --show-types \
    --ignore CAMELCASE,NEW_TYPEDEFS,INITIALISED_STATIC -
```

This assumes a kernel tree in `~/src/linux/`.

## Rule 4: Pay extra attention to adding new files to the tree

Before adding new files:

- Check that the names are acceptable with other maintainers.
- Ensure file modes are correct.
- Verify the license and copyright headers.
- If the file must be part of the release tarball, ensure it is included by
  `make dist` (especially for documentation or non-code files).

## Rule 5: Keep the mailing list in sync with the commit process

When applying patches, send a response to the mailing list once the code is
pushed upstream. Usually this is one email per patch, but a patch set may use a
single response covering the entire set. If applying only a subset, state that
clearly.
