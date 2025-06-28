CPython extensions must be GIL-aware in order to avoid defeating threads (e.g. holding the GIL when waiting on I/O).

