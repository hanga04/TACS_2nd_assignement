# Agent log

This is a copied skeleton of the already implemented agent log for "airlock" repository.

| # | Tool and model | Goal | Context given | Prompt summary | Accepted | What was wrong, and how it was detected |
|---|---|---|---|---|---|---|
| 1 | Claude Code, Opus 5.5 | Port the airlock to Java 21, with the same design, the same scenario files and the same tests, so that one specification serves both languages | the Rust version: its code, `SPEC.md`, `TEST.md` and `scenarios/` | "Ok, can you create a Java implementation of the airlock? Can the scenarios be reused between implementations (or copied)? And should SPEC.md and TEST.md be generic enough to apply to both implementations, or need to be tailored?" | all of it: one class per Rust module; messages as sealed interfaces and records, printed in the same format as the Rust log, so the scenario files and expected logs are shared unchanged; `SPEC.md` and sections 1 to 3 of `TEST.md` made language-neutral, and identical in both versions; section 4 of `TEST.md` written for Java | The first draft of the system test ended with a meaningless static block, left by the agent while editing; it was found on rereading the file and removed. Then the log of S1 was compared, message by message, with the Rust version's: the same 36 messages in the same order. All 10 tests pass; the random scenarios need 14 runs to take every transition, as in Rust, since the generator and its seed are the same. |
| 2 | Claude Code, Opus 5.5 | Move the shared parts up (see entry 26 of the Rust log); this version moves to `java/` | the Rust and Java versions | "Should both .md be moved up, and TEST.md include info for both languages?", then "Yes" | all of it; the tests read `../scenarios/` | All 10 tests pass from the new place. |

## Code review checklist

- [ ] Each decision function matches its state machine in SPEC.md 3.3, transition by transition, and ignores every other event
- [ ] Timing edge cases: `>` or `>=`, and two events less than half a second apart
- [ ] A late or unexpected message is ignored in every state
- [ ] The token is released only after the sensor confirms "closed"
- [ ] Every timer is stopped or replaced when its state is left
- [ ] No exception swallowed; `InterruptedException` restores the interrupt flag
- [ ] No unbounded queues, and no shared state between components
- [ ] Each thread stops when `stopped()` is true, and `send` cannot block for ever
- [ ] Every `switch` on a message is exhaustive, with no `default`
- [ ] The change cites the property it serves, and the tag is on the right code


