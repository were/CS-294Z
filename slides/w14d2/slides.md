---
marp: true
theme: default
paginate: true
size: 16:9
style: |
  section {
    padding: 30px 50px;
    font-size: 36px;
  }
  section::after {
    content: attr(data-marpit-pagination) ' / ' attr(data-marpit-pagination-total);
    font-size: 18px;
    color: #555;
    position: absolute;
    bottom: 30px;
    right: 50px;
  }
  header {
    font-size: 20px;
    color: #888;
  }
  h1 {
    color: #2c3e50;
  }
  h2 {
    color: #34495e;
    border-bottom: 2px solid #3498db;
    padding-bottom: 10px;
  }
  footer {
    display: none;
  }
  .columns { display: flex; gap: 40px; align-items: flex-start; }
  .col { flex: 1; }
  pre { font-size: 20px; }
  code { font-size: 20px; }
  .small { font-size: 27px; }
  .center { text-align: center; }
---

# Hopefully the Last Lecture
## Academic Writing

Jian Weng  
CEMSE, KAUST  
Week 14, Session 2

---

# Today's Agenda

1. Why academic writing is different from technical writing
2. What an academic paper is trying to do
3. What a contribution actually means
4. The standard paper narrative
5. Academic reading and reviewing expectations
6. How to evaluate and revise a research claim

---

# Why This Topic?

Good work does not explain itself.

By the time a reader sees your paper, they usually do not see:

- your effort
- your failed attempts
- your private intuition
- your project history
- your informal motivation

They only see the argument you put on the page.

---

# Technical Writing

Technical writing is mostly about transferring operational knowledge.

Examples:

- how a system is structured
- how a process works
- how to reproduce a result
- how to use a tool
- what future maintainers must know

Good technical writing reduces future confusion.

---

# Academic Writing

Academic writing is not just documentation.

It tries to convince a reader that:

- an area matters
- a problem is important
- previous solutions are insufficient
- your idea is new and useful
- your evidence supports the claim

The paper is an argument, not a project log.

---

# A Blunt Difference

Technical writing:

- "Here is what I did."
- "Here is how it works."
- "Here is how to use it."

Academic writing:

- "Here is why this problem matters."
- "Here is why existing work is not enough."
- "Here is why this idea deserves attention."

Same work. Different reader contract.

---

# Papers Sell Ideas

This does not mean lying.

It means the paper must make the value of the idea legible.

- choose the right problem framing
- make the contribution crisp
- explain why the design is not arbitrary
- show that the result matters beyond one example

If the reader cannot see the idea, the effort does not save it.

---

# What Is a Contribution?

People often say a paper needs a contribution.

That does not mean:

- the implementation is large
- the project took a long time
- the system has many features
- the writing sounds impressive

Contribution means the work changes what the reader understands or can do.

---

# Contribution Is a Delta

A contribution is about the difference from before.

Before:

- people had a limitation
- existing methods made an assumption
- an important question was unanswered
- evidence was missing

After:

- the limitation is reduced
- the assumption is weakened
- the question is answered
- the evidence is clearer

---

# Not Just Beautiful Implementation

Beautiful implementation can support a paper.

But it is usually not the contribution by itself.

The paper should emphasize:

- what is unique compared with prior work
- what insight made the solution possible
- what changed in the problem understanding
- what others can reuse from the idea

Academic contribution is closer to insight than effort.

---

# Types of Contributions

Contributions can take many forms:

- a new method
- a new dataset
- a new measurement
- a new theory or model
- a new problem formulation
- a surprising negative result
- a cleaner explanation of a known phenomenon

Not every contribution has to be a tool.

---

# Weak Contribution Claims

```text
We built a complete system.
We implemented many features.
We carefully optimized the workflow.
```

These claims may be true.

But they do not yet explain:

- what is new
- why it matters
- what insight the reader should remember
- what evidence supports the claim

---

# Stronger Contribution Claims

```text
We show that a small amount of structured metadata can make
previously informal reproduction steps visible, auditable,
and comparable across studies.
```

This is stronger because it names:

- the mechanism
- the prior limitation
- the resulting capability
- the scope of the idea

---

# Contribution Checklist

When writing the contribution, ask:

