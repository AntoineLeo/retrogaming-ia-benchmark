# RetroGaming AI Benchmark

**An objective benchmark for evaluating AI coding agents on retro game development, cross-platform porting, and performance optimization.**

[![License: GPL v3](https://img.shields.io/badge/License-GPLv3-blue.svg)](LICENSE)

## Overview

RetroGaming AI Benchmark is an open-source project designed to evaluate the ability of Large Language Models (LLMs) and AI coding agents to develop, port, and optimize video games for historically constrained hardware architectures.

Inspired by the technical challenges of video game development in the late 1970s and 1980s, the project focuses on environments where memory, processing power, graphics capabilities, and audio hardware impose significant constraints on software design.

The benchmark aims to go beyond evaluating whether an AI model can generate plausible code. It seeks to measure whether an autonomous agent can produce **working, technically compliant, portable, and optimized software for real retro computing architectures**.

The project is currently in its conceptual design stage. Its initial objective is to define a reproducible evaluation protocol, a validated reference game catalogue, and objective scoring methods before implementing the benchmark infrastructure.

## Objectives

The benchmark has five main objectives:

- **Measure functional development capabilities:** determine whether an AI agent can implement games that satisfy a precise, architecture-independent specification.
- **Evaluate cross-platform portability:** measure whether an agent can reproduce an existing game's behavior on a different hardware architecture, starting from the original executable binary alone.
- **Assess performance optimization:** measure whether an agent can improve execution speed, memory usage, or rendering performance without breaking the game.
- **Compare models objectively:** use automated tests, reproducible execution environments, and explicit scoring criteria.
- **Evaluate architectural understanding:** distinguish general software development skills from the ability to exploit the specific characteristics of historical hardware.

The benchmark will focus exclusively on video game development, rather than general-purpose software development.

## Target platforms

The initial scope includes eleven retro platforms spanning home computers, personal computers, and dedicated game consoles.

| Platform | Category |
|---|---|
| Amstrad CPC 6128 | 8-bit home computer |
| Commodore 64 | 8-bit home computer |
| Atari ST | 16/32-bit home computer |
| Commodore Amiga 500 | 16/32-bit home computer |
| Sega Master System | 8-bit game console |
| Sega Mega Drive / Genesis | 16-bit game console |
| Nintendo Entertainment System (NES) | 8-bit game console |
| Nintendo Game Boy | 8-bit handheld console |
| MSX | Home computer standard and platform family |
| Thomson TO7 | 8-bit home computer |
| ZX Spectrum | 8-bit home computer |

The benchmark will use modern development tools while requiring generated software to respect the technical characteristics and limitations of its historical target.

Each platform will have an appropriate toolchain, execution environment, and automated validation mechanism.

The benchmark will not assume that all platforms share the same programming language, graphics model, memory organization, or audio capabilities.

## Evaluation protocol

The benchmark is organized into three complementary phases.

### Phase 1 — Native development from a common specification

**Goal: implement the same game across as many target platforms as possible.**

The benchmark will provide a curated catalogue of minimalist games described in an architecture-independent pseudocode.

The catalogue will draw inspiration from the gameplay mechanics of classic video games from the late 1970s and early 1980s. Reference games may include simplified variants of paddle games, brick breakers, fixed-screen shooters, and maze-based games.

These will be benchmark-specific reference implementations, with precisely defined rules and expected behavior.

The pseudocode will describe the game's logic independently of its hardware implementation. It will define elements such as:

- Game state and state transitions.
- Player inputs and expected responses.
- Movement, collisions, and interactions.
- Scoring and progression.
- Winning and losing conditions.
- Deterministic test scenarios.

The AI agent must implement each game natively for the target platform, using suitable graphics, audio, and input mechanisms.

Visual appearance may differ between platforms. The primary requirement is compliance with the common functional specification, within the technical capabilities of the target hardware.

**Evaluation criteria**

The principal metric will be the number and proportion of successful platform implementations. Compilation alone will not constitute success: the generated game must execute correctly and pass the required functional tests.

Results will also be reported per game and per platform to identify platform-specific strengths and weaknesses.

### Phase 2 — Cross-platform porting from binaries only

**Goal: reproduce an existing game on another platform without access to its original source code.**

This phase evaluates reverse engineering, behavioral reconstruction, and cross-architecture porting.

The input will be a working game binary produced during Phase 1, together with the execution environment and the tools required to analyze it.

The original source code and original pseudocode will not be provided for the porting task.

The agent may use all available analysis tools, including:

- Emulation and interactive observation.
- Debuggers and execution tracing.
- Memory inspection.
- Disassembly and machine-code analysis.
- Automated input sequences and output capture.
- Freely accessible technical documentation.

The agent must infer the game's behavior from the binary and its execution, then implement a corresponding game for the destination platform.

The objective is not merely to recreate the abstract rules. The destination implementation should reproduce the observable characteristics of the source game as faithfully as possible, including its timing, visual behavior, and audio behavior where applicable.

#### Cross-platform test matrix

Every Phase 1 game binary will serve as a source for porting to every other target platform for which the benchmark defines a valid port.

With eleven platforms, a complete directed matrix contains up to **110 source-to-destination platform pairs per game**, excluding ports back to the original platform.

For example, a game originally implemented for the Commodore 64 could be ported to the ZX Spectrum, Amstrad CPC 6128, Atari ST, or any other supported destination.

The benchmark will track the success of each directed port independently. Porting from platform A to platform B and porting from platform B to platform A are separate tasks.

#### Porting score

The porting score will be a weighted combination of four dimensions:

| Criterion | Weight | Evaluation focus |
|---|---:|---|
| Functional fidelity | 40% | Rules, interactions, collisions, scoring, game states, and outcomes |
| Visual fidelity | 30% | Screen composition, object positions, shapes, colors, and visual transitions |
| Temporal fidelity | 20% | Movement speed, animation timing, responsiveness, and execution cadence |
| Audio fidelity | 10% | Sound effects, event synchronization, and audio behavior |

Each dimension will be normalized to a score from 0 to 100.

The overall porting score will be calculated as:

`Porting Score = 0.40 × Functional + 0.30 × Visual + 0.20 × Temporal + 0.10 × Audio`

For example, a port scoring 90 in functional fidelity, 80 in visual fidelity, 70 in temporal fidelity, and 60 in audio fidelity would obtain an overall score of 80.

Audio scoring must account for source games that have no audio output: the absence of audio should not automatically penalize a port that faithfully reproduces the source.

Because the source and destination hardware may differ substantially, visual and audio comparisons must use explicitly defined normalization rules. The benchmark must distinguish unavoidable hardware limitations from implementation errors.

The detailed scoring rubrics, comparison procedures, and acceptance thresholds will be defined before the corresponding tests are released.

### Phase 3 — Performance optimization

**Goal: improve a working game while preserving its expected behavior.**

This phase evaluates whether an agent can identify bottlenecks, implement optimizations, and demonstrate measurable improvements on a specific historical target.

The agent will receive a working reference implementation, a defined set of functional tests, a target execution environment, and a performance profile or baseline measurements.

It must then improve the implementation without introducing functional regressions or violating the target platform's resource constraints.

Optimization tasks may include:

- Reducing execution time or processor-cycle consumption.
- Reducing memory or video-memory usage.
- Improving rendering performance and frame consistency.
- Optimizing algorithms and data representations.
- Exploiting platform-specific hardware features.
- Balancing speed, memory consumption, and rendering quality.

#### Proposed optimization task types

**Single-objective optimization**

Improve a specified metric while respecting explicit limits on other resources. For example, reduce the time required to update a game frame without exceeding a defined memory budget.

**Constrained optimization**

Meet a measurable performance target while respecting the platform's memory and processing limits. For example, maintain the required update rate under a specified processor-cycle budget.

**Multi-objective optimization**

Find a better trade-off between execution speed, memory consumption, and rendering performance. This allows the benchmark to recognize that different implementations may be preferable under different hardware constraints.

#### Optimization validation

Every optimization task will use a baseline measured before the agent begins.

The candidate implementation must pass the functional tests before performance improvements are considered valid. Measurements will be performed under equivalent, reproducible conditions.

Where reliable processor-cycle measurements are available, they may be used directly. Otherwise, the benchmark will use a documented alternative appropriate to the platform.

Optimization scoring will be defined separately from the Phase 2 porting score. Its exact normalization, weighting, and aggregation rules remain to be specified.

## Autonomous AI agents

The benchmark is designed for autonomous AI coding agents rather than single-response code generation.

Agents will be able to inspect project files, edit code, invoke toolchains, execute programs, analyze failures, and iterate on their solutions.

Modern development tools will be permitted, including compilers, debuggers, emulators, and performance-analysis utilities.

Access to public technical documentation and Internet resources will be allowed. The benchmark will therefore measure not only implementation skills but also the agent's ability to research and apply hardware-specific information.

To ensure fair comparisons, the benchmark infrastructure should record and control, where applicable:

- The evaluated model and model version.
- The agent harness and tool configuration.
- The available execution and interaction budget.
- Toolchain and emulator versions.
- Task instructions and permitted resources.
- Execution time and relevant resource consumption.
- Test results and performance measurements.

Model capability must be distinguished from the capabilities of the agent harness and its tools. Comparisons will therefore require a clearly defined evaluation configuration.

## Automated evaluation and reproducibility

Automated testing is central to the benchmark.

Each reference game will have a validated specification and a set of test scenarios designed to verify expected behavior independently of the implementation.

Where possible, the benchmark will use deterministic input sequences, controlled initial states, repeatable execution conditions, and machine-readable output measurements.

The evaluation infrastructure should separate public task descriptions from private validation assets. Agents may freely consult technical documentation, but must not be able to inspect or modify hidden evaluation tests.

The test suite should cover both normal gameplay and edge cases, including collisions, boundary conditions, state transitions, and repeated input sequences.

Visual, temporal, and audio fidelity tests will use captured outputs and explicitly documented comparison procedures. Where exact pixel or sample equality is inappropriate because of hardware differences, normalized metrics will be used.

All benchmark runs should produce machine-readable reports containing the task identifier, source and destination platforms when applicable, configuration details, test results, scores, and relevant performance measurements.

## Reporting and model comparison

The benchmark should publish detailed results rather than relying exclusively on a single aggregate ranking.

The main results will include:

- **Native development:** successful implementations per game and platform, plus overall platform coverage.
- **Cross-platform porting:** porting success rates and fidelity scores for each source-to-destination pair.
- **Performance optimization:** measured improvements relative to the baseline, together with functional correctness and resource constraints.

A global ranking may be introduced once the scoring methodology is fully specified. Its weighting and aggregation rules must be published in advance.

Detailed per-platform results will remain available so that strong performance on one architecture cannot completely conceal weaknesses on another.

The benchmark will also preserve the evaluation configuration for each run, allowing results to be compared across models and model versions.

## Project roadmap

The project is currently at the conceptual design stage. The following roadmap describes the intended development sequence, not features already implemented.

- [ ] Define the benchmark architecture and evaluation data model.
- [ ] Specify the architecture-independent game pseudocode.
- [ ] Design and validate the initial reference game catalogue.
- [ ] Define platform-specific build and execution environments.
- [ ] Implement automated functional validation for Phase 1.
- [ ] Establish the source-to-destination porting matrix for Phase 2.
- [ ] Define objective visual, temporal, and audio fidelity metrics.
- [ ] Design and validate the performance optimization protocol.
- [ ] Implement a reproducible agent execution harness.
- [ ] Produce the first baseline results across representative models.
- [ ] Expand testing to all eleven target platforms.
- [ ] Publish benchmark reports and scoring documentation.

The initial implementation should prioritize a small number of representative platforms and a limited reference game catalogue. The framework can then be extended progressively while preserving the same evaluation principles.

## Design principles

The benchmark will follow five principles.

**Objectivity.** Results should be supported by observable execution behavior, automated tests, and documented measurements.

**Reproducibility.** Evaluation conditions and scoring rules should be explicit and repeatable.

**Hardware fidelity.** Generated software must respect the characteristics and limitations of its target architecture.

**Separation of skills.** Native development, binary-only porting, and optimization must remain distinguishable evaluation dimensions.

**Cross-platform comparability.** Common game specifications and carefully defined measurements should enable meaningful comparisons without pretending that different architectures are identical.

## Contributing

Contributions will be valuable as the benchmark design and implementation progress.

Potential areas include reference game specifications, platform toolchains, automated tests, emulator integration, fidelity metrics, performance measurement, and evaluation methodology.

Contributions should favor deterministic behavior, documented assumptions, reproducible measurements, and tests that can be independently validated.

## License

The repository currently includes the GNU General Public License version 3.0 (GPL-3.0). See [LICENSE](LICENSE) for the license text.

Any future reference binaries, game assets, documentation, or third-party tools included in the benchmark should have their licensing and redistribution requirements reviewed separately.

---

**RetroGaming AI Benchmark** aims to make AI coding capabilities measurable through the demanding constraints of historical game development: not merely generating code, but building working games, reconstructing them from binaries, porting them across architectures, and proving that optimizations deliver real improvements.
