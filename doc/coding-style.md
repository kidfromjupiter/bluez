# BlueZ coding style

Every project has a coding style, and BlueZ is not an exception. This document
captures the preferred style for BlueZ code so that changes stay readable and
maintainable.

## Kernel coding style

BlueZ follows the Linux kernel coding style rules:
<https://www.kernel.org/doc/Documentation/process/coding-style.rst>.

Use `checkpatch.pl` to validate patches:

```sh
checkpatch.pl --no-tree patch_name
```

Clean up all warnings and errors except this one:
`ERROR: Missing Signed-off-by: line(s)`. BlueZ does not use Signed-off lines,
so including them is itself an error. In some cases the 80-character limit can
be exceeded if the alternative harms readability.

## BlueZ-specific rules

Rules are mandatory (M) or optional (O). Optional rules are still preferred.

### Mandatory

#### M1: Blank line before and after an if/while/do/for statement

There should be a blank line before an `if` statement unless the `if` is nested
and not preceded by an expression or variable declaration.

Example:

```c
a = 1;
if (b) {  // wrong
```

```c
a = 1

if (b) {
}
a = 2;    // wrong
```

```c
if (a) {
    if (b)  // correct
```

```c
b = 2;

if (a) {    // correct

}

b = 3;
```

Exception: checking errors for a variable that was just set:

```c
err = stat(filename, &st);
if (err || !S_ISDIR(st.st_mode))
    return;
```

#### M2: Multiple line comment

If a comment has more than one line, start it from the second line.

```c
/*
 * first line comment  // correct
 * ...
 * last line comment
 */
```

#### M3: Space before and after operator

There should be a space before and after each operator.

```c
a + b;  // correct
```

#### M4: Wrap long lines

If a condition or function declaration is too long to fit on one line, wrap it
and indent the continued line without aligning with the body.

```c
if ((adapter->supported_settings & MGMT_SETTING_SSP) &&
    !(adapter->current_settings & MGMT_SETTING_SSP)) // wrong
```

```c
if ((adapter->supported_settings & MGMT_SETTING_SSP) &&
                !(adapter->current_settings & MGMT_SETTING_SSP))
```

```c
void btd_adapter_register_pin_cb(struct btd_adapter *adapter,
                 btd_adapter_pin_cb_t cb) // wrong
```

```c
void btd_adapter_register_pin_cb(struct btd_adapter *adapter,
                            btd_adapter_pin_cb_t cb)
```

Prefer indenting as far right as possible without exceeding 80 columns.

#### M5: Space when doing type casting

There should be a space between the new type and the variable.

```c
a = (int *)b;  // wrong
```

```c
a = (int *) b;  // correct
```

#### M6: Do not initialize variables unnecessarily

When declaring a variable, avoid initializing it unless necessary.

```c
int i = 1;  // wrong

for (i = 0; i < 3; i++) {
}
```

#### M7: Follow the order of include header elements

Order include header elements as:

- `#include`
- forward declarations
- `#define`
- enums
- typedefs
- function declarations and inline function definitions

#### M8: Internal headers must not use include guards

Any new header file with non-public API must not contain include guards.

#### M9: Naming of enums

Enums must have a descriptive name. The enum type should be small caps and not
`typedef`-ed. Enum contents should be in capitals, prefixed by the enum type
name.

```c
enum animal_type {
    ANIMAL_TYPE_FOUR_LEGS,
    ANIMAL_TYPE_EIGHT_LEGS,
    ANIMAL_TYPE_TWO_LEGS,
};
```

If values are defined, use this formatting:

```c
enum animal_type {
    ANIMAL_TYPE_FOUR_LEGS =        4,
    ANIMAL_TYPE_EIGHT_LEGS =       8,
    ANIMAL_TYPE_TWO_LEGS =         2,
};
```

#### M10: Enum as switch variable

If a switch variable is an enum, include all values in the switch body, even if
there is a `default`. This is enforced by compiler warnings so that new enum
values are not missed.

```c
enum animal_type {
    ANIMAL_TYPE_FOUR_LEGS =        4,
    ANIMAL_TYPE_EIGHT_LEGS =       8,
    ANIMAL_TYPE_TWO_LEGS =         2,
};

enum animal_type t;

switch (t) { // OK
case ANIMAL_TYPE_FOUR_LEGS:
    ...
    break;
case ANIMAL_TYPE_EIGHT_LEGS:
    ...
    break;
case ANIMAL_TYPE_TWO_LEGS:
    ...
    break;
default:
    break;
}

switch (t) { // wrong
case ANIMAL_TYPE_FOUR_LEGS:
    ...
    break;
case ANIMAL_TYPE_TWO_LEGS:
    ...
    break;
default:
    break;
}
```

If the enum comes from an external header (for example Android headers), this
rule may not apply since BlueZ cannot make assumptions about its definition.

#### M11: Always use parentheses with sizeof

The expression argument to `sizeof` should always be in parentheses.

```c
memset(stuff, 0, sizeof(*stuff));
```

```c
memset(stuff, 0, sizeof *stuff); // wrong
```

#### M12: Use void if function has no parameters

A function with no parameters must use `void` in the parameter list.

```c
void foo(void)
{
}
```

```c
void foo()  // wrong
{
}
```

### Optional

#### O1: Try to avoid complex if bodies

Prefer early return/break/continue/goto to keep `if` bodies small.

```c
if (device) {  // worse
    memset(&eir_data, 0, sizeof(eir_data));
    if (eir_len > 0)
        eir_parse(&eir_data, ev->eir, eir_len);
    ...
} else {
    error("Unable to get device object for %s", addr);
    return;
}
```

```c
if (!device) {
    error("Unable to get device object for %s", addr);
    return;
}

memset(&eir_data, 0, sizeof(eir_data));
if (eir_len > 0)
    eir_parse(&eir_data, ev->eir, eir_len);
...
```