- what was hard before this paper?
- what is possible after this paper?
- what is the smallest core idea?
- what did you sacrifice to get the result?
- what evidence makes the claim believable?

If the contribution is vague, the whole paper becomes vague.

---

# Why Academic Writing Feels Weird

Academic papers often spend a lot of space before the main idea.

That space is doing work:

- establishing importance
- narrowing the problem
- positioning against prior work
- preparing the reader for the contribution

It can feel indirect, but the structure has a purpose.

---

# The Typical Paper Flow

Most papers follow a familiar shape:

1. Why the area is important
2. What problem limits the area
3. Why prior work does not solve it
4. What solution you propose
5. What trade-off you accept
6. How you evaluate the solution

This shape is boring because it works.

---

# Academic Reading

Reading a paper is not passive.

A strong reader constantly asks:

- what is the problem?
- what is the claim?
- what is the contribution?
- what is the evidence?
- what assumption is hidden?
- what would make the claim false?

Academic reading is structured skepticism.

---

# Academic Reviewing

Reviewers read with expectations.

They are usually looking for:

- a clear problem
- a recognizable gap
- a crisp contribution
- a plausible method
- convincing evidence
- honest limitations

If the paper does not expose these pieces, reviewing becomes harder.

---

# Why Flow Matters to Reviewers

Reviewers have limited time and many papers to judge.

The standard flow helps them follow:

```text
area -> problem -> gap -> idea -> evidence -> limitation
```

When the paper follows this flow, reviewers can spend attention on judging the idea.

When the paper hides the flow, reviewers spend attention on navigation.

---

# Reading With the Flow

When reading a paper, annotate each part:

- area: why should anyone care?
- problem: what exactly is hard?
- gap: why existing work is insufficient?
- idea: what is the new contribution?
- evidence: what supports the claim?
- limitation: where does the claim stop?

This is also a good way to debug your own paper.

---

# Reviewer-Friendly Writing

Reviewer-friendly writing is not about pleasing everyone.

It means the paper makes judgment easy.

- state the claim early
- make the contribution explicit
- connect each section to the claim
- show the evidence before overclaiming
- admit the trade-off clearly

A confused reviewer usually becomes a skeptical reviewer.

---

# Step 1: Why the Area Matters

Start broader than your specific project.

The reader first needs to know:

- who cares?
- what is at stake?
- why now?
- what becomes possible if the problem is solved?
- what remains painful if it is not solved?

Importance is the doorway into the paper.

---

# Step 2: What Is the Problem?

A paper is not just "we built a thing."

It needs a problem statement:

- what is hard today?
- who experiences the pain?
- why does the pain block progress?
- why is the problem not already solved?

The problem statement narrows the broad area into a research target.

---

# Weak Problem Statement

```text
Reproducibility is important.
We built a tool to help people reproduce studies.
```

This is not enough.

- no precise limitation
- no clear target user
- no reason the approach should matter
- no measurable claim

The reader sees activity, not research.

---

# Stronger Problem Statement

```text
Many empirical studies depend on informal preprocessing steps.
When those steps are not captured, other researchers cannot tell
whether a failed reproduction comes from the method, the data,
or the missing preparation details.
```

This gives the paper something to argue.

- the limitation is specific
- the affected reader is clear
- the consequence is concrete

---

# Step 3: Why Prior Work Fails

Prior work should not be a list of random citations.

It should explain the gap:

- what existing approaches already do well
- what assumptions they make
- where they break down
- why your setting exposes a missing piece

The goal is not to insult prior work.
The goal is to locate your contribution.

---

# Related Work Is Positioning

Related work answers:

- what family does this paper belong to?
- what is closest to your idea?
- what is genuinely different?
- what evidence would change the reader's mind?

If related work is weak, the contribution feels ungrounded.

---

# Step 4: Proposed Solution

The solution section should explain the idea, not dump every detail.

Focus on:

- the key mechanism
- the design choices
- the data or conceptual model
- the workflow
- the interface between parts

This is still not a technical report.

---

# Not a Project Diary

Avoid writing:

```text
First we collected examples. Then we cleaned the data.
Then we changed the method. Then the results improved.
```

That may be useful in a lab notebook.

It is usually not useful in a paper.

