# Frequently Asked Questions

## What kind of projects are you accepting?

We are currently in a beta status, and still working out issues in our service. At this point, we
can only commit to supporting established projects that have a critical impact on infrastructure and
user security. We will consider each request on a case-by-case basis, but some things we keep in mind are:

  - Exposure to remote attacks (e.g. libraries that are used to process untrusted input)
  - Number of users/other projects depending on this project.

We hope to relax this requirement in the future though, so keep an eye out even if we are not able
to accept your project at this time!

## Why do you use a [different issue tracker](https://bugs.chromium.org/p/oss-fuzz/issues/list) for reporting bugs in OSS projects?

Security access control is important for the kind of issues that OSS-Fuzz detects.
We will reconsider github issue tracker once the
[access control feature](https://github.com/isaacs/github/issues/37) is available.

## Why do you require an e-mail associated with a Google account?

The [issue tracker](https://bugs.chromium.org/p/oss-fuzz/issues/list) uses Google accounts for authentication. 
Note that any e-mail address [can be associated](https://support.google.com/accounts/answer/176347?hl=en)
with a Google account. 

## Why do you use Docker?

Building fuzzers requires building your project with a fresh Clang compiler and special compiler flags. 
An easy-to-use Docker image is provided to simplify toolchain distribution. This also limits our exposure
to a multitude of Linux varieties and provides a reproducible and secure environment for fuzzer
building and execution.

## How do you handle timeouts and OOMs?

If a single input to a [fuzz target](glossary.md#fuzz-target)
requires more than **~25 seconds** or more than **2GB RAM** to process, we
report this as a timeout or an OOM (out-of-memory) bug
(examples: [timeouts](https://bugs.chromium.org/p/oss-fuzz/issues/list?can=1&q=%22Crash+Type%3A+Timeout%22),
[OOMs](https://bugs.chromium.org/p/oss-fuzz/issues/list?can=1&q="Crash+Type%3A+Out-of-memory")).
This may or may not be considered as a real bug by the project owners,
but nevertheless we treat all timeouts and OOMs as bugs
since they significantly reduce the efficiency of fuzzing.

Remember that fuzzing is executed with AddressSanitizer or other
sanitizers which introduces a certain overhead in RAM and CPU.

We currently do not have a good way to deduplicate timeout or OOM bugs.
So, we report only one timeout and only one OOM bug per fuzz target.
Once that bug is fixed, we will file another one, and so on.

Currently we do not offer ways to change the memory and time limits.
