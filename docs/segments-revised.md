# Code Reviews That Don't Suck: Business Value Over Syntax

## Blog Post & KSS Segments

---

### Segment 1: You're Not a Compiler (The Hook)

**What it does:** Grabs the room. Names the problem everyone feels but nobody says out loud.

**The pitch:** Every dev has been on both ends of a PR that sat for two days because someone flagged a missing semicolon, a camelCase violation, or an import ordering issue — while the actual business feature sat untouched. That's not code review. That's acting as a slow, error-prone linter.

**The hard truth:** Tech exists because business exists. Every minute a human spends catching what a machine can catch is a minute stolen from catching what a machine *can't*.

**Thesis for the rest of the talk:** We need to stop reviewing like compilers and start reviewing like engineers.

---

### Segment 2: Draw the Line — What Machines Handle, What You Don't

**What it does:** Concrete and actionable. Not philosophy — actual tools and actual rules.

**The principle:** If a problem can be caught deterministically, no human eye should ever see it in a PR review. Full stop.

**What to offload (with specific tools):**

| Concern | Tool | Human ever needs to see this in review? |
|---------|------|----------------------------------------|
| Code style, formatting | Prettier, ESLint, Checkstyle | No |
| Naming conventions, code smells | SonarQube, ESLint rules | No |
| Build breaks, compile errors | CI pipeline | No |
| Type safety | TypeScript compiler, strict mode | No |
| Dependency vulnerabilities | Dependabot, Snyk | No |
| Basic test execution | CI pipeline | No |

**The uncomfortable question:** If your PR review comments are 80% about formatting and naming, your automated pipeline isn't failing — *your process is*. Fix the pipeline, don't patch it with human eyeballs.

**What this frees up:** Every minute you spent flagging a misplaced bracket is a minute you can now spend on the stuff that actually matters. Which brings us to—

---

### Segment 3: Where Humans Still Win (The Real Review)

**What it does:** The core value. This is what code review was always supposed to be. These are the things no machine can catch — and now you finally have time for them.

**The checklist that matters:**

1. **Business Logic — Does this code solve the actual problem?**
   Not "does it compile." Not "is it pretty." Does it do what the requirement says? Read the ticket, read the code, and verify they match. Tech without business value is just expensive typing.

2. **Edge Cases — What happens when things go wrong?**
   Happy path works? Great. What about null inputs? Concurrent requests? Network timeouts? Partial failures? The bugs that take down production are never in the happy path.

3. **Error Handling — Will it fail gracefully?**
   When (not if) something breaks, does the system degrade safely? Or does one unhandled exception cascade into a service outage? Are errors logged with enough context to debug at 2 AM?

4. **Architectural Health — Are we building debt or building value?**
   Is it DRY, or is the same logic copy-pasted across three files? Does it respect SOLID, or is this one function doing five things? Does the new code couple tightly to existing systems, making future changes painful?

5. **Test Quality — Are the tests testing the right thing?**
   CI can tell you if a test passes. It cannot tell you if the test *matters*. Are the assertions validating business acceptance criteria, or just inflating the coverage number? (This becomes even more critical in the agentic era — see next segment.)

**The shift in mindset:** You're no longer a syntax checker. You're an architect. Your job in a review is to protect the business value, the system integrity, and the developer experience of whoever maintains this code six months from now.

---

### Segment 4: The Agentic Era — Tests, AI, and the Trap

**What it does:** The modern twist. Acknowledges reality — devs are already using AI agents — and provides the critical warning that makes this session worth attending.

**Part A: The Death of the "No Time for Tests" Excuse**

The old excuse: "Sprint deadline is tomorrow, I didn't have time to write tests."

That excuse is dead. An AI coding agent can generate a comprehensive test suite — mocking dependencies, covering edge cases, setting up fixtures — in seconds. The friction is near zero.

**The new role:** Developers aren't "test writers" anymore. They're "test editors." You spend 2 minutes directing an agent to write tests, and 5 minutes reviewing them — instead of an hour writing from scratch.

**The new standard:** Submitting a PR without tests in the agentic era isn't a time constraint. It's laziness. If an agent can stub out 15 edge cases in three seconds, there is zero excuse for a naked PR.

**Part B: Agentic Confirmation Bias — The Critical Warning**

Here's the trap. If you ask an agent, "Write tests for this code I just wrote," it will write brilliant, thorough tests that prove your code works *exactly the way you wrote it* — even if your code has a fundamental logic bug.

The agent tests the implementation. Not the requirement.

**Example:** You write a discount function that applies 20% off when it should apply 10% off. You ask the agent to test it. The agent writes a test asserting the output is 20% off. Test passes. Everything green. Business logic is wrong.

**This is why Segment 3's "Test Quality" check matters more than ever.** The human reviewer must verify: Do these tests validate the *business acceptance criteria*, or do they just echo the author's (potentially flawed) implementation?

**The rule:** Never trust an agent-generated test at face value. Read it against the requirement, not against the code.

---

### Closing: The New Contract

**One slide. Three lines. This is the takeaway.**

| Who | Owns What |
|-----|-----------|
| **Pipelines** | Syntax, style, builds, basic test execution |
| **AI Agents** | Test generation, boilerplate, heavy lifting |
| **Humans** | Business logic, edge cases, architecture, test intent |

**The call to action:** Set up your pipeline to catch everything it *can* catch. Use agents to generate what's tedious. Then spend your human review time on the only thing that actually matters — making sure the code delivers business value.

**Closing line:** Let's stop reviewing like compilers and start reviewing like engineers.
