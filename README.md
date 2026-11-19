# AgentFail

**Demystifying the Lifecycle of Failures in Platform-Orchestrated Agentic Workflows**

This repository hosts **AgentFail**, the dataset released with the paper. It is a collection of real-world failure cases from agentic workflows built on low-code orchestration platforms.

## Overview

Low-code platforms such as Dify, Coze and n8n make it fast to build multi-agent systems. Their failures are hard to diagnose. A failure propagates across heterogeneous nodes through natural-language messages, tool calls and dynamic control logic. Fixing it may mean rewriting a prompt or restructuring the workflow, not just editing code.

The paper studies failures along their whole **lifecycle**:

1. **Error exposure**: the wrong output the user sees.
2. **Failure attribution**: the workflow node that is actually responsible.
3. **Root cause analysis**: why that node failed.
4. **Repair**: the fix that resolves the failure.

## The AgentFail Dataset

- **449 failure cases** from **12 workflows** on three platforms: Dify, Coze and n8n.
- 435 cases come from our own runs and 14 from community-reported failures that we could reproduce.
- The workflows were run on 2,147 test inputs in total. Public benchmarks (HumanEval, SWE-bench, TravelPlanner, CRAG, ninja-x-deepresearch) and hand-crafted inputs were used.
- Tasks cover code generation, program repair, product QA, travel planning, deep research, market research, RAG, automated design and task planning.
- Workflow structures cover serial, parallel, branching, looping and hybrid.
- Failure cases were labeled through multi-round expert annotation to reach consensus.

Each instance contains:

| Element | Description |
|---|---|
| Query | The user input from the test case |
| Failure log | The full execution trace of the failed run |
| Workflow & configuration | Node graph, plus each node's name, prompt, code and tool settings |
| Annotations | Root failure location, root-cause label, an explanation, and the repair strategy |

### Repository layout

```
coze/
  dataset/        # test inputs for the Coze workflows (QA, industry, market, search, travel)
  output_fail/    # failure traces and labels
dify/
  output_fail/    # failure traces, labels/ and location/ annotations (APR, QA, code, research, travel)
n8n/
  google_rag_workflow_traces/
  poster_execution_traces/
  todoist_mailbox_workflow_traces/
```

## Main findings

- **Node types.** LLM & Agent nodes account for over half of failure root locations (54%). Logic & control nodes and knowledge nodes come next. Code & template nodes are only a small share, so failure patterns differ from traditional software.
- **Root-cause taxonomy.** The paper defines 16 root-cause categories in three levels, each mapped to a repair strategy:
  - **Agent-level (F1.1–F1.7)**: tool or action planning error, response format error, response content deviation, knowledge or reasoning limitation, poor prompt design, language or encoding issue, tool invocation or KB retrieval error.
  - **Structure-level (F2.1–F2.7)**: missing input verification, unreasonable node dependency, loops and deadlocks, faulty conditional judgment, improper task decomposition, context conflict, cross-agent tool or interface mismatch.
  - **Platform-level (F3.1–F3.2)**: network and resource fluctuation, service unavailability.
- **Most frequent causes.** Agent-level causes dominate. The most common are knowledge and reasoning limitations (F1.4) and poor prompt design (F1.5).
- **Automated attribution.** Our re-rollout-based attribution framework (trajectory analyst, oracle generator and counterfactual replacement) reaches 65.8% accuracy.
- **Automated root-cause identification.** Accuracy is 9.9% with no guidance. It reaches 45.9% when both the failure location and the taxonomy are given.
- **Automated repair.** A four-role repair team (analyst, architect, implementer, validator) reaches a 66.1% repair success rate with the location and root cause given, and introduces new failures in only 2.8% of cases. With neither, success is 16.4% and the new-failure rate is 13.2%.
- **Repair difficulty.** Agent-level causes are relatively easy to repair. Structure-level causes (control logic, node dependencies, task decomposition) are harder. Nodes with deterministic behavior (code, template) are easier to repair than LLM & Agent and tool nodes.

The paper also gives design guidelines for developers: clear role specification and modular prompts, explicit input/output validation, selective checks and fallbacks on high-risk nodes, and progressive workflow design.

