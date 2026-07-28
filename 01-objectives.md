# The Backstop Study — Objectives

**Study name:** The Backstop Study
**Conducted by:** HDGregory LLC / the Redlynr team
**Disclosure:** This is a vendor-conducted field trial. HDGregory LLC designed, built, and
ran this study. All methodology, scenario prompts, and raw results are published in full so
that any technically literate reader can evaluate the claims independently.

---

## What this study measures

The Backstop Study is a disclosed, vendor-conducted field trial that runs real Claude-driven
multi-agent scenarios against four conditions and measures whether Redlynr actually prevents
realistic runaway-agent incidents — at what cost in added latency, and with what false-positive
rate against legitimate heavy use.

This study is **not** a substitute for the `redlynr-benchmark` correctness suite. That suite
proves "Redlynr's code does what its own contract claims." This study instead runs real,
non-deterministic Claude agent behavior through Redlynr and measures real outcomes — the
evidence that would actually mean something to a prospective customer deciding whether to pay
for this product.

---

## Research questions

1. **Catch rate:** Does Redlynr actually stop or meaningfully slow runaway-agent trajectories
   before they cross a pre-defined damage threshold?
2. **Comparative value:** Does Redlynr's added behavioral sophistication outperform a free,
   commodity rate limiter? By how much, and on which scenario classes?
3. **Tuning impact:** Does a tuned Redlynr policy meaningfully outperform factory defaults,
   and at what cost in false-positive rate?
4. **False-positive rate:** Does Redlynr wrongly throttle legitimate heavy use at a
   meaningful rate?
5. **Latency overhead:** What is the actual per-call latency cost of routing through Redlynr
   vs. a naive limiter vs. no guardrail?

---

## What this study is not

- Not a comparison against other named commercial guardrail products.
- Not a claim of statistical significance in the strict inferential sense for the pilot batch
  — pilot results are directional only. The full run is designed to support Wilson-interval
  confidence intervals.
- Not a substitute for `redlynr-benchmark`. That suite still governs whether Redlynr's code
  is correct. This study measures whether correct code produces good real-world outcomes.

---

## Credibility commitments

A vendor testing its own product is not disqualifying, but it requires doing three things
honestly. This study commits to all three from the start:

1. **Full disclosure.** Every published output states plainly that HDGregory LLC / the
   Redlynr team designed and ran this study. No implied third-party independence.
2. **Full methodology transparency.** Every scenario prompt, every condition's exact
   configuration, every metric's exact definition, and every raw trial result is published in
   full. A skeptical reader with their own Anthropic API key should be able to reproduce this
   study from the published methodology and data alone.
3. **Report the real numbers, including unfavorable ones.** If Redlynr has a meaningful
   false-positive rate, or a naive rate limiter performs comparably on some scenario class, or
   latency overhead is higher than expected, that goes in the report exactly as measured.
