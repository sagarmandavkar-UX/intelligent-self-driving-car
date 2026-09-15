# MumbaiDrive AI — Autonomous Mobility for Dense Urban Roads

MumbaiDrive AI is a product and engineering concept exploring how autonomous mobility could work in a city like **Mumbai, India**, where traffic is dense, road behavior is highly mixed, curb space is constrained, lane markings are inconsistent, and monsoon conditions can materially change the driving environment.

The project builds on the original self-driving-car architecture—GPS, IMU, ultrasonic sensing, computer vision, reinforcement learning concepts, Arduino control, and an Android interface—but reframes the work around a stronger product question:

> **How might we design an autonomous urban mobility system that can navigate Mumbai's complex road environment safely while giving riders the visibility, trust, and control they expect from modern ride-hailing products?**

> **Project status:** This repository currently contains the system architecture, Android/CV/RL design documentation, dependencies, and product strategy. Some implementation modules described in the original project plan are not yet committed here. Planned capabilities are labeled separately below so the repository does not present roadmap items as already shipped.

---

## Why Mumbai?

Mumbai is not a simple lane-following problem. A useful autonomous-mobility system must reason about:

- cars, buses, taxis, auto-rickshaws, motorcycles, scooters, bicycles, and pedestrians sharing constrained space
- frequent cut-ins and vehicles approaching from unconventional angles
- weak or missing lane markings
- narrow roads and informal stopping behavior
- crowded railway-station, office, hospital, mall, and airport pickup zones
- potholes, speed breakers, construction, temporary diversions, and parked vehicles
- heavy rain, standing water, reduced visibility, and changing road conditions during monsoon season

That makes Mumbai the product constraint—not merely the test location.

---

## Product vision

**MumbaiDrive AI combines autonomous navigation, rider trust, and fleet operations into one Mumbai-first mobility experience.**

### Core product loop

**Request → Safe pickup → Autonomous ride → Explain decisions → Remote assist when needed → Learn from the route**

### Primary users

1. **Rider** — wants a predictable, safe, understandable ride.
2. **Remote operator** — assists when the autonomous system encounters ambiguity.
3. **Fleet operator** — monitors vehicle health, interventions, route risk, and service quality.
4. **Product / safety team** — studies failure modes and improves the Mumbai driving model.

---

## Product pillars

### 1. Mumbai Driving Intelligence

Instead of treating lane detection as the core problem, the system is designed around **unstructured-road understanding**.

Planned perception classes include:

- auto-rickshaws
- motorcycles and scooters
- pedestrians
- buses and cars
- road barriers
- parked vehicles
- potholes and uneven road surface
- standing water
- construction zones

The intended decision stack is:

**Perception → Mumbai Context Engine → Risk Assessment → Motion Decision → Vehicle Control**

### 2. Rider trust and transparency

Inspired by the product patterns of ride-hailing platforms, the rider experience is designed around confidence before and during the trip.

Planned rider features:

- trip PIN verification
- live vehicle location and trip progress
- safer pickup-zone recommendation
- emergency stop / assistance controls
- trusted-contact trip sharing
- remote-operator assistance
- contextual explanations such as:
  - “Slowing down: motorcycle entering path”
  - “Waiting: pedestrian crossing ahead”
  - “Re-routing: road obstruction detected”
  - “Reducing speed: uneven road detected”

A major product principle is that the rider should understand **why the autonomous vehicle behaved unexpectedly**.

### 3. Smart pickup and drop-off

Exact GPS coordinates are not always the safest or most practical stopping locations in Mumbai.

The product concept therefore includes **Smart Pickup Zones** that prioritize:

- safe stopping space
- lower pedestrian conflict
- lower traffic obstruction
- access to known station/airport/office pickup points
- short walking distance from the user's requested location

Example:

> **Requested:** Andheri Station East  
> **Recommended autonomous pickup:** 110 m away at a lower-conflict curb zone

### 4. Risk-aware routing

The fastest route is not always the best autonomous route.

A future route engine would score roads using factors such as:

- traffic density
- pedestrian density
- road width
- lane-marking confidence
- construction activity
- pothole / road-quality risk
- intersection complexity
- weather conditions
- historical intervention frequency

The system can then compare:

| Route | ETA | Complexity |
|---|---:|---|
| Fastest | 24 min | High |
| Recommended AV route | 29 min | Moderate |

### 5. Monsoon Mode

Heavy rain changes both perception quality and safe-driving behavior.

Planned Monsoon Mode behavior:

- lower maximum speed
- increase following distance
- reduce steering aggressiveness
- lower trust in lane markings
- increase obstacle sensitivity
- detect standing water / pothole risk
- prefer object-boundary reasoning when lane confidence is low

Example rider message:

> **Monsoon Safety Mode Active**  
> Speed reduced • Following distance increased • Lane confidence low

### 6. Remote assistance

The product does not assume perfect full autonomy.

When system confidence falls below a safe threshold, the vehicle should enter a controlled state and request assistance.

Possible operator actions:

- continue
- reroute
- reverse
- remain stopped
- end autonomous mode

This makes the system more credible than claiming it can autonomously resolve every road situation.

### 7. Fleet operations

The product vision includes an operations layer for monitoring a small autonomous fleet.

Example fleet metrics:

- vehicle status
- battery level
- autonomous / assisted state
- rides completed
- remote interventions
- emergency stops
- difficult intersections
- potholes / road hazards detected
- average autonomous completion rate

---

