# Suggestions & Feature Ideas — BlockRun LLM SDK

## 1) SDK Hardening & Safety

- 🏁 **Quick Win — Add `max_price_usd` / `max_amount_micro` guardrails per call.**
  Abort signing if 402 quote exceeds caller-defined ceiling.

- 🏁 **Quick Win — Add recipient and asset allowlists.**
  Require payee + token contract match expected values for chain/environment.

- 🏁 **Quick Win — Add session budget controls.**
  `max_session_spend_usd`, `max_daily_spend_usd`, and per-model caps.

- 🛠️ **Build Next — Preflight balance check helper integrated into request path.**
  Optionally fail-fast before signing if projected cost > available balance.

- 🛠️ **Build Next — Dry-run cost estimation mode.**
  Return estimate object without signing or sending paid retry.

- 🗺️ **Roadmap — Secure key management adapters.**
  Pluggable key providers: OS keychain, cloud KMS, hardware wallet, external signer callback.

- 💡 **Explore — In-memory key lifecycle hardening.**
  Best-effort key wiping / reduced-lived signer objects.

## 2) Developer Experience

- 🏁 **Quick Win — First-class retry policy object.**
  Configurable retry-on (429/5xx/timeouts), backoff, jitter, and max attempts.

- 🏁 **Quick Win — Strongly typed `search_parameters` in public method signatures.**
  Accept `SearchParameters | dict` and validate before sending.

- 🛠️ **Build Next — Streaming chat completions (sync + async).**
  SSE/token streaming for better UX on long generations.

- 🛠️ **Build Next — Conversation session helper.**
  Automatic message history, truncation, and token budgeting.

- 🛠️ **Build Next — Rich response metadata.**
  Include latency, effective paid amount, chain, asset, and tx/payment receipt references.

- 🗺️ **Roadmap — CLI utility (`blockrun`).**
  Quick prompts, model list, balance check, spend summary, and payment diagnostics.

## 3) Observability & Debugging

- 🏁 **Quick Win — Structured logging with redaction by default.**
  Log levels (`off/basic/debug`) and redacted fields for secrets/signatures.

- 🛠️ **Build Next — Request/response middleware hooks.**
  Allow apps to attach metrics/tracing/logging integrations without patching SDK code.

- 🛠️ **Build Next — Local spend ledger (SQLite/JSON).**
  Persist paid calls, estimated vs actual cost, and status for reconciliation.

- 🗺️ **Roadmap — OpenTelemetry instrumentation.**
  Spans for preflight, 402 parse, sign, retry, and response parse.

- 💡 **Explore — Debug bundle export command.**
  Generate sanitized diagnostics package for support cases.

## 4) Multi-Chain & Payment Flexibility

- 🏁 **Quick Win — Explicit chain capability matrix in code and runtime checks.**
  Fail fast if a selected chain/payment method is not fully supported by signer implementation.

- 🛠️ **Build Next — True XRPL signing module + tests.**
  Separate signer abstraction for EVM vs XRPL flows.

- 🛠️ **Build Next — Batched/preauthorized payment modes.**
  Reduce overhead where safe (N calls under one allowance/window).

- 🗺️ **Roadmap — Additional chains and stablecoins.**
  Arbitrum/Optimism/etc with configurable token registries.

- 💡 **Explore — Escrow/deposit model with consumption receipts.**
  Alternative to per-call signing for high-throughput workloads.

## 5) Model & Feature Expansion

- 🏁 **Quick Win — Model capability descriptors in `list_models()`.**
  e.g., `supports_streaming`, `supports_tools`, `supports_vision`.

- 🛠️ **Build Next — Function/tool-calling passthrough.**
  Preserve provider-specific tool schemas safely.

- 🛠️ **Build Next — Embeddings endpoint support.**
  Common need for retrieval pipelines.

- 🛠️ **Build Next — Vision/multimodal message helpers.**
  Consistent payload builders for image/PDF/audio inputs.

- 🗺️ **Roadmap — Fallback routing strategies.**
  Try model A then B by policy (cost/latency/capability).

- 💡 **Explore — Smart router with spend-aware optimization.**
  Auto-select model by objective (budget, quality, latency).

## 6) Testing & CI

- 🏁 **Quick Win — Add adversarial unit tests for 402 parsing and payment validation.**
  Include malformed wrappers, missing fields, wrong assets, and extreme amounts.

- 🏁 **Quick Win — Add regression tests for body-wrapped `x402` handling.**
  Protect against current fallback parsing bugs.

- 🛠️ **Build Next — Property-based tests (Hypothesis) for validation + signing invariants.**

- 🛠️ **Build Next — Mock x402 contract tests.**
  Deterministic tests for 402 → sign → paid retry lifecycle.

- 🗺️ **Roadmap — Security CI stack.**
  `bandit`, `pip-audit`, Semgrep rules, dependency freshness checks.

- 💡 **Explore — Integration test spend budget enforcement in CI.**
  Hard fail if projected/actual test spend crosses threshold.

## 7) Documentation & Ecosystem

- 🏁 **Quick Win — Tighten docs to match implemented chain behavior exactly.**
  Avoid trust-damaging ambiguity around XRPL vs EVM signer paths.

- 🏁 **Quick Win — Add “Payment Safety Checklist” section to README.**
  Price caps, allowlists, idempotency recommendations, and timeout caveats.

- 🛠️ **Build Next — API reference docs (MkDocs/Sphinx) generated from type hints.**

- 🛠️ **Build Next — Cookbook examples for production patterns.**
  Cost guards, retries, streaming, circuit breakers.

- 🗺️ **Roadmap — Versioned migration guides/changelog discipline.**
  Especially for payment protocol and signature behavior changes.

- 💡 **Explore — Framework adapters.**
  LangChain/LlamaIndex wrappers that preserve payment controls.

## 8) Anything Else

- 💡 **Explore — “Safe mode” preset profile.**
  One config toggle enabling conservative defaults (strict validation, low spend limits, no auto-retry on paid ambiguity).

- 🛠️ **Build Next — Internal architecture cleanup.**
  Extract shared payment orchestration to reduce sync/async/image duplication and bug propagation.

---

## Top 5 Priorities (one engineer, two weeks)

1. 🏁 Add per-call/session spend caps + strict 402 validation (asset/payee/amount checks).
2. 🏁 Fix 402 body parsing edge cases and ship regression tests.
3. 🛠️ Implement idempotency-safe paid retry semantics and document ambiguity handling.
4. 🏁 Align docs/runtime with actual chain support (or implement missing XRPL signer path).
5. 🛠️ Add configurable retry policy and structured logging with redaction.
