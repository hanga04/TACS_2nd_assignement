# worked example

Prepared for the master's course Advanced Software Construction Techniques
(TACS), Faculty of Engineering of the University of Porto (FEUP), 2026/27, to
illustrate Agentic MDE for Embedded Controllers, the topic of the course's
second assignment: behavioural models (state machines, sequence diagrams and
temporal properties) turned into concurrent code by AI agents, and checked by
timed scenario tests.


| | |
|---|---|
| Specification: context, requirements (journeys, properties with their hazards, times), design (UML views, design decisions; modules, their dependencies and shared data, section 3.4) | [SPEC.md](SPEC.md) |
| Test plan: criteria, test scenarios; how the tests are built, in both languages | [TEST.md](TEST.md) |
| Scenario files and expected logs (S1 to S6), read by both versions | [scenarios/](scenarios/) |
| tracecheck: compares actual logs with expected logs, for any language (Python, standard library only) | [tracecheck/](tracecheck/README.md) |
| Agent context: rules for any language | [AGENTS.md](AGENTS.md) |
| Agent skills: add a scenario, derive an expected log, review a state machine | [.github/skills/](.github/skills/) |
| Rust version: code, tests, agent context, agent log | [rust/](rust/README.md) |
| Java version: code, tests, agent context, agent log | [java/](java/README.md) |

## Run it

The tests compare each run's log with its expected log using the provided
checker, [tracecheck/](tracecheck/README.md), so they need Python 3.8 or later.


## How it works

- **Each component is an active object.** 
- **Every message is logged** 
- **The tests run the real threads,**

## Using this example

You may copy and adapt any of this with your agent, into any of the languages
of the assignment. Record that in your agent log, and treat what you copy like
any other agent output: check it, and be able to explain it. Watch for airlock
assumptions that survive the adaptation, such as two doors, or a timing limit
that does not apply to your controller.

Prove early that your adapted setup really tests something: break the code on
purpose, for example release the token too early, and check that a scenario
fails.

## Agent skills

A skill is a folder with a `SKILL.md` file: instructions that an agent loads
only when a task needs them. GitHub Copilot (agent mode in VS Code, the
Copilot CLI and the cloud agent) finds them in `.github/skills/`; other agents
read the same format. These three are examples for the steps of the
assignment that are easy to get wrong:

| Skill | When | What it does |
|---|---|---|
| [add-scenario](.github/skills/add-scenario/SKILL.md) | test first, step 1 | adds the next scenario of TEST.md and its expected log, runs the tests, checks that the new one fails; never touches the code |
| [derive-expected-log](.github/skills/derive-expected-log/SKILL.md) | in a fresh session | writes an expected log from SPEC.md and TEST.md alone, citing the transitions, without reading the code or the actual logs |
| [review-state-machine](.github/skills/review-state-machine/SKILL.md) | after a change to a decision function | compares it with its state machine, transition by transition, ignored messages included, and reports |

They were written after the example was built, so the agent logs do not use
them. Adapt them to your project, or write your own, and record in your agent
log which skill each task used.

## What is required, and what goes beyond

This example does more than the assignment asks. Your project needs:

- **Modelling:** a structural diagram of any kind (here, a communication
  diagram), a state machine for each concurrent component, and a sequence
  diagram of the main scenario.
- **Testing:** scenarios with expected logs, described in your TEST.md,
  that together take every state and transition of every state machine and
  stress every property, all run by one command; at least one scenario with a
  race, run several times, whose expected log allows every valid outcome
  (here S5, run ten times); and a review of the code of each state machine
  against its diagram.

These parts are optional: copy them if they help, and leave them out if not.

- Alternatives (`or`) and gaps (`...`) beyond what a race scenario needs.
- Users with routes: simpler fake users that just press buttons at given
  times are enough.
- `REPEAT`, to run *every* scenario many times.

## What this example leaves out

- It covers one kind of fault, a stuck door.
- It ignores a press while a door is closing, and has no obstruction sensor,
  to keep the code short. `SPEC.md` describes both.
- Times are checked only where an expected log gives a bound.

## License

MIT (see `LICENSE`): copy and adapt any of it, code and documents, in your project; say so in your agent log.
