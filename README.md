# Single or Multi-Agent? Analysis structure

[![GitHub stars](https://img.shields.io/github/stars/AlexFCox/single-vs-multi-agent-protocol?style=flat&logo=github)](https://github.com/AlexFCox/single-vs-multi-agent-protocol/stargazers)
[![GitHub forks](https://img.shields.io/github/forks/AlexFCox/single-vs-multi-agent-protocol?style=flat&logo=github)](https://github.com/AlexFCox/single-vs-multi-agent-protocol/network/members)
[![License](https://img.shields.io/github/license/AlexFCox/single-vs-multi-agent-protocol?style=flat)](LICENSE)
[![Last commit](https://img.shields.io/github/last-commit/AlexFCox/single-vs-multi-agent-protocol?style=flat)](https://github.com/AlexFCox/single-vs-multi-agent-protocol/commits)
[![Status: preregistered protocol](https://img.shields.io/badge/status-preregistered%20protocol-blue?style=flat)](#paper)
[![Data: none yet](https://img.shields.io/badge/data-none%20yet-lightgrey?style=flat)](#how-to-use-the-workbook)
[![Excel | LibreOffice](https://img.shields.io/badge/Excel%20%7C%20LibreOffice-no%20macros-217346?style=flat&logo=microsoftexcel&logoColor=white)](#requirements)
[![Benchmarks](https://img.shields.io/badge/benchmarks-%CF%84--bench%20%C2%B7%20WorkBench%20%C2%B7%20CRMArena--Pro-informational?style=flat)](#design-in-brief)
[![ZHAW](https://img.shields.io/badge/ZHAW-AF%26E%20Projekte%20HS26-0064A6?style=flat)](https://www.zhaw.ch/en/sml/)

This repository holds the analysis workbook for a pre-specified study protocol. The protocol compares the reliability of three LLM agent architectures under identical conditions:

- **S1:** a single agent.
- **M2:** a centralised supervisor with worker agents.
- **M3:** M2 plus a dedicated critic.

The workbook lays out every data point the study collects and every result derived from it. The primary outcome is pass^k across repeated runs. Secondary outcomes are success rate, cost per successful task and MAST failure profiles. All calculations are pre-built as formulas.

> **Status:** template only. The study has not been run and the workbook contains no real data. Example rows are fictitious and marked (see below).

## Paper

Caduff, F., Camenisch, G.-L., Cox, A., Forster, M., & Reimann, P. (2026). *Single or Multi-Agent? A Structured Literature Review and Pre-Specified Protocol for Attributing Agent Reliability to Coordination Structure.* AF&E Projekte HS 2026, Institute of Business Information Technology (IWI), Zurich University of Applied Sciences (ZHAW). Supervision: Mike Krey.

The workbook implements Table B1 (Appendix B) of the paper. Section numbers below refer to the paper.

## Contents

| File | Description |
| --- | --- |
| `Proposed_Analysis_Structure.xlsx` | Analysis workbook (11 sheets, about 21,000 formulas, no macros) |

## Design in brief

| | |
| --- | --- |
| Factor | Coordination architecture: S1, M2, M3 (Section 5.1) |
| Held constant | Base model and version, tools and tool descriptions, task prompt, total token budget across all agents, temperature, seed policy, user simulator, evaluation script |
| Benchmarks | τ-bench, WorkBench, CRMArena-Pro (Section 5.2) |
| Primary outcome | pass^k per arm and benchmark; each task run exactly k times per arm |
| Tests | Paired contrasts S1→M2 and M2→M3 per benchmark, exact McNemar test, Holm correction over six tests (Section 5.3) |
| Failure coding | MAST taxonomy (Cemri et al., 2025), misalignment modes split into agent–agent (A) and agent–user (U); double-coded sample, Cohen's κ ≥ 0.70 before full coding (Section 3.3, Appendix A) |
| Propositions | P1–P4 tested, P5 exploratory (Section 5.4, Table 4) |

## How to use the workbook

Work through the sheets in this order:

| Step | Sheet | When | What to do |
| --- | --- | --- | --- |
| 1 | Setup | Before the first run | Fill and freeze all parameters. These are model, temperature, seeds, user simulator, token budget, k, number of workers, critic revision limit, α and minimum detectable difference |
| 2 | Tasks | Before the first run | List every task and count its sub-goals (input for P5) |
| 3 | RunLog | During the runs | One row per run, written by the harness. Infrastructure failures are marked and repeated; budget overruns count as failures |
| 4 | Primary | After the runs | Calculated: success rate, pass^k, paired contrasts. Paste the bootstrap confidence intervals from the analysis script |
| 5 | Cost | After the runs | Calculated: tokens, tool calls and optional USD per successful run, ratio to S1 |
| 6 | Agreement | Before full coding | Two coders code the pre-declared sample; continue only if κ reaches the threshold |
| 7 | FailureCoding | After agreement | One primary MAST mode per failed run, plus A/U for misalignment modes |
| 8 | FailureProfile | After coding | Calculated: failure shares per arm (Table A2) |
| 9 | Propositions | After all results | Record the observed result and verdict for P1–P5 |

**Colour legend.** Yellow cells are required input and light blue cells are optional input. Grey cells are calculated, so do not type in them. Orange cells hold values pasted from the external analysis script.

**Example data.** Rows whose Run ID starts with `EX-`, and the two retail tasks in *Tasks*, are fictitious and shown in italics. Delete them and reset k in *Setup* before the first real run.

**Built-in checks.** The workbook flags three kinds of problem:
- *Tasks*: any task without exactly k counted runs.
- *FailureCoding*: unknown Run IDs, and successful runs entered as failures.
- *FailureCoding*: misalignment modes coded without A/U.

## Requirements

Microsoft Excel 2010 or later, or LibreOffice Calc 7 or later. No macros, add-ins or dynamic-array functions are used.

## Planned additions

Once the study runs, this repository will also hold the harness scripts, configurations, exact benchmark and model versions, and raw execution traces (Section 5.3). Anyone can then rerun the comparison on another model or add an arm of their own.

## Licence

[CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). You may reuse and adapt the workbook with attribution to the paper above.
