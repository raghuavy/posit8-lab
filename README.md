# Posit8 Lab

A focused workspace for studying **8-bit posit arithmetic** and taking a codec from an explicit specification to reproducible tests and, eventually, RTL.

**Current stage: project setup and verification planning.** This repository does not yet contain a reference implementation, synthesizable RTL, or measured PPA results.

## Existing work

This workspace builds on public work already available elsewhere:

- [Stone Arch Silicon 8-Bit_Posit](https://github.com/Stone-Arch-Silicon/8-Bit_Posit) — the research documentation and interactive codec manual.
- [Interactive posit lab in the portfolio](https://github.com/raghuavy/about_me) — the browser implementation described in that repository's README, including decode/encode behavior and an accuracy visualization.

Those sources remain in their existing repositories. This project will hold the standalone engineering work and its reproducibility evidence.

## Initial scope

The starting configuration is **8 total bits, es = 2**, matching the existing codec documentation. The first milestone is a codec, followed by an arithmetic operation chosen after the codec is verified.

Before implementing it, record the exact posit definition/version, the bit layout, handling of zero and NaR, signed encoding, rounding and tie-breaking, endpoint behavior, and conversion interfaces. Any behavior specific to this experiment should be documented separately from claims of standard compliance.

## Roadmap and acceptance criteria

| Milestone | Deliverable | Done when |
| --- | --- | --- |
| 1. Freeze the contract | A versioned format and conversion specification | Special cases, rounding, limits, and examples are explicit |
| 2. Extract the reference model | Standalone codec with a documented invocation | It runs without the portfolio website and records provenance |
| 3. Verify the codec | Exhaustive code-space checks and independent reference vectors | Every code is covered; boundary and rounding failures are visible |
| 4. Implement RTL | Decoder/encoder modules and a self-checking testbench | RTL agrees with the reference for the documented input domain |
| 5. Add arithmetic | One operation with a specified result/rounding contract | Exhaustive operand pairs agree with an independent oracle |
| 6. Evaluate implementation | Pinned synthesis/physical-design setup and reports | Another person can reproduce area and timing from a clean checkout |

No milestone is marked complete until its artifacts and command output are committed or linked.

## Verification plan

- Decode all **256 bit patterns**, checking zero and NaR separately.
- Check encode/decode round trips for all finite representable values.
- Check signed symmetry and ordering in the domain where each property is defined.
- Test the exact midpoint and values on either side of each adjacent representable pair; verify the chosen tie rule.
- Test minimum and maximum magnitudes, underflow/overflow behavior, and all documented exceptional inputs.
- Compare against an independent implementation or independently generated golden vectors. A round trip alone can hide matching encoder/decoder bugs.
- When adding a binary operation, exercise all **65,536 operand pairs**, with explicit special-value expectations.
- Save seeds, tool versions, the source revision, expected results, actual results, and failing vectors.

## Planned layout

Create these directories as their first real artifacts land:

```text
docs/       format contract and design decisions
reference/  software model and golden-vector generation
rtl/        synthesizable modules
tb/         self-checking RTL testbenches
tests/      software and cross-model checks
scripts/    repeatable simulation and implementation commands
results/    concise reports with provenance
```

Large generated build directories belong outside the source tree. Publish selected reports or release artifacts with commands to regenerate them.

## First contribution

Start with the format contract and a small golden-vector table from an independent reference. Keep the first pull request focused. Preserve attribution when extracting existing code, and record both its source path and commit.

**Next implementation step:** bring the existing codec into a standalone reference module, then add the exhaustive codec checks above.