## Mumbai field-test scenarios

The testing plan focuses on product-relevant edge cases rather than generic obstacle avoidance.

| Scenario | Expected behavior |
|---|---|
| Auto-rickshaw cuts into path | Reduce speed, preserve clearance, avoid aggressive steering |
| Motorcycle filters between vehicles | Maintain path, reduce speed if risk rises |
| Pedestrian crosses between parked cars | Immediate risk escalation and controlled stop |
| Large pothole ahead | Detect, slow, and safely adjust path if clearance allows |
| Heavy monsoon rain | Enter conservative Monsoon Mode |
| Weak / missing lane markings | Shift from lane-centric to object-boundary reasoning |
| Road blocked by delivery vehicle | Stop safely and request reroute / remote assistance |
| Crowded station pickup | Recommend lower-conflict pickup zone |

Detailed scenarios are documented in [`product/MUMBAI_TEST_PLAN.md`](product/MUMBAI_TEST_PLAN.md).

---

## Product metrics

### North Star Metric

**Safe Autonomous Kilometers Completed**

The project should not optimize only for technical accuracy. Product success requires safety, autonomy, rider confidence, and operational reliability.

Supporting metrics include:

| Category | Metric |
|---|---|
| Safety | interventions per 100 km |
| Safety | near-miss / critical-event rate |
| Autonomy | autonomous trip completion rate |
| Reliability | disengagements per km |
| Rider | completed rides |
| Rider | safety-confidence score |
| Rider | explanation usefulness score |
| Pickup | successful first-attempt pickup rate |
| Operations | remote assists per ride |
| Mumbai | motorcycle interaction success rate |
| Mumbai | pedestrian response success rate |
| Mumbai | pothole / road-hazard avoidance rate |
| Mumbai | Monsoon Mode completion rate |

See [`product/METRICS.md`](product/METRICS.md) for definitions.

---

## Existing technical foundation

The original project architecture includes:

- Arduino-based hardware I/O
- Bluetooth communication
- ultrasonic, GPS, and IMU sensing
- camera-based lane / object perception concepts
- reinforcement-learning concepts for adaptive driving
- Android-based telemetry and control concepts
- manual override / emergency-stop design

The architecture documentation currently specifies a modular stack covering perception, decision, control, communication, and UI layers. See [`docs/ARCHITECTURE.md`](docs/ARCHITECTURE.md).

---

## Current repository contents

```text
intelligent-self-driving-car/
├── android/
│   └── README.md                # Android control-interface design
├── arduino/
│   └── README.md                # embedded-system design
├── cv/
│   └── README.md                # computer-vision design
├── rl/
│   └── README.md                # reinforcement-learning design
├── docs/
│   └── ARCHITECTURE.md          # original system architecture
├── product/
│   ├── PRODUCT_STRATEGY.md      # user problem, positioning, roadmap
│   ├── MUMBAI_TEST_PLAN.md      # field-test scenarios and safety gates
│   ├── USER_JOURNEY.md          # rider + remote-assistance journey
│   └── METRICS.md               # product and safety metrics
├── requirements.txt
└── README.md
```

---

## Product roadmap

### P0 — Build the trustworthy Mumbai prototype

- make the documented Arduino / CV / control modules executable in-repo
- instrument emergency-stop and intervention logging
- detect cars, pedestrians, motorcycles, and auto-rickshaws
- add pothole / road-hazard detection prototype
- add Mumbai scenario simulation harness
- build rider trip-state UI
- build “Why did the car do that?” explanations

### P1 — Complete the mobility experience

- Smart Pickup Zones
- route-risk scoring
- Monsoon Mode
- remote-assistance workflow
- operations dashboard
- trip safety / intervention timeline

### P2 — City-learning layer

- crowdsourced road hazards
- known difficult-intersection database
- fleet-shared pothole / standing-water observations
- route recommendations informed by historical interventions
- scenario regression testing by Mumbai road archetype

### Later

- SLAM
- LiDAR
- V2V communication
- richer edge-compute architecture
- multi-vehicle fleet optimization

---

## Product documentation

- [`PRODUCT_STRATEGY.md`](product/PRODUCT_STRATEGY.md) — customer problem, positioning, prioritization, roadmap
- [`MUMBAI_TEST_PLAN.md`](product/MUMBAI_TEST_PLAN.md) — scenario-based field testing and safety gates
- [`USER_JOURNEY.md`](product/USER_JOURNEY.md) — rider and remote-operator experience
- [`METRICS.md`](product/METRICS.md) — safety, autonomy, rider, and Mumbai-specific metrics

---

## Safety scope

This repository is an educational prototype and product exploration. It is **not a road-certified autonomous-driving system** and should not be used for unsupervised public-road operation. Real-world testing should begin in controlled environments, use a human safety operator, enforce low-speed limits, and define clear emergency-stop procedures.

---

## Product story for PM / APM interviews

> Designed a Mumbai-first autonomous mobility concept for dense, unstructured traffic. Reframed a self-driving-car prototype around rider trust, mixed-traffic perception, smart pickup zones, risk-aware routing, monsoon behavior, remote assistance, and fleet operations. Defined a scenario-based field-test plan and product metrics spanning safety, autonomy, rider confidence, and Mumbai-specific road interactions.

---

## License

MIT License. See [`LICENSE`](LICENSE).

## Author

**Sagar Mandavkar**  
GitHub: [@sagarmandavkar-UX](https://github.com/sagarmandavkar-UX)