The paper needs the stable idea behind those steps.

---

# What Is the Key Insight?

For every detail, ask:

- does this reveal the research idea?
- does it explain a design decision?
- does it support the main claim?
- would the paper be weaker without it?

If the answer is no, it probably belongs in notes, not the paper.

---

# Trade-Offs

Every research idea buys something by giving something up.

Examples:

- broader coverage, less depth
- stricter assumptions, cleaner results
- more automation, less interpretability
- more realism, less experimental control

A paper should be honest about the trade-off.

Hiding it makes the work look weaker, not stronger.

---

# Design Decisions

Do not only say what you chose.

Explain why:

- why this abstraction?
- why this dataset?
- why this study design?
- why this evaluation setting?
- why this comparison point?

Design decisions are where work becomes research reasoning.

---

# Detail: How Much?

Enough detail:

- reader can understand the mechanism
- reader can see why the design works
- reader can judge whether the claim is plausible

Too much detail:

- chronological debugging history
- incidental parameter choices
- internal names that do not matter
- decisions unrelated to the claim

Academic writing compresses effort into insight.

---

# Step 5: Evaluation

Evaluation answers one question:

Did the proposed solution actually support the claim?

Common parts:

- setting
- data or materials
- metrics
- baselines
- experimental procedure
- result interpretation

Do not treat evaluation as an appendix to the idea.

---

# Evaluation Setting

Readers need enough context to trust the evidence.

Depending on the paper, this may include:

- participants or samples
- data sources
- inclusion and exclusion criteria
- tools and materials
- environment
- procedure

Reproducibility starts with naming the setting.

---

# Metrics

Pick metrics that match the claim.

If the claim is accuracy:

- precision
- recall
- error rate

If the claim is usability:

- time to complete a task
- success rate
- survey score
- qualitative themes

Wrong metrics make the paper feel confused.

---

# Baselines

The baseline is the thing the reader compares against.

Examples:

- current practice
- previous best-known method
- a simple heuristic
- no intervention
- expert judgment
- random assignment

Without a baseline, improvement is just a number.

---

# Data for Design Decisions

Evaluation is not only for the final result.

It can support design decisions:

- why this scope?
- why this method?
- why this workload?
- why this population?
- why this measurement?

Good papers connect evidence back to choices.

---

# Ablation Study

An ablation study removes one mechanism at a time.

Question:

```text
How much does each part contribute to the final result?
```

Example:

- full method
- without component A
- without component B
- with a simpler variant

This helps argue that the contribution is not accidental.

---

# The Annoying Part of Ablations

Ablations can be painful in real projects.

- you need many variants
- experiments take time
- results may interact in unexpected ways
- one broken variant can distort the conclusion

But without this evidence, readers may not know which part mattered.

---

# Common Failure Mode: Too Much Text

More text does not always make the paper stronger.

Often it makes the argument harder to see.

Common causes:

- background that never returns
- claims without evidence
- details without purpose
- paragraphs that repeat the same point

Revision is often deletion.

---

# Common Failure Mode: Hidden Claim

Sometimes the real claim appears too late.

The reader should not have to guess:

- what problem you solve
- what your idea is
- why it is different
- what evidence supports it

If the central claim is hidden, the rest of the paper becomes harder to read.

---

# Better Revision Questions

Ask concrete questions:

```text
What is the main claim of this introduction?
Which paragraph does not support the claim?
Which term is undefined?
Which result is not connected to the thesis?
What can be deleted without loss?
```

Revision should sharpen the argument, not just polish the language.

---

# Paper Writing Checklist

Before submitting, ask:

- what is the one-sentence claim?
- what is the problem?
- what is the key idea?
- what is the trade-off?
- what is the strongest evidence?
- what should be deleted?

If you cannot answer these quickly, the paper is not ready.

---

# Final Takeaways

- Technical writing preserves operational knowledge
- Academic writing builds an argument
- A paper sells the importance of an idea
- Contribution means the unique insight or delta from prior work
- Readers and reviewers expect a recognizable paper flow
- Details must be compressed into insight
- Evaluation must match the claim
- Revision should make the core argument easier to see

The hard part is not writing more.
The hard part is knowing what the paper is really about.
