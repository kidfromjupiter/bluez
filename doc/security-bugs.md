# Security bugs

BlueZ developers take security very seriously. Please report security bugs to
the BlueZ security team so they can be fixed and disclosed as quickly as
possible.

## Contact

Email the security team at `<security@bluez.org>`. This is a private list of
security officers who will help verify the report and develop and release a
fix. If you already have a fix, include it to speed up the process.

As with any bug report, the more information provided the easier it will be to
diagnose and fix. Exploit code is especially helpful and will not be released
without consent from the reporter unless it has already been made public.

Please send plain text emails without attachments where possible.

## Disclosure and embargoed information

The security list is not a disclosure channel. For disclosure, see
[Coordination](#coordination).

Once a robust fix has been developed, the release process starts. Fixes for
publicly known bugs are released immediately.

For publicly undisclosed bugs, fixes are released as soon as they are available
unless the reporter or an affected party requests a delay. Delays can be up to 7
calendar days from the start of the release process, with an exceptional
extension to 14 calendar days if the bug criticality requires more time. The
only valid reason for deferring publication is to accommodate QA and large-scale
rollouts that require release coordination.

Embargoed information may be shared with trusted individuals to develop a fix,
but it will not be published alongside the fix or on any disclosure channel
without the reporter's permission. This includes the original report and
follow-up discussions (if any), exploits, CVE information, or the identity of
the reporter.

In short, the goal is to get bugs fixed. All information submitted to the
security list and follow-up discussions remain confidential, even after an
embargo has been lifted.

## Coordination

Fixes for sensitive bugs (such as those leading to privilege escalation) may
need coordination with the private `<linux-distros@vs.openwall.org>` mailing
list so distribution vendors can prepare updates for public disclosure. Distros
typically need a few days of embargo and prefer publication Tuesday through
Thursday.

When appropriate, the security team can help with coordination, or the reporter
can include linux-distros from the start. If you include linux-distros, prefix
the email subject with `[vs]` as described in the linux-distros wiki:
<http://oss-security.openwall.org/wiki/mailing-lists/distros#how-to-use-the-lists>

## CVE assignment

The security team does not normally assign CVEs, nor are they required for
reports or fixes. CVE assignment can delay handling without adding value. If a
reporter wants a CVE identifier before public disclosure, they should contact
linux-distros (see [Coordination](#coordination)). If a CVE is known before a
patch is provided and the reporter agrees, mention it in the commit message.

## Non-disclosure agreements

The BlueZ security team is not a formal body and cannot enter into
non-disclosure agreements.
