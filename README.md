# What

This project reproduces a bug happening with cargo as of `cargo 1.83.0 (5ffbef321 2024-10-29)` (and earlier):

## The bug

When running `cargo doc` incrementally, edits to files are not detected, ending up in a previous doc version.

### Reproduction

Run `cargo doc --open`.

Output is:

```
 Documenting crate1 v0.1.0 (/path/to/here/crate1)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.17s
     Opening /path/to/here/target/doc/crate1/index.html
```

Change some documentation in [src/lib.rs](src/lib.rs) (and save).

Run again `cargo doc --open`.

Output is:

```
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.00s
     Opening /path/to/here/target/doc/crate1/index.html
```

But should be the same as before. Notice the opened documentation still shows the previous content.

## Workaround

Run `cargo clean` (with `-p package` in bigger projects to avoid too expensive recompilation).
