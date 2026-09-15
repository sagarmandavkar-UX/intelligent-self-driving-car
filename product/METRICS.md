# MumbaiDrive AI — Metrics Framework

## North Star Metric

**Safe Autonomous Kilometers Completed**

A kilometer counts toward the North Star only when the vehicle remains within the defined operating domain, completes the segment without a collision or uncontrolled safety event, and logs any intervention or fallback correctly.

This avoids optimizing for distance alone.

## Metric hierarchy

### 1. Safety

| Metric | Definition | Why it matters |
|---|---|---|
| Critical events / 100 km | collisions, near misses, uncontrolled motion | primary safety guardrail |
| Emergency stops / 100 km | safety-stop activations | highlights dangerous situations or overly conservative behavior |
| Minimum clearance violations | object clearance below threshold | measures spatial safety |
| Hard-braking events / 100 km | deceleration above comfort/safety threshold | detects reactive rather than anticipatory driving |
| Intervention rate | human safety interventions / 100 km | core autonomy-quality signal |

### 2. Autonomy

| Metric | Definition |
|---|---|
| Autonomous trip completion rate | trips completed without human intervention |
| Disengagements / km | transitions out of autonomous mode per distance |
| Remote assists / ride | high-level operator assistance requests |
| Safe fallback success rate | uncertain situations that correctly enter safe state |
| Scenario pass rate | successful repeated runs by test scenario |

### 3. Rider experience

| Metric | Definition |
|---|---|
| Ride completion rate | requested rides completed successfully |
| Safety-confidence score | post-ride 1–5 confidence rating |
| Explanation usefulness | rider rating of “why did the car do that?” messages |
| Pickup success | rider boards at first recommended pickup location |
| Pickup walking distance | displacement between request point and recommended curb |
| Unexpected-stop confusion rate | riders reporting that a stop felt unexplained |

### 4. Operations

| Metric | Definition |
|---|---|
| Assistance response time | time from AV request to operator action |
| Vehicle availability | % fleet time available for rides |
| Safety alerts / vehicle-day | operational exception volume |
| Repeat-problem road segments | locations causing recurring interventions |
| Mean time to recovery | time from degraded state to normal operation |

### 5. Mumbai-specific interaction metrics

| Metric | Definition |
|---|---|
| Motorcycle interaction success rate | safe encounters with nearby motorcycles / scooters |
| Auto-rickshaw interaction success rate | safe encounters with auto-rickshaws |
| Pedestrian response success rate | pedestrian scenarios handled without critical event |
| Pothole / hazard avoidance rate | detected hazards safely handled |
| Weak-lane scenario pass rate | safe navigation under degraded lane markings |
| Monsoon Mode completion rate | rainy / low-visibility scenarios completed safely |
| Crowded pickup success rate | safe pickup completion in high-conflict curb scenarios |

## Funnel metrics

### Request → Pickup

- ride requests
- recommended pickup accepted
- rider reaches pickup
- vehicle reaches pickup
- first-attempt boarding success

### Pickup → Ride

- PIN verification success
- autonomous mode engaged
- trip starts without manual intervention

### Ride → Completion

- autonomous completion
- remote assistance required
- emergency stop required
- rider reaches intended / approved drop-off

### Completion → Learning

- event log completeness
- scenario labels assigned
- difficult road segment recorded
- rider feedback submitted

## Product-quality guardrails

A feature should not be considered successful if it improves one metric by damaging a more important safety metric.

Examples:

- reducing ETA is not a win if interventions increase
- avoiding potholes is not a win if lateral collision risk increases
- fewer remote assists is not a win if the system proceeds while uncertain
- higher autonomous completion is not a win if rider confidence or critical-event rate worsens

## Initial prototype targets

These are **product targets**, not claims of current performance.

- 100% emergency-stop command execution in controlled tests
- 100% critical-event logging
- >95% scenario-log completeness
- zero collisions in low-speed closed-course demonstrations
- >90% safe fallback success in intentionally ambiguous scenarios
- rider explanation shown for 100% of remote-assistance requests

## Dashboard recommendation

A portfolio-quality operations dashboard should lead with:

1. Safe autonomous km
2. Autonomous completion rate
3. Interventions / 100 km
4. Critical events
5. Remote assists
6. Mumbai scenario pass rate
7. Rider safety-confidence score

Then allow drill-down by:

- road archetype
- weather mode
- actor type
- route segment
- vehicle
- scenario ID

## PM interpretation

The purpose of this framework is to show that autonomous mobility is not one accuracy number. Product quality emerges from the interaction of **safety, autonomy, rider trust, local road performance, and operations**.
