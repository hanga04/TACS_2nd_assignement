# Test plan
## write about Criteria, Scenarios, How the tests are built
How the specification in [SPEC.md](SPEC.md) is verified: what is tested, and
when it is enough (sections 1 and 2, independent of the language), and how
the tests are built, in Java

## 1. Criteria

The tests are adequate when:

1. **Coverage.**
2. **Requirements verification.** 
3. **Design conformance.** 
4. **Concurrency.** 

**Limitations.** 

## 2. Test scenarios


| # | Scenario | What happens | Inputs | Expected log | New transitions |
|---|---|---|---|---|---|
| S1 | Main scenario (J1; P1, P2, P4) | A user goes in, through A and then B; once inside, the user presses B while A is still closing, so B waits for the token, which A hands over once closed. | user 1 at 0 through A, B; walk 1 s, cross 0.5 s | all of it, as in the sequence diagram of SPEC.md, section 3.2 | T1 to T4, T6, T9 to T12 |


## 3. Test implementation

### 3.1 Test architecture

The tests drive the system from outside, as its users would, and judge it
only by its log. One test runs every scenario file through the test driver,
checks P1 over the log, and has the provided tool,
[`tracecheck`](tracecheck/README.md), compare it with the expected log:


| Part | Where | Responsibility |
|---|---|---|
| The scenarios | `scenarios/`, shared by both versions | a scenario file and an expected log for each scenario (section 3.3) |


### 3.2 The fake environment


**Limitation: y**

### 3.3 Scenario files and expected logs


Its expected log (`scenarios/<name>.expected.txt`) has one line per arrow to
or from a controller, in order, as the log prints it (the arrows between the
users and the hardware are not logged).The full notation is in
[tracecheck/README.md](tracecheck/README.md).



### 3.4 Testing on real threads

The operating system decides the schedule, and threads are late by up to a
few tens of milliseconds, sometimes more. So:

- Scenarios keep independent events apart, by half a second or more, which
  makes a change of order unlikely, but not impossible. Where several orders
  remain possible, the expected log allows each of them, with `or` or with a
  gap: in S5, a user's press may reach a door just before the other door
  releases the token, or just after.


| Name | Value | Meaning |
|---|---|---|
| `TRAVEL` | 1 000 ms | fake hardware: time a door takes to move, unless the scenario file says otherwise |

