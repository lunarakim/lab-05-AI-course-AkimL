# Lab 05 — Conclusion

<!--
Read this before you write.

This file is graded by your instructor, by hand, and it is worth 6 of the 15 points.

- Type it yourself. Do not use a chatbot, an AI assistant, an AI autocomplete or a "rewrite my
  text" tool for any part of it, not even for grammar. English that is plain and a little
  clumsy but yours is better than polished text that is not.
- Use your own numbers. Every number you quote must be one your own notebook printed. Copy
  the number exactly as it appeared. A number your notebook did not print is marked as wrong.
- Answer in your own words, about 80-200 words per question. A short answer that uses your
  numbers beats a long answer that could have been written by anyone.
- Delete these comment lines if you like. Keep the three headings and the last line.
-->

## 1. Which layer changed your results the most?

Give the numbers before -> after for that layer (correct / wrong / no answer, out of 20) and say
whether it changed what the agent *can do* or only what it *admits*. If no layer changed the
number of correct answers, say so and explain what the layers did change instead.

Layer 1 changed the results the most. Before to after of 24 questions, bare was 16 correct/ 4 wrong/ 4 no answer. Layer 1 was 18/6/0. Correct went up from 16 to 18, and no answer went down to 0. But wrong also went up from 4 to 6. It changed what the agent can do. In the bare loop, all four alias questions q21-q24 stopped at the first tool error. With layer 1, the  error go back to the model. The model called list_regions and tried again. q23 and q24 became correct, but q21 and q22 became wrong. 
Layer 2 changed nothing 18/6/0 the same as layer 1. Layer 3 only changed 18/6/0 to 18/4/2. Correct stayed 18, two wrong answers q18 and q22 became no answer.

## 2. One failure the full harness did not fix

Pick one question that the full harness (layer 3) still gets wrong or refuses. Give its id
(q01 ... q20). Open its trace in the notebook: at which step did it go wrong, and what did
the model do instead? Name one thing you would add to the harness, and what it would cost
(extra model calls, tokens, time, or a new way to fail).

I choose q21, "What was the unemployment rate in Nur-Sultan in 2022?" Layer 3 answered 3.9% and this is wrong. In the trace, step 1 was get_series with Nur-Sultan. The tool said Unknown region. In step 2 the model called list_regions. It went wrong in step 3. The model used Almaty city instead of Astana and then it answered 3.9%. The harness  did not see this problem. The number 3.9 is really in a tool output, so the grounding check said OK. A number from a tool is not always the right number. I would add one more check: the region in the last tool call must be the same place as in the question. For example, one more model call: "Is Almaty city the same place as Nur-Sultan?" The cost is more model calls and more tokens. I did not measure how many. It can also fail in a new way, because the checker is the same small model and it can say yes to a wrong region.

## 3. Workflow or agent for the monthly ranking?

Dala Insight wants to publish, every month, "which region is highest and which is lowest on one
indicator in one year". Would you ship the fixed workflow or the agent? Use the accuracy, the
number of model calls, the tokens and the time you measured in Part 1. Then say in one sentence
what kind of question would make you choose the other one.

I would use the fixed workflow. The monthly question always has the same shape, one indicator, one year, highest ans lowest. In Part 1, on the 15 fixed-shape questions, the workflow got 13 correct, 0 wrong and 2 no answer. It used 15 model calls, 3852 tokens and 17 seconds. The bare agent got 9 correct, 2 wrong and 4 no answer. It used 26 model calls, 19835 tokens and 56 seconds. So the workflow was better in every number I measured. The workflow failed only on q21 and q22, the alias questions. A ranking question has no region name in it, so this problem does not happen. The agent was wrong on q9 and q11, both lowest questions, and the workflow got them right. This is only 15 questions, so the proof is small. I would choose the agent for a question where the steps are not known before, like a percent change or a region with a strange name.

---

I wrote this file myself, without any AI tool.
