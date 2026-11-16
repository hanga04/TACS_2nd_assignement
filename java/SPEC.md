# Specificationc
## write about Context, Functional requirements, Non-functional requirements, Constraints, Design

This document specifies the context, requirements and design of 


## 1. Context
This section

![](airlock.svg)

**Figure 1.** **



## 2. Requirements

### 2.1 Functional requirements
This section describes


#### Table 1. Users' journeys

| # | Journey |
|---|---|
| J1 | A user goes in: presses door A's button, goes through when its light is green, then presses door B's button and goes through B when its light is green (Figure 2, section 2.3). |



#### Table 2. Alternative and exception flows

| # | Flow | What the airlock does |
|---|---|---|
| A1 | A user is too slow to go through an open door. | The door closes on its own after a while; the user must press again. |



### 2.2 Non-functional requirements: temporal properties

This section describes

#### Table 3. Temporal properties
| # | Kind | Property | Hazard if violated |
|---|---|---|---|
| P1 | safety | The two doors cannot be simultaneously open, partially or fully. | the room is open to the outside (severe) |



All are *temporal properties*: 


### 2.3 Main journey, as the user sees it


**Figure 2.** 

### 2.4 Constraints, assumptions and exclusions

Constraints, on the solution:

- Each component runs on its own thread, and they share nothing but bounded
  message queues and the run context: the event log, the clock and the stop
  flag (section 3.4).


Assumptions:
-
-

Exclusions:
-
-

## 3. Design

### 3.1 Structure

The system structure at runtime is described by a communication diagram (Figure 3): 


**Figure 3.** 


#### Table 4. Messages
| Message | From → to | Meaning | In the log |
|---|---|---|---|
| `Press` | user → door hardware | the user presses the door's button | not logged |



### 3.2 Main scenario with internal interactions (S1)


**Figure 4.** 


### 3.3 State machines


**M1: Door controller**


**Figure 5.**


**M2: Interlocker**

**Figure 6.** 



### 3.4 Implementation modules and dependencies



**Figure 7.** 


#### Table 5. Implementation modules
| Module | Responsibility | Exposes |
|---|---|---|


### 3.5 Summary of design decisions

#### Table 6. Summary of design decisions and their rationale
| # | Decision | Rationale | Serves |
|---|---|---|---|
| D1 | The airlock controller is implemented by active objects (two door controllers and an interlocker), each with its own thread and inbox; they share no memory and only exchange messages. | no data races; each component can be understood and reviewed alone | P1 to P3 |
| D2 | The right to open is a token held by the interlocker, with a FIFO queue of waiting doors; a door releases it only once closed.  | P1, as long as every door follows the protocol (section 3.1); first come, first served bounds the wait (P2); no data races | P1, P2 |

