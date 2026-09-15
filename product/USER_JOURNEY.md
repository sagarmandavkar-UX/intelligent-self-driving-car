# MumbaiDrive AI — User Journey

## Rider journey

### 1. Request ride

The rider enters pickup and destination.

The system evaluates not only ETA, but also whether the requested pickup point is appropriate for an autonomous vehicle.

**Product decision:** if the exact pickup has high curb conflict, pedestrian density, or poor stopping space, recommend a nearby safer pickup zone.

Example:

> Requested: Andheri Station East  
> Recommended pickup: 110 m away  
> Reason: lower pedestrian conflict and safer stopping space

### 2. Confirm vehicle

Before boarding, the rider sees:

- vehicle ID
- vehicle location
- ETA
- trip PIN
- current autonomy status
- expected route
- whether Monsoon Mode is active

### 3. Boarding and verification

The rider confirms the trip with a PIN before the vehicle begins the ride.

The UI makes the system state explicit:

> Autonomous mode active  
> Remote assistance available  
> Emergency stop available

### 4. Autonomous ride

The rider should not need to interpret raw telemetry.

Instead, the system surfaces human-readable explanations only when useful.

Examples:

> **Slowing down**  
> Motorcycle entering the vehicle path.

> **Waiting**  
> Pedestrian crossing ahead.

> **Adjusting route**  
> Road obstruction detected.

> **Monsoon Mode**  
> Visibility reduced. Speed and following distance adjusted.

### 5. Ambiguous road situation

If autonomy confidence falls below the operating threshold:

1. vehicle enters a safe state
2. rider sees a clear message
3. remote operator receives context
4. operator selects an allowed action
5. ride resumes or reroutes only when safe

Rider message:

> **Remote assistance requested**  
> The vehicle detected an uncertain road blockage and is waiting safely.

The product should avoid alarming technical language unless action is required.

### 6. Destination and drop-off

Like pickup, the system evaluates the requested drop-off curb.

If the precise destination is unsafe to stop at, it recommends the nearest practical alternative.

### 7. Post-ride feedback

The rider can review:

- ride completed autonomously or with assistance
- number of unusual driving events
- major route changes
- safety explanation timeline

Feedback asks:

- How safe did the ride feel?
- Were the vehicle's actions understandable?
- Was pickup/drop-off convenient?

## Remote operator journey

### Trigger

Remote assistance should be requested only after the vehicle has entered a stable safe state.

### Operator view

The operator receives:

- vehicle ID
- current speed
- autonomy confidence
- reason for assistance request
- map location
- recent perception events
- relevant camera / sensor view when available
- proposed safe options

### Allowed actions

The initial product concept limits operator actions to clear high-level commands:

- continue
- reroute
- reverse slowly
- remain stopped
- disable autonomy / hand off to safety operator

The operator should not be expected to continuously teleoperate the vehicle at road speed.

## Fleet operator journey

The fleet dashboard prioritizes exceptions rather than raw telemetry.

### Fleet overview

Each vehicle shows:

- ride state
- autonomous / assisted mode
- battery
- current location
- intervention count
- active safety alert

### Exception queue

Operators see vehicles requiring attention first:

1. emergency stop
2. remote assistance requested
3. degraded perception / sensor health
4. route blocked
5. low battery / service issue

## Key trust moments

The product should be designed around five moments where rider trust can be lost:

1. finding the vehicle
2. confirming the correct autonomous vehicle
3. unexpected slowing or stopping
4. remote-assistance handoff
5. unusual drop-off location

Each moment needs an explicit UI state and explanation.

## Product principle

**The rider should never have to guess whether the vehicle is confused, waiting intentionally, or experiencing a failure.**
