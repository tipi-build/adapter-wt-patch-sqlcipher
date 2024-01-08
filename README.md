Adapter WT patch SQLCipher
==========================

Uses CMake FetchContent to build and install Wt after patching in / replacing the build-in SQLite with SQLCipher.

Customize by setting:

```
WT_PATCH_SQLCIPHER_REPOSITORY: repository to clone Wt from
WT_PATCH_SQLCIPHER_GIT_TAG: tag or revision to use
```

In your `tipi` deps/opts.