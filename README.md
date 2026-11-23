# buf LSP: go-to-definition on an enum value in an option's value finds nothing

A minimal reproduction for buf's language server (`buf lsp serve`).

`opts/opts.proto` declares an enum `Level`, a message `Meta` with a
`Level` field, and two custom message options, `level` (a `Level`) and
`meta` (a `Meta`). `use.proto` sets both, each with `LEVEL_HIGH` as its
value.

`buf build` accepts the module.

## Steps

1. Open `use.proto` in an editor connected to `buf lsp serve`, run from
   this directory.
2. Go to definition on `LEVEL_HIGH` in `option (opts.level) = LEVEL_HIGH;`:
   no location.
3. Go to definition on `LEVEL_HIGH` in
   `option (opts.meta) = { level: LEVEL_HIGH };`: no location.
4. Go to definition on the option name `level`, or on the key `level` in
   the message literal: `opts/opts.proto`, lines 17 and 13, as expected.

Expected in steps 2 and 3: `opts/opts.proto`, line 9, where `LEVEL_HIGH`
is declared.

Seen with buf 1.73.0 and with buf `main` at `fe4085d` (1.75.1-dev).
