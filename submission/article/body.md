# ScholarMatch: Auto-Reviewing Scholarship Applications with Strands Decider 2B and Amazon Bedrock

By Naveensri V

ScholarMatch automates the first pass of university scholarship applications. It reads a student's free-text reason, asks Strands Decider 2B whether it matches the Need-Based Financial Aid Scholarship policy, and returns eligible, needs_review, or ineligible with a plain explanation. Vague or low-confidence cases always go to a human — the agent never auto-approves when unsure. On 13 test cases the decider was 4.4× faster than the LLM baseline.

## How it works

Built with Strands Agents SDK on Amazon Bedrock (Amazon Nova Pro) deployed to Amazon Bedrock AgentCore Runtime. The Strands Decider decision runs before the LLM writes its reply — the LLM only explains the verdict.

**Upload: submission/article/img/architecture.png**
*Architecture: student message → Strands Decider 2B (#policy) → deterministic reply → Amazon Bedrock AgentCore Runtime*

Three questions asked in one forward pass: `eligibility` (choice: eligible / needs_review / ineligible), `genuine` (yes/no: is the reason specific?), `financial_need` (yes/no: does it describe real hardship?). Low confidence or vague reasons always escalate to human review.

## Live results from the deployed agent

| Student | Reason summary | Verdict | Confidence |
|---|---|---|---|
| STU-101 | Father lost job, can't pay tuition | eligible | 0.74 |
| STU-104 | "I need financial help for my studies" | needs_review | 0.43 |
| STU-107 | CGPA 9.4, wants scholarship for research | ineligible | 0.68 |

## Measured

Decider: `strands-decider-2b-hobson-v19` · LLM baseline: `qwen3.5-4b` · 13 test cases, 26 decisions.

| | Strands Decider 2B | LLM baseline |
|---|---|---|
| Accuracy | **73.1%** | 65.4% |
| Latency p50 | **4,019 ms** | 17,781 ms |
| Cost / 1k decisions | $0.035 | $0.00 |
| Accuracy when confident | **100%** | 65.4% |

**Upload: submission/article/img/bench-accuracy.png**
**Upload: submission/article/img/bench-latency.png**
**Upload: submission/article/img/bench-cost.png**
**Upload: submission/article/img/bench-confidence.png**

Every decision the decider answered with confidence ≥ 0.7 was correct. The `eligibility` question alone scores 61.5%; the code compensates with explicit rules in `decide()` so the final action accuracy is higher.

## Reusable pattern

Copy `agent/decision.py`, swap `POLICY` and `SCHOLARSHIP` for your own, reword the three questions, and adjust the thresholds in `decide()`. Tag: `#policy`. Works for any free-text vs. prose-policy classification — eligibility screening, content moderation, intake triage.

## What I learned

Option descriptions matter most. Short labels with no descriptions gave poor `needs_review` accuracy; adding one-line definitions for each option fixed it. The decider and the LLM can disagree — `_guard_reply()` in `agent.py` ensures the reply always matches the decider's verdict.

---

Code: (add GitHub repo link) · Video: (add YouTube/Vimeo link) · Tags: #strands-decider-2b #policy

Built with the AWS User Group Madurai Strands Decider hackathon kit
