# Mnemo Core 🦀  
**Memory that sticks — the Rust-powered in-memory engine behind the Mnemo Stack**

[![CI](https://img.shields.io/github/actions/workflow/status/mnemo-cache/mnemo-core/ci.yml?branch=main)](…) 
[![Crates.io](https://img.shields.io/crates/v/mnemo)](https://crates.io/crates/mnemo) 
[![License: MIT](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)

![Logo](https://dummyimage.com/900x300/000000/fff.png&text=Mnemo)

---

## ✨ Why Mnemo Core?

| What you get | Why it matters |
|--------------|----------------|
| **Blazing-fast RESP 3 server** (PING/SET/GET/DEL) | Drop-in with existing Redis clients |
| **Modular bus (Rust or WASM)** | Add JSON, Search, TS… without forking the core |
| **Snapshot + AOF persistence** | Restart safe, point-in-time recovery |
| **Async Tokio runtime** | Scales across cores out-of-the-box |
| **Tangible ergonomics** | Single binary, one-line launch, zero external deps |

_Mnemo Core is the heartbeat of the **Mnemo Stack**. Keep it lean; bolt on the fancy parts later._

---

## 🚀 Quick Start

```bash
# 1. Grab it
cargo install mnemo

# 2. Fire it up
mnemo-server --port 6379

# 3. Talk to it
redis-cli -p 6379 PING         # → PONG
redis-cli -p 6379 SET hello 42 # → OK
redis-cli -p 6379 GET hello    # → "42"
````

Need Docker? `docker run -p 6379:6379 ghcr.io/mnemo-cache/core:latest`

---

## 🏗️ Architecture in 30 sec

```
┌─ Front End ─────────────────────────────────────────┐
│ RESP3 │ gRPC (opt) │ HTTP JSON (opt)               │
└────────┬──────────────────────┬────────────────────┘
         ▼                      ▼
┌───────────── Router + ACL + Auth ──────────────┐
│ Parses frames • dispatches commands • enforces │
└────────┬───────────────┬───────────────────────┘
         ▼               ▼
  Core Engine        Module Bus (dlopen/WASM)
  - Key-space        - mnemo-json
  - TTL wheel        - mnemo-search
  - Snapshots        - your-next-cool-idea
```

Everything speaks a tiny `Module` trait, so you can hot-reload new data models while the cache is live.

---

## 🔮 Roadmap

| Milestone      | ETA      | Highlights                            |
| -------------- | -------- | ------------------------------------- |
| **M0 Alpha**   | ✅ Now    | RESP 3, SET/GET, snapshots            |
| **M1 JSON**    | Aug 2025 | `JSON.SET/GET`, 80 % RedisJSON parity |
| **M2 Search**  | Sep 2025 | Full-text + HNSW vector search        |
| **M3 Cluster** | Nov 2025 | Consistent-hash sharding & Raft meta  |
| **1.0 GA**     | Jan 2026 | SLA docs, perf bench ≥ 1 M req/s/core |

Follow progress → [Projects board](https://github.com/mnemo-cache/mnemo-core/projects/1)

---

## 🤝 Contributing

1. **Fork & clone**: `git clone https://github.com/mnemo-cache/mnemo-core`
2. `make dev` spins a hot-reload server + `redis-cli`.
3. Write tests (`cargo test --all --nocapture`).
4. Run `make fmt && make clippy` until green.
5. Open a PR 🔀 — we tag first-timers with **🎉** love.

Looking to extend? Start with [`examples/module-skeleton`](./examples).

---

## 📜 License

Mnemo Core is dual-licensed under **MIT or Apache-2.0** — pick whichever suits you.

---

### Acknowledgements

Big nod to Antirez for Redis, Rustaceans for fearless concurrency, and **you** for giving Mnemo its memory.
