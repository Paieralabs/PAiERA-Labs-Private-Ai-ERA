# PAiERA Labs – Memory Layer Devlog (v9.1, Stage7.33 + Stage9.3)

This repository is a **public devlog snapshot** of PAiERA’s memory layer and chat–memory contract.

PAiERA itself is a fully offline, local-first personal AI project.
The private core (engine, local models, owner bio, long-term memory, files, UI) stays **closed and offline**.
This repo only exposes a thin, safe slice of the **engineering work around memory contracts and invariants**.

---

## What this repo contains

Documents:

* `docs/memory_surface_v1.md`
  Describes the **allowed call surface** for the memory manager:

  * where `saveFacts(...)` / `forgetEntity(...)` / `getRelevantFacts(...)` are allowed to be called;
  * how the backend is forced to go through a small set of modules (server / engine_core / file_facts_apply).

* `docs/chat_memory_contract_v1.md`
  Defines the **behavioural contract** for `/api/chat` in relation to local memory:

  * canonical scenarios (age, family, city, work, etc.);
  * the rule that personal facts must come from local memory, not from model hallucinations;
  * expectations for engineers when they change engine_core, memory_manager or server.

Selftest scripts:

* `scripts/selftest_memory_public_api_invariant.sh`
  Ensures the canonical public memory API key (`__PAIERA_PUBLIC_API_STAGE5_18__`) exists and exposes
  `saveFacts` / `forgetEntity` with the expected shape.

* `scripts/selftest_memory_public_api_key_literal_guard.sh`
  Guards the **literal count** of the public API key inside `memory_manager.js` (protects against accidental duplication/removal).

* `scripts/selftest_memory_public_api_key_scope_guard.sh`
  Guards the **scope** of the public API key: checks that the key lives inside a well-defined BEGIN/END surface block and appears exactly once there.

* `scripts/selftest_chat_memory_contract_v1.sh`
  Smoke-test for `/api/chat` + memory: calls the chat endpoint and verifies that a canonical question is answered
  from local memory, not hallucinated.

> Note: these scripts are extracted from the private monorepo.
> On their own they are mostly **specification and examples**, not a runnable product.

---

## What this repo does *not* contain

* No model weights, no engine core, no routing code.
* No real user data, no owner bio, no long-term history.
* No full PAiERA backend or UI.

This is **not** a demo app or library.
It is a **transparent engineering slice** showing how we lock down memory invariants and chat–memory behaviour.

---

## Why publish this

There is a lot of “AI theatre” out there: flashy UIs with zero guarantees under the hood.
This devlog is meant to show a different approach:

* memory has a **public API with a hard key**;
* the call surface is **small and audited**;
* contracts are enforced by **selftests and guards**, not just by comments;
* chat behaviour with respect to personal facts is treated as a **formal contract**, not a side-effect.

Future devlogs (Stage10+ and beyond) will cover:

* file registry and ingest (dev-only, no fact writes),
* knowledge source map,
* knowledge-augmented chat built strictly on top of local facts.

---

## Status

* Target: **PAiERA v9.1**
* Scope of this snapshot: **Stage7.33 (memory invariants) + Stage9.3 (chat–memory contract)**.

This repo will evolve slowly and intentionally as PAiERA’s internals harden.
Think of it as a public trail of the private architecture work, not as the main codebase.
