# Awesome LNURLcash [![Awesome](https://awesome.re/badge.svg)](https://awesome.re)

> Bearer notes on Lightning, built on plain LNURL.

**LNURLcash** ([LUD-25 draft](https://github.com/lnurl/luds/pull/301)) makes a
bearer instrument out of an ordinary
[LUD-03](https://github.com/lnurl/luds/blob/luds/03.md) withdrawRequest link,
by treating its `k1` as the asset itself:

```
lnurlw://mint.example/w?k1=<secret>&amount=<msat>
```

Whoever knows the `k1` controls the sats behind it, like a banknote. It can be
handed over offline as a QR code, rotated onto a fresh secret, split, merged,
or melted back into a BOLT-11 payment. No new endpoint and no new encoding — a
wallet that has never heard of LNURLcash sees a normal withdraw link and can
still cash it out.

## Contents

- [Specification](#specification)
- [Reference implementations](#reference-implementations)
- [Wallets](#wallets)
- [Mints](#mints)
- [Hardware](#hardware)
- [Libraries](#libraries)
- [Testing and conformance](#testing-and-conformance)
- [Related LUDs](#related-luds)
- [Understanding it](#understanding-it)
- [Contributing](#contributing)

## Specification

- [LUD-25 draft](https://github.com/lnurl/luds/pull/301) — the specification,
  under review. Comments belong on the PR.
- [The LUD index](https://github.com/lnurl/luds) — every LNURL specification.

## Reference implementations

Both by [dni](https://github.com/dni), both MIT, and both the place to look
first when a question is really about what the protocol means.

- [lnurl-mint](https://github.com/dni/lnurl-mint) — the reference service.
  Python and FastAPI, backed by lnd or cln. Mint, rotate, split, merge, melt,
  optional fees, optional offline verification, and a sunset switch.
- [lnurl-wallet](https://github.com/dni/lnurl-wallet) — the reference wallet.
  A single static page with no backend and no accounts; notes live encrypted in
  the browser. Hosted at
  [wallet.lnurlcash.com](https://wallet.lnurlcash.com).

## Wallets

- [lnurl-wallet](https://github.com/dni/lnurl-wallet) — the reference wallet
  (see above).

## Mints

- [lnurl-mint](https://github.com/dni/lnurl-mint) — the reference service (see
  above).

## Hardware

- [lnurl-vault](https://github.com/dni/lnurl-vault) — an ESP32-S3 hardware
  vault. Generates note secrets from a hardware RNG, discloses only their
  hashes until a mint confirms, and gates every plaintext export behind a
  physical button press. Works fully offline: hold both buttons to unveil a
  note as an on-screen QR code.

## Libraries

- [lnurlcash-kit](https://github.com/TheCryptoDonkey/lnurlcash-kit) —
  TypeScript. Extracted from the reference wallet's protocol layer.
- [lnurlcash-py](https://github.com/TheCryptoDonkey/lnurlcash-py) — Python,
  sync and async, with a no-I/O protocol layer for other HTTP stacks.
- [lnurlcash-core](https://github.com/TheCryptoDonkey/lnurlcash-core) — Rust,
  with UniFFI bindings for Kotlin and Swift.
- [lnurlcash-kotlin](https://github.com/TheCryptoDonkey/lnurlcash-kotlin) —
  Kotlin and JVM, over the Rust core.
- [lnurlcash-go](https://github.com/TheCryptoDonkey/lnurlcash-go) — Go.

## Testing and conformance

- [lnurlcash-conformance](https://github.com/TheCryptoDonkey/lnurlcash-conformance)
  — language-neutral test vectors, an adversarial mock mint that can be told to
  drop a connection mid-mutation or sign in the wrong byte order, and a grader
  that exits non-zero on a non-compliant service. Run these before you run real
  sats through anything.

## Related LUDs

The specifications LNURLcash is assembled from, rather than replacing:

- [LUD-01](https://github.com/lnurl/luds/blob/luds/01.md) — bech32 encoding.
- [LUD-03](https://github.com/lnurl/luds/blob/luds/03.md) — `withdrawRequest`.
  A note *is* one of these.
- [LUD-06](https://github.com/lnurl/luds/blob/luds/06.md) — `payRequest`.
  Paying one mints a note; its preimage is the secret.
- [LUD-11](https://github.com/lnurl/luds/blob/luds/11.md) — `disposable`.
- [LUD-16](https://github.com/lnurl/luds/blob/luds/16.md) — Lightning
  Addresses, including the bare-domain `_` convention.
- [LUD-17](https://github.com/lnurl/luds/blob/luds/17.md) — the `lnurlw://`
  scheme prefixes.
- [LUD-21](https://github.com/lnurl/luds/blob/luds/21.md) — `verify`. For
  LNURLcash the preimage it returns is bearer material, which changes the
  calculus considerably.

## Understanding it

The parts that are easy to get wrong, and where each is explained:

- **Who generates a replacement secret.** The wallet, never the service — see
  the `h`/`h2` section of the
  [LUD-25 draft](https://github.com/lnurl/luds/pull/301). A service-issued
  replacement has, structurally, already been seen by that service.
- **Ambiguous mutations.** A rotate that times out may already have burned the
  input. [lnurlcash-kit's README](https://github.com/TheCryptoDonkey/lnurlcash-kit#the-five-things-that-will-cost-you-money)
  works through what to do about it.
- **HTTP retries.** Every mutation is a GET, and GET is meant to be idempotent.
  This one is not. The
  [conformance vectors](https://github.com/TheCryptoDonkey/lnurlcash-conformance)
  name it as a scenario, having caught it in two implementations.
- **Melt semantics.** `OK` means the payment is in flight, not that the note is
  spent — see lnurl-mint's README on the reserved/`pending` state.
- **Offline verification.** The exact signing scheme, including which end of
  the signature carries the recovery id, is in
  [lnurlcash-conformance's signature vectors](https://github.com/TheCryptoDonkey/lnurlcash-conformance/blob/main/vectors/signature.json).

## Contributing

Additions welcome, particularly implementations in languages not listed here,
and wallets or mints in the wild. Open a pull request; see
[CONTRIBUTING.md](CONTRIBUTING.md).

Entries need a one-line description saying what the thing *is*, not what it
aspires to be. Working software before announcements.
