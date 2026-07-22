<div align="center">

# mkpool

### Modern, high-performance Bitcoin mining pool software. Written in C++23.

Stratum V1 · Stratum V1 over TLS · native Stratum V2 (Noise-encrypted) · multiple coins · one codebase

[![Live pool](https://img.shields.io/badge/live-mkpool.com-f7931a?style=flat-square)](https://mkpool.com)
[![License: GPLv3](https://img.shields.io/badge/License-GPLv3-blue.svg?style=flat-square)](https://github.com/mkpool/mkpool/blob/main/COPYING)
[![C++23](https://img.shields.io/badge/C%2B%2B-23-00599C.svg?style=flat-square&logo=cplusplus)](https://github.com/mkpool/mkpool)
[![Throughput](https://img.shields.io/badge/throughput-330k%20shares%2Fs-success.svg?style=flat-square)](https://mkpool.com/blog/mkpool-vs-ckpool-benchmark)

</div>

---

## The goal

Make Bitcoin mining faster and better for everyone.

mkpool is mining pool software: a solo mining pool engine written from scratch in C++23, published openly, and proven on mainnet rather than in a whitepaper. Most pool software running in production today is over a decade old. It is slow under load, painful to operate, hard to audit, and largely undocumented. Miners point real money at it and get very little visibility in return. This is an attempt to fix that.

## Highlights

**Full Stratum V2.** Native SV2 with a Noise NX handshake, secret authority keys and signed certificates, full-block mode, and an empty-block toggle. Handled in-process, not bolted on via proxy.

**Stratum V1 over TLS.** `stratum+ssl://` served from the same binary, with SIGHUP certificate reload. No stunnel in front of it.

**Bidirectional SV2 to V1 translation.** Point a Noise-encrypted SV2 ASIC such as a Bitaxe at any plain V1 pool, or point plain V1 rigs at an SV2-only pool. Most hardware still speaks V1, so this bridges a fleet in either direction. Verified on real hardware.

**Rental marketplace ready.** Tested against NiceHash and MiningRigRentals traffic patterns, including the reconnect storms they generate, roughly 6,400 full connect cycles per second. A 30-second job rebroadcast with `clean_jobs=false` keeps strict rental proxies and farm controllers from idle-disconnecting between blocks.

**Braiins SV2 compatible.** Works with Braiins firmware over native SV2 today, tested with live hashpower.

**Performance.** Around 330,000 fully validated shares per second on an eight-core box. Median submit-to-ack of 116 µs, 602 µs at the 99th percentile, 66 MiB resident at 2,000 idle connections. Every share is validated end to end before the pool answers, so those are real figures rather than socket throughput. The benchmark kit is open source and reproducible.

**Multiple coins, one codebase.** BTC, BCH, BC2, BCH2, XEC, DGB, LTC, DOGE (merge-mined on LTC) and ZEC. SHA-256d, Scrypt and Equihash. Each is a config file away.

**Genuinely solo.** A miner's username is their payout address, and the coinbase is rebuilt per session, so a found block pays straight to the finder's wallet. The pool never holds anyone's coins.

**ASICBoost and version rolling.** BIP310 `mining.configure` negotiation with validated version masks, plus `subscribe-extranonce` and per-coin clamped suggested difficulty.

**Hardened.** Token-bucket rate limiting, automatic bans on invalid-share floods, an in-memory blacklist, stale-by-block and duplicate rejection, and a published fuzzing harness that hammers the Stratum parser.

**Operable.** RPC failover across multiple nodes with a primary-recovery watchdog, block-submit retry, a JSON control socket (`mkpool-ctl`) for live inspection and control without a restart, `SO_REUSEPORT` for near-zero-downtime restarts, and a built-in Prometheus endpoint.

## Repositories

| Repo | What it is |
|---|---|
| [**mkpool**](https://github.com/mkpool/mkpool) | The pool engine. C++23, GPLv3. |
| [**mkpool-vs-ckpool-benchmark**](https://github.com/Mecanik/mkpool-vs-ckpool-benchmark) | Reproducible benchmark kit: load generator, orchestrator, configs. |

The operational stack around the engine (database and analytics service, public REST API, website) is not part of the open release. The engine itself is published in full, for transparency.

## Status

Live on mainnet, powering [mkpool.com](https://mkpool.com), under real production load including rented hashrate. Every feature in the repository is deployed and running today.

## Get involved

Bug reports, protocol edge cases, new coin families, performance work, docs and README translations are all welcome. Read `CONTRIBUTING.md` before opening a PR, and use `SECURITY.md` rather than a public issue for anything security related.

If mkpool is useful to you, a star genuinely helps other people find it. ⭐

## Who builds it

mkpool is written and maintained by **Mecanik**, sole developer, with twenty years of commercial engineering behind it. If you open an issue, the person who wrote the code is the one who reads it.

Free and open source under GPLv3. No usage fee, no built-in donation skim.
