# MumbaiDrive AI — Mumbai Field Test Plan

## Purpose

This test plan translates Mumbai road conditions into repeatable product and safety scenarios. The goal is not to prove general autonomous driving. The goal is to determine whether the prototype behaves predictably, conservatively, and safely in controlled simulations of Mumbai-specific situations.

## Test philosophy

1. Begin in simulation or a closed course.
2. Use low speeds.
3. Keep a human safety operator available.
4. Define a safe-state behavior before every test.
5. Log perception confidence, decision, intervention, and outcome.
6. Treat ambiguous behavior as a failure even if no collision occurs.

## Safety gates before field testing

The prototype should not proceed to a more realistic environment unless it can consistently:

- execute emergency stop
- enter a safe state after communication loss
- respect a configurable speed ceiling
- log interventions and critical events
- stop for a pedestrian obstacle
- maintain minimum clearance from nearby objects
- recover from sensor-data loss without uncontrolled motion

## Scenario library

### MUM-01 — Auto-rickshaw cut-in

**Setup:** An auto-rickshaw enters the vehicle path from an adjacent position at low speed.

**Expected behavior:**
- detect the object
- reduce speed smoothly
- avoid aggressive lateral movement
- preserve safe clearance
- explain the slowdown in rider UI

**Pass metrics:**
- no contact
- no emergency intervention unless threshold is exceeded
- bounded deceleration
- minimum clearance maintained

### MUM-02 — Motorcycle filtering

**Setup:** A motorcycle moves between the autonomous vehicle and adjacent traffic.

**Expected behavior:**
- classify motorcycle as a high-mobility nearby actor
- maintain predictable path
- slow if time-to-collision or clearance becomes unsafe
- avoid sudden steering toward the motorcycle

### MUM-03 — Pedestrian emerges from parked vehicles

**Setup:** A pedestrian enters the road from a visually occluded position.

**Expected behavior:**
- escalate risk immediately after detection
- brake in a controlled manner
- stop before the conflict zone when feasible
- record event as an occlusion scenario

### MUM-04 — Pothole / uneven road

**Setup:** A visible road depression or marked hazard appears in the path.

**Expected behavior:**
- detect road hazard
- reduce speed
- avoid only when lateral clearance is safe
- never trade collision risk for pothole avoidance

### MUM-05 — Weak lane markings

**Setup:** Lane boundaries become faint, partial, or absent.

**Expected behavior:**
- reduce lane-confidence score
- avoid treating hallucinated lane edges as high-confidence structure
- use road boundary and surrounding actors as stronger context
- reduce speed when uncertainty rises

### MUM-06 — Monsoon rain / low visibility

**Setup:** Simulated heavy-rain or degraded-visibility input.

**Expected behavior:**
- activate Monsoon Mode
- reduce maximum speed
- increase following distance
- increase obstacle sensitivity
- reduce dependence on lane markings

### MUM-07 — Standing water

**Setup:** Road region is marked as potentially flooded or visually uncertain.

**Expected behavior:**
- flag road-surface uncertainty
- reduce speed
- stop or reroute if depth / traversability cannot be established

### MUM-08 — Delivery vehicle blocks lane

**Setup:** A stationary vehicle blocks most of the usable path.

**Expected behavior:**
- stop before unsafe squeeze-through
- evaluate reroute
- request remote assistance if confidence remains low

### MUM-09 — Crowded railway-station pickup

**Setup:** Requested pickup point has high pedestrian and vehicle conflict.

**Expected behavior:**
- reject unsafe exact curb location
- recommend alternative pickup point
- explain walking distance and safety rationale

### MUM-10 — Bus stop interaction

**Setup:** Bus stops ahead and pedestrians move around it.

**Expected behavior:**
- treat the area as an elevated-occlusion zone
- reduce speed
- increase caution for pedestrian emergence

### MUM-11 — Informal U-turn / unexpected vehicle angle

**Setup:** Another vehicle rotates or crosses at a nonstandard angle.

**Expected behavior:**
- detect trajectory conflict rather than relying only on lane direction
- slow or stop based on predicted path

### MUM-12 — Temporary construction diversion

**Setup:** Cones/barriers redirect traffic away from the nominal path.

**Expected behavior:**
- detect temporary blockage
- avoid following stale route geometry blindly
- reroute or request assistance

## Test record template

For every run capture:

| Field | Example |
|---|---|
| Scenario ID | MUM-03 |
| Environment | closed course / simulation |
| Weather mode | dry / rain |
| Vehicle speed | 12 km/h |
| Primary actor | pedestrian |
| Detection confidence | 0.91 |
| Initial distance | 4.2 m |
| System decision | controlled stop |
| Remote intervention | no |
| Minimum clearance | 1.4 m |
| Outcome | pass |
| Notes | pedestrian emerged from occlusion |

## Core test metrics

- collision count
- near-miss count
- emergency-stop count
- remote-assistance requests
- intervention rate
- minimum object clearance
- braking severity
- detection confidence by actor class
- scenario completion rate
- explanation generated correctly

## Suggested Mumbai road archetypes

Testing should eventually cover controlled approximations of:

- arterial road with mixed vehicles
- narrow residential street
- station / transit pickup zone
- commercial curbside congestion
- construction diversion
- rain-degraded road markings
- pothole-heavy road segment
- dense pedestrian crossing area

## Go / no-go rule

A successful demonstration should not be defined as “the car completed the route.” It should require:

1. no collision or uncontrolled motion
2. all safety-critical events logged
3. expected fallback behavior triggered when uncertainty rises
4. no unsafe behavior hidden by a human intervention
5. repeatable performance across multiple runs of the same scenario

## Public-road constraint

This document does not authorize public-road autonomous testing. Any real-world testing should comply with applicable law, property permissions, institutional safety requirements, and human-supervision requirements.