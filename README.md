# Backstop Study 2.0
### Benchmarking Agentic Cost-Escalation Guardrails
#### A Redlynr Validation Study — HDGForge Labs

**Backstop** is HDGForge Labs' validation study for [Redlynr](https://redlynr.com), a per-request guardrail API for LLM agent chains.

The study asks a practical question: **does Redlynr stop the runaway patterns it claims to stop without intervening on legitimate traffic?**

## Study at a glance

- **1,568 trials**
- **28 scenarios** across **10 domains**
- **4 conditions**
- **3 models** across **2 providers**
- GPT-5.6 Luna — 560 trials
- GPT-5.6 Terra — 448 trials
- Claude Haiku 4.5 — 560 trials

The four conditions were:

| Condition | Description |
|---|---|
| A | No guardrail |
| B | Naive fixed-window rate limiter |
| C1 | Redlynr factory defaults |
| C2 | Redlynr tuned policy |

## Primary result — cost escalation

Cost escalation was the study's primary statistically powered target.

| Condition | Cost-escalation TPR | Control FPR |
|---|---:|---:|
| C1 — Redlynr factory defaults | **100% (98/98)** | **0% (0/84)** |
| C2 — Redlynr tuned | **100% (98/98)** | **0% (0/84)** |
| B — Naive rate limiter | 66.3% (65/98) | 48.8% (41/84) |
| A — No guardrail | 0% (0/98) | N/A |

For C1/C2, the Wilson 95% lower bound on the observed 100% cost-escalation TPR is **96.3%**. The Wilson 95% upper bound on the observed 0% control FPR is **4.4%**.

Median Redlynr decision latency was approximately **6.4 ms**, with no statistically significant variation across the tested models.

## Loop detection

Only loop scenarios that failed to self-resolve in the no-guardrail condition were counted as genuine incidents. Fourteen trials met that criterion. Redlynr caught **14/14** under both C1 and C2.

The study does **not** claim that every evaluated incident category was independently validated at the same statistical power as cost escalation. Retry-storm and other categories in which models self-resolved are discussed as behavioral findings and limitations in the full report.

## What the study found before the final run

The mechanism-first validation process surfaced four genuine findings before Phase 2 data collection:

1. **Cost accumulator commit bug** — `slow_down` decisions did not commit the accumulator to Redis, leaving later steps with a stale baseline. Corrected before Phase 2.
2. **Subagent stop propagation gap** — a stop inside a subagent did not terminate the parent orchestration loop. Corrected before Phase 2.
3. **Batch-loop harness defect** — the harness continued processing a parallel batch after a stop. This was a harness issue, not a Redlynr product defect. Corrected before final data collection.
4. **Check-order inversion** — a step-cap warning could be returned before a warranted cost hard stop. Corrected before Phase 2.

All 1,568 trials reported in the final dataset were collected after the relevant corrections.

## Methodology

Backstop used a two-phase design.

**Phase 1 — validation gate.** Four direct test suites had to pass at their required counts before live model inference was authorized: 80 mock trials, 48 behavioral trials, 54 adversarial trials, and 9 batch-cost-fix validation trials.

**Phase 2 — full pilot runs.** Twenty-eight scenarios across ten domains were evaluated under all four conditions against the three models.

Core methodological principles included:

- mechanism-first verification before inference spend;
- a mandatory pre-spend validation gate;
- per-tenant isolation for history-dependent state;
- explicit disclosure of study amendments;
- retention of study history rather than silent rewriting.

## Reports

- [`reports/Backstop-executive-summary.pdf`](reports/Backstop-executive-summary.pdf) — executive summary
- [`reports/Backstop-2-Guardrail-Benchmark-2026.pdf`](reports/Backstop-2-Guardrail-Benchmark-2026.pdf) — full technical report
- [`reports/Backstop-executive-summary.docx`](reports/Backstop-executive-summary.docx) — editable executive summary
- [`reports/Backstop-2-Guardrail-Benchmark-2026.docx`](reports/Backstop-2-Guardrail-Benchmark-2026.docx) — editable technical report

The repository also contains the study objectives, methodology documentation, and scenario definitions. Note that `02-methodology.md` records the earlier study design; where it differs from the completed study, the final technical report is authoritative.

## Scope and interpretation

The strongest validated claim from Backstop 2.0 is specific:

> Redlynr's cost-escalation guardrail stopped the cost-escalation incidents in the evaluated sample and produced no observed false stops on the evaluated legitimate controls across GPT-5.6 Luna, GPT-5.6 Terra, and Claude Haiku 4.5.

Observed sample rates are not guarantees of future production performance. The full report provides Wilson confidence intervals, limitations, self-resolution behavior, amendments, and category-specific interpretation.

## Related

- [Redlynr](https://redlynr.com) — product under study
- [Basanos](https://github.com/HDGForge-Labs/basanos) — ReliAgent validation program
- [ReliAgent](https://reliagent.net) — agent tool-call reliability monitoring
- [HDGForge](https://hdgforge.com) — HDGForge

---

*Backstop is an HDGForge Labs validation study. If you find an error in the published materials, open an issue.*

*HDGForge Labs · HD Gregory LLC · 2026*
