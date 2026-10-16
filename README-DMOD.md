# DMOD TLS backend fork

Based on Mbed TLS 4.1.1 and the matching TF-PSA-Crypto fork. `dmod/main`
contains unchanged generated library sources from the official release archive,
so consumers of pinned source archives do not need code generation.

With `MBEDTLS_DMOD_C`, constant TLS/X.509 description tables store text inline
instead of retaining module-relative pointers to string literals. OID sentinels
use their lengths and verification-message sentinels use their numeric codes.
Normal configurations retain upstream pointer fields and semantics.

The TF-PSA-Crypto submodule supplies typed `DMOD_GLOBAL_POINTER` slots and
explicit startup initialization for PK dispatch and ECC precomputed data.
Refer to its README-DMOD.md. The dmtls module calls these hooks before crypto
initialization and keeps the backend ABI private.

This fork requires no change to the DMOD file format, SDK or loader. Final ELF
pointer audits, native OpenSSL interoperability, and real STM32 diagnostics live
in the dmtls repository. Updating either upstream baseline requires repeating
those checks. Cryptographic algorithms and certificate verification remain the
upstream implementations.
