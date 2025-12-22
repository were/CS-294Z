---
description: Run debate-based slide and presentation review using multiple models.
allowed-tools: Read, Write
---

# Debate-based Slide and Presentation Review

This skill facilitates the review of slides and presentations through a debate format using multiple AI models.
Particularly, an external model (like Codex-5.2) is preferred for slide analysis, as the master model is from
Claude family --- using an external model can fully avoid cached knowledge about the slides to give fresh ideas.

## CLI Tool Usage

### Codex

It is highly preferred to use codex to avoid cached knowledge about the slides.
```bash
codex exec -m gpt-5.2-codex \
  -s read-only \
  --enable web_search_request \
  -c model_reasoning_effort=xhigh \
  -o CODEX_OUTPUT.txt \
  < prompt.txt
```

**Key points:**
- `-s read-only`: Sandbox prevents all file writes
- `--enable web_search_request`: Web search is a **MODEL feature**, not network access
- **Important**: `sandbox_read_only.network_access` is NOT a valid config option. Only `sandbox_workspace_write` supports network configuration.
- Output: `-o` flag captures output to file

### Claude Code

If codex is not available, you can use the Claude model instead:
```bash
claude -p --model opus \
  --tools "Read,Grep,Glob,WebSearch,WebFetch \
  -permission-mode bypassPermissions \
  < prompt.txt > CLAUDE_OUTPUT.txt
```

**Key points:**
- `--tools`: Restricts available tools to read-only + web
- `--allowedTools`: Auto-approves the same tools
- `--permission-mode bypassPermissions`: Autonomous operation
- Output: stdout redirected to file by script

## Prompt Template

You are an **independent reviewer to the given slides and presentations**.
You are going to check: 1) If the flow of the presentation is fluent and logical;
2) If the slide wording is clear and concise and gently guides the audience through
the content; 3) If the visual design is clear and appealing.

The slides are in Markdown format so that it can be easily rendered by Marp,
and LLMs can easily parse and understand the content. If needed, you can also read
other slides within this project directory to get more context and check if all
points mentioned in the slides are consistent.

NOTE: This is a slide for 1.5 hours class instruction for junior or senior undergrad or graduate students.
1. Make sure the length of the content is appropriate for the time duration;
2. Interactions should also be considered for the class instruction;

### Input Slides

{{Input Slides Content Here}}

### Host View

This is from the host model's view of this debate:

```
{{Host Model Debate Input Here}}
```

### User View (Optional)

```
{{User View Input Here}}
```

Do not get confused with user's decision and user's view of the debate.
User's view is to claim some confusing point the user is gonna say during the presentation,
but not reflected in the slides.


### Steps

1. **Initial Review**: Read through the slides and provide an initial review based on the criteria mentioned above.
2. **Flow Review**: Check if the flow of the presentation is fluent and logical. If the transitions between the slides make sense.
3. **Fact Checking**: If the slides is trying to convey some knowledge or factual information, check if the facts are accurate and up-to-date to the best of your knowledge. If necessary, use web search to verify the facts.
4. **Debate Simulation**: Look at the host's view and see if it makes sense. If it fully considered the users' clarifications.
5. **Final Recommendations**: Based on the debate, provide a final set of recommendations for improving the slides and presentation.
   1. **Rate the Quality**: Finally, rate the overall quality of the slides on a scale from 1 to 10, where 1 is poor and 10 is excellent. Through the following dimensions:
      - Clarity of Content
      - Length of the Content
      - Visual Design
      - Language and Wording
      - Engagement and Interaction
      - Overall
    2. **Summary of Changes**: Provide a concise summary of the key changes that should be made to improve the slides.
      - Specifically mention which line of the Markdown file needs to be changed, and what the new content should be added.