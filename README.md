# Agentic Programming: Specs, Structure, and Reliable AI-Assisted Development

This repository contains course materials for teaching **AI-assisted programming as software engineering**, not as prompt-only "vibe coding."

The compiler project is a vehicle for practice, but the core lesson is broader:
- AI can generate code quickly, but speed without structure lowers reliability.
- Most failures come from missing engineering awareness: unclear architecture, weak tests, vague goals, and ad hoc workflows.
- You do not need to memorize every detail of a codebase, but you must understand its high-level structure and development rules.

## Core Position

`vibe` is not the target.  
`spec` is the target.

A useful spec should include at least:
- Project architecture (how major modules connect)
- Test standards (what quality gates must pass)
- Roadmap and goals (what outcome is being optimized)
- Code quality standards (style, correctness, maintainability)
- Development workflow (how work is planned, executed, and reviewed)

This class teaches how to define and enforce these standards so AI can act like a reliable engineering teammate.

## Development Standard Taught in This Course

1. Update docs first (DDD)
2. Write/adjust tests second (TDD)
3. Implement code last (SDD-aligned execution)
4. Review with checkpoints before merge

## Session Index (Engineering-Focused)

These sessions are the primary references for the AI-assisted engineering workflow:

1. **Week 1, Session 1: Intro + Why "Not Vibe Coding"**
   - Focus: course framing, reliability mindset, limits of vibe coding
   - Slides (Marp): [`slides/w01d1/slides.md`](./slides/w01d1/slides.md)
   - PDF: [`slides/w01d1/slides.pdf`](./slides/w01d1/slides.pdf)

2. **Week 1, Session 2: Driving Effective Agentic Programming**
   - Focus: SDD / DDD / TDD, docs-first/tests-first flow, standards as the charter
   - Slides (Marp): [`slides/w01d2/slides.md`](./slides/w01d2/slides.md)
   - Syllabus notes: [`slides/w01d2/syllabus.md`](./slides/w01d2/syllabus.md)
   - PDF: [`slides/w01d2/slides.pdf`](./slides/w01d2/slides.pdf)

3. **Week 2, Session 1: Bootstrapping Agentic Automation**
   - Focus: project rules, skill/rule/command setup, planning habits
   - Slides (PPTX): [`slides/w02d1/slides.pptx`](./slides/w02d1/slides.pptx)
   - Course overview mention: [`syllabus.md`](./syllabus.md)

4. **Week 2, Session 2: Planning and Execution**
   - Focus: plan mode, architecture-aware planning, iterative execution
   - Slides (Marp): [`slides/w02d2/slides.md`](./slides/w02d2/slides.md)
   - Syllabus notes: [`slides/w02d2/syllabus.md`](./slides/w02d2/syllabus.md)
   - PDF: [`slides/w02d2/slides.pdf`](./slides/w02d2/slides.pdf)

5. **Week 5, Session 1: Workflow and Orchestration**
   - Focus: turning principles into repeatable workflows, multi-plan strategy, human/AI/code responsibility split
   - Slides (Marp): [`slides/w05d1/slides.md`](./slides/w05d1/slides.md)
   - Syllabus notes: [`slides/w05d1/syllabus.md`](./slides/w05d1/syllabus.md)
   - PDF: [`slides/w05d1/slides.pdf`](./slides/w05d1/slides.pdf)

6. **Week 5, Session 2: Ralph Loop for Reliable Long-Running Execution**
   - Focus: iterative continuation for large tasks, drift/hallucination mitigation, control loops
   - Slides (Marp): [`slides/w05d2/slides.md`](./slides/w05d2/slides.md)
   - Syllabus notes: [`slides/w05d2/syllabus.md`](./slides/w05d2/syllabus.md)
   - PDF: [`slides/w05d2/slides.pdf`](./slides/w05d2/slides.pdf)

## Repository Layout

- `slides/` - session materials
- `style/` - shared slide style/template
- `syllabus.md` - top-level course outline

