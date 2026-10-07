---
name: feedback-loops
description: Feedback loops — a defect crashes loudly at the point it is detected; only an expected failure gets a retry or fallback. Use when writing or reviewing error handling, catch blocks, default values, fallbacks, retries, or setup, CI and infrastructure scripts, and whenever deciding whether code should crash or recover.
---

# Feedback loops

An agent gets better only as fast as reality tells it that it is wrong. A crash at the point of the bug, with a message that names the cause, closes the loop: read it, fix it, rerun, all in minutes. That loop is what makes agentic work compound. Every bug that surfaces this way is cheap.

Fake resilience cuts the loop. A fallback, a default value, a caught-and-logged error or a silent retry turns a bug into wrong behaviour that surfaces later, somewhere else. By then the evidence is buried in plausible-looking output, and someone has to dig through it to find the cause.

## Classify every failure

Every failure path is one of two kinds. Decide which, explicitly, each time you write or review one:

1. **Defect.** The code, its configuration or its environment is not what we assumed. Examples: an impossible state, missing configuration, a pinned tool absent from the image, an unknown enum value, a schema mismatch, a broken invariant. **Crash**, at the point of detection. This is about 99% of failure paths.
2. **Expected failure.** Something the world may do to correct code: a network timeout, a third-party 5xx, invalid user input, a contended lock, a rate limit. **Recover**, with a bounded retry or a typed error the caller branches on.

To tell them apart, name the real event that triggers the path in a correctly working system. If you can name one, it is an expected failure. If you can't, it is a defect. "It might happen" is not an event.

## A good crash

- It fails where the wrong assumption is first visible, not three layers later.
- Its message states what was expected, what was found, and what to change.
- The state that explains the next drift is logged on every run, even successful ones. That way drift shows up before it breaks anything.

## A good recovery

- It covers only the expected failure you named; every other error still crashes.
- It is bounded: timeouts, capped attempts, exponential backoff with jitter.
- It retries at one layer only. Retries multiply across layers: five layers with three retries each send 243 times the load to the bottom.
- It retries only idempotent operations.
- It is visible: an exhausted retry crashes, and a fallback that fires is logged.

## Fake resilience to remove on sight

Each of these turns a defect into silence. Replace it with a crash, or narrow it to the one expected failure it was meant for:

- a catch that logs and continues;
- a default for a value that must exist;
- "if it's missing, install, download or create it";
- a retry around a deterministic error;
- an empty result returned on failure;
- a type widened to accept the impossible state.

## Sources

- [James Shore, "Fail Fast", IEEE Software (2004)](https://martinfowler.com/ieeeSoftware/failFast.pdf): failing immediately and visibly shortens debugging, while workarounds and defaults make software fail mysteriously later.
- [Joe Armstrong, "Making reliable distributed systems in the presence of software errors" (2003)](https://erlang.org/download/armstrong_thesis_2003.pdf): "let it crash"; a failing process stops and recovery happens elsewhere, instead of limping on.
- [George Candea and Armando Fox, "Crash-Only Software", HotOS IX (2003)](https://www.usenix.org/conference/hotos-ix/crash-only-software): making crash and recovery the only paths keeps them exercised and trustworthy.
- [Marc Brooker, "Timeouts, retries, and backoff with jitter", Amazon Builders' Library](https://builder.aws.com/content/3EumjoZascWd1oZiEgL8ORlv3qE/timeouts-retries-and-backoff-with-jitter): retry only transient failures; bound retries with backoff and jitter; require idempotency; retries compound across layers.
- [Anthropic, "Building effective agents"](https://www.anthropic.com/engineering/building-effective-agents): agents depend on "ground truth from the environment" at each step, such as tool results and tests.
