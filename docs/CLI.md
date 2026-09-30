# Signet CLI

`signet` binds the wallet you deploy contracts from to your Signet handle, so
the contracts that wallet has deployed are attributed to you.

That binding is the thing the whole product rests on, so the CLI is deliberately
careful about what it proves and what it never touches. Two facts to hold onto
while reading the rest:

- **signet never reads your secret key.** Signing goes through your local
  `stellar` CLI (`stellar tx sign --sign-with-key`), which already owns key
  storage. signet passes it a transaction and gets a signed one back.
- **Linking takes two independent proofs.** Approving in the browser proves you
  own the handle. Signing a challenge proves you control the deploy key.
  Neither alone is enough, because accepting either on its own is exactly how
  someone would claim another developer's contracts.

## Install

```bash
npx @signet/cli link
```

`npx` fetches a small wrapper that downloads the right prebuilt binary archive for your
platform, containing both `signet` and `signet-simulator` side by side. To keep it around:

```bash
npm install -g @signet/cli
signet --version
```

Building from source needs Go and Rust:

```bash
cd cli && go build ./cmd/signet
cargo build --release --locked -p signet-simulator
```

## Prerequisite: the `stellar` CLI