0. ScholarMatch reads a student's scholarship application reason and asks Strands Decider whether it matches the Need-Based Financial Aid Scholarship policy. The decider returns eligible, needs_review, or ineligible with a confidence score, and the agent explains the verdict.

1. A scholarship reviewer that reads a student's application reason and decides whether it matches the scholarship policy. What it does: Read a student's application reason against the scholarship policy. Classify eligible / needs_review / ineligible with Strands Decider 2B. Hold low-confidence or vague reasons for a human, never auto-approve. Explain the verdict in a short sentence that references the policy.

2. The agent you are about to see is running on Amazon Bedrock AgentCore Runtime.

3. A specific, genuine financial hardship — a parent's job loss. The decider scored confidence 0.74, genuine 0.81, financial need 0.86, so the verdict was eligible, in 5.1 seconds.

4. A vague reason with no real detail — too short to judge. The decider scored confidence 0.43, genuine 0.30, financial need 0.72, so the verdict was needs_review, in 5.5 seconds.

5. A merit reason that does not match the financial-need policy. The decider scored confidence 0.68, genuine 0.68, financial need 0.24, so the verdict was ineligible, in 5.0 seconds.

6. On 26 labelled decisions, with the decider on the AWS User Group Madurai hosted API, Strands Decider was 73% accurate at a median of 4019 milliseconds, against 65% and 17781 milliseconds for a hosted open LLM asked the same questions.

7. One forward pass per application, a typed verdict with a confidence the code can branch on, and unsure cases held for a human.