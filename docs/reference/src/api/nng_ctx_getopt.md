# nng_ctx_getopt()

## NAME

nng_ctx_getopt --- get context option (deprecated)

## SYNOPSIS

```c
#include <nng/nng.h>

int nng_ctx_getopt(nng_ctx ctx, const char *opt, void *val, size_t *valszp);

int nng_ctx_getopt_bool(nng_ctx ctx, const char *opt, bool *bvalp);

int nng_ctx_getopt_int(nng_ctx ctx, const char *opt, int *ivalp);

int nng_ctx_getopt_ms(nng_ctx ctx, const char *opt, nng_duration *durp);

int nng_ctx_getopt_size(nng_ctx ctx, const char *opt, size_t *zp);

int nng_ctx_getopt_string(nng_ctx ctx, const char *opt, char **strp);

int nng_ctx_getopt_uint64(nng_ctx ctx, const char *opt, uint64_t *u64p);
```

## DESCRIPTION

> [!IMPORTANT]
> These functions are deprecated. Please see [nng_ctx_get](nng_ctx_get.md).
> They may not be present if the library was built with `NNG_ELIDE_DEPRECATED`.
> They may also be removed entirely in a future version of _NNG_.

The `nng_ctx_getopt()` functions are used to retrieve option values for
the [context](nng_ctx.md) _ctx_.
The actual options that may be retrieved in this way vary.

> [!NOTE]
> Context options are protocol specific.
> The details will be documented with the protocol.

### Forms

In all of these forms, the option _opt_ is retrieved from the context _ctx_.
The forms vary based on the type of the option they take.

The details of the type, size, and semantics of the option will depend
on the actual option, and will be documented with the option itself.

- `nng_ctx_getopt()`:\
   This function is untyped and can be used to retrieve the value of any option.
  The caller must store a pointer to a buffer to receive the value in _val_,
  and the size of the buffer shall be stored at the location referenced by
  _valszp_.\
  \
   When the function returns, the actual size of the data copied (or that
  would have been copied if sufficient space were present) is stored at
  the location referenced by _valszp_.
  If the caller's buffer is not large enough to hold the entire object,
  then the copy is truncated.
  Therefore the caller should check for truncation by verifying that the
  returned size in _valszp_ does not exceed the original buffer size.\
  \
  It is acceptable to pass `NULL` for _val_ if the value in _valszp_ is zero.
  This can be used to determine the size of the buffer needed to receive
  the object.

- `nng_ctx_getopt_bool()`:\
  This function is for options which take a Boolean (`bool`).
  The value will be stored at _ivalp_.

- `nng_ctx_getopt_int()`:\
  This function is for options which take an integer (`int`).
  The value will be stored at _ivalp_.

- `nng_ctx_getopt_ms()`:\
  This function is used to retrieve time [durations](nng_duration.md)
  (such as timeouts), stored in _durp_ as a number of milliseconds.
  (The special value `NNG_DURATION_INFINITE` means an infinite amount of time, and
  the special value `NNG_DURATION_DEFAULT` means a context-specific default.)

- `nng_ctx_getopt_size()`:\
  This function is used to retrieve a size into the pointer _zp_,
  typically for buffer sizes, message maximum sizes, and similar options.

- `nng_ctx_getopt_string()`:\
  This function is used to retrieve a string into _strp_.
  This string is created from the source using `nng_strdup()`](nng_strdup.md)
  and consequently must be freed by the caller using
  [`nng_strfree()`](nng_strfree.md) when it is no longer needed.

- `nng_ctx_getopt_uint64()`:\
  This function is used to retrieve a 64-bit unsigned value into the value
  referenced by _u64p_.
  This is typically used for options related to identifiers, network
  numbers, and similar.

## RETURN VALUES

These functions return 0 on success, and non-zero otherwise.

## ERRORS

- `NNG_EBADTYPE`: Incorrect type for option.
- `NNG_ECLOSED`: Parameter _s_ does not refer to an open socket.
- `NNG_EINVAL`: Size of destination _val_ too small for object.
- `NNG_ENOMEM`: Insufficient memory exists.
- `NNG_ENOTSUP`: The option _opt_ is not supported.
- `NNG_EWRITEONLY`: The option _opt_ is write-only.

## SEE ALSO

[nng_strdup()](nng_strdup.md),
[nng_strfree()](nng_strfree.md),
[nng_duration](nng_duration.md),
[nng_ctx](nng_ctx.md),
[nng_options](nng_options.md)