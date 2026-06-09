# Code Reviews That Don't Suck: Business Value Over Syntax

We've all been there. You open a pull request, and within an hour, you get three comments:

> "This should be camelCase."
>
> "Missing semicolon on line 42."
>
> "Can you reorder these imports?"

And the actual logic change? The thing that solves a real user problem? Nobody looked at it. The PR sits there for two more days while people argue about brackets.

That's not code review. That's being a slow, expensive linter.

Here's the thing — tech exists because business exists. If the code doesn't solve the actual problem, it doesn't matter how pretty it looks. Every minute a human spends catching something a machine can catch is a minute stolen from catching something a machine *can't*.

So let's talk about how to fix this.

---

## Draw the Line

First thing — let's be clear about what machines should handle and what they shouldn't.

If a problem can be caught by a tool, no human should ever flag it in a PR review. Period. We have the tools. We just don't use them properly.

| What | Who handles it |
|------|----------------|
| Code formatting | Prettier, ESLint, Checkstyle — not you |
| Naming conventions, code smells | SonarQube, linting rules |
| Build breaks, compile errors | CI pipeline |
| Type issues | TypeScript compiler with strict mode |
| Dependency vulnerabilities | Dependabot, Snyk |
| Running tests | CI pipeline |

If your PR review comments are mostly about formatting and naming, your automated pipeline isn't failing — *your process is*. Fix the pipeline. Don't patch it with human eyeballs.

Set up Prettier. Configure ESLint. Plug in SonarQube. Make the CI fail on violations. Then stop commenting about semicolons in code reviews forever.

## Where Humans Still Win

Now here's the real question — once you stop wasting time on syntax, what should you actually review?

This is the part that was always supposed to matter. And now you finally have time for it.

### 1. Does this code solve the actual problem?

Not "does it compile." Not "is it clean." Does it do what the requirement says?

I know this sounds obvious. But you'd be surprised how many PRs implement something technically correct that completely misses the point of the ticket. Maybe the requirement said "admin users only" and the code checks for "logged in users." Both work. Only one is right.

Tech without business value is just expensive typing.

### 2. What happens when things go wrong?

Happy path works? Cool. But bugs that take down production are never in the happy path.

Think about — null inputs, concurrent requests, network timeouts, partial failures, unexpected user behavior. What happens when the third-party API returns a 500? What happens when the database connection drops mid-transaction?

These are the things worth spending your brain cells on. Not whether someone used tabs or spaces.

### 3. Will it fail gracefully?

Something *will* break eventually. That's just how software works. The question is — when it breaks, does the system degrade safely? Or does one unhandled exception cascade into a full service outage?

Are errors logged properly? Can someone debug this at 2 AM without guessing? These are review-worthy questions.

### 4. Are we building debt or building value?

Is the same logic copy-pasted across three files? Is this one function doing five different things? Does the new code tightly couple to existing systems so that changing anything later becomes a nightmare?

DRY, SOLID, separation of concerns — yeah, these are buzzwords, but they exist for a reason. Every shortcut you approve today is a headache someone (probably you) will deal with six months from now.

### 5. Are the tests actually testing the right thing?

CI can tell you if a test passed. It can't tell you if the test *matters*.

Are the assertions checking real business behavior, or are they just there to push the coverage percentage up? A test that asserts `expect(result).toBe(result)` will pass every time and prove absolutely nothing.

This one becomes even more important now — and I'll explain why next.

---

## The Agentic Era: Tests, AI, and the Trap

Let's address the elephant in the room. AI coding agents are everywhere now. Claude Code, Cursor, Copilot — pick your favorite.

And this changes something important about how we think about tests.

### The old excuse is dead

Remember when people said "I didn't have time to write tests, sprint deadline is tomorrow"? Yeah, that excuse doesn't work anymore.

An AI agent can generate a full test suite in seconds — mocking, edge cases, fixtures, the whole thing. The developer's role has shifted. You're not a test writer anymore. You're a test editor.

Spend 2 minutes telling the agent what to test. Spend 5 minutes reviewing what it wrote. Done. That used to take an hour.

So in 2025, if someone submits a PR without tests, it's not a time problem. It's a discipline problem. When an agent can stub out 15 edge cases in three seconds, there's just no excuse.

### But here's the trap

This is the important part. Pay attention.

Say you wrote some code. It's got a bug — maybe a discount function applies 20% off when the requirement says 10% off. You didn't notice. Then you ask an AI agent: "Write tests for this code."

The agent will write thorough, well-structured tests that prove your code does exactly what it does — gives 20% off. Every test passes. CI goes green. Everything looks perfect.

Except the business logic is wrong. The agent tested your implementation. Not the requirement.

This is what I call **agentic confirmation bias**. The agent doesn't know what the code *should* do. It only knows what the code *does* do. And it writes tests to confirm what's already there.

This is exactly why point #5 above — "are the tests testing the right thing?" — matters more now than ever before. When you're reviewing a PR, you have to read the tests against the actual requirement, not against the code. If you don't, the whole testing layer becomes theater.

---

## The New Contract

Here's the deal. This is the takeaway.

| Who | Owns What |
|-----|-----------|
| **Pipelines** | Syntax, style, builds, running tests |
| **AI Agents** | Generating tests, boilerplate, the heavy lifting |
| **You** | Business logic, edge cases, architecture, test intent |

Set up your pipeline to catch everything it *can* catch. Use agents to handle what's tedious. Then spend your human review time on the only thing that actually moves the needle — making sure the code delivers real business value.

Let's stop reviewing like compilers and start reviewing like engineers.
