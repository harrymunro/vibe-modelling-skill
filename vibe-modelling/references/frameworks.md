# Vibe Modelling Frameworks Reference

## The OSVC Framework: Worked Examples

The OSVC (Objective, System, Variables, Constraints) framework transforms vague goals into simulation-ready questions.

### Template

> "What is the impact on **[Objective]** in **[System]** if we change **[Variable]**, given **[Constraints]**?"

### Example 1: Warehouse Fulfilment

**Before**: "Improve warehouse efficiency"

**After applying OSVC**:
- **Objective**: Reduce average order fulfilment time (minutes)
- **System**: The forklift-based picking process for standard orders
- **Variable**: Routing algorithm for forklift task assignment
- **Constraint**: No major operational disruption; budget under £50k

**Simulation-ready question**: "What is the impact on average order fulfilment time in the forklift-based picking process if we implement an optimised routing algorithm, provided it causes no operational disruption and costs under £50k to implement?"

### Example 2: Call Centre

**Before**: "Make the call centre better"

**After applying OSVC**:
- **Objective**: Reduce call abandonment rate from 15% to under 5%
- **System**: Incoming call queue and agent assignment process (9am-11am peak)
- **Variable**: Number of agents rostered during peak morning hours
- **Constraint**: Weekly staff budget cannot increase by more than 10%

**Simulation-ready question**: "How many additional agents do we need between 9am and 11am to reduce call abandonment to under 5%, without exceeding a 10% increase in weekly wage budget?"

### Example 3: Hospital A&E

**Before**: "A&E waiting times are too long"

**After applying OSVC**:
- **Objective**: Reduce average wait time for non-critical patients to under 2 hours
- **System**: Patient journey from arrival through triage, consultation, to discharge (weekend evenings, 7pm-midnight)
- **Variable**: Staffing configuration (triage nurses, doctors, workflow rules)
- **Constraint**: Existing physical infrastructure; current budget envelope

**Simulation-ready question**: "What staffing configuration during weekend evenings will reduce average non-critical patient wait time from 4 hours to under 2 hours, using existing infrastructure?"

### Example 4: Manufacturing Line

**Before**: "We need more capacity"

**After applying OSVC**:
- **Objective**: Increase throughput from 18 to 24 units/hour
- **System**: Surface-mount assembly line (from component staging to quality check)
- **Variable**: Changeover rules between product variants
- **Constraint**: No capital expenditure over £100k; maintain defect rate below 1.5%

**Simulation-ready question**: "Can optimised changeover rules increase throughput to 24 units/hour without exceeding £100k investment or raising defect rate above 1.5%?"

---

## The Three Bricks: System Decomposition Guide

Every operational system can be decomposed into three building blocks.

### Brick 1: Entities

The things that **flow through** the system. They are created, they move, they are processed, they leave.

| Domain | Entity Examples | Key Properties |
|--------|----------------|----------------|
| Retail | Customers, orders | Type, arrival time, patience, basket value |
| Healthcare | Patients | Priority/severity, arrival time, condition type |
| Manufacturing | Parts, products | Type, batch size, quality grade |
| Logistics | Parcels, vehicles | Size, destination, urgency class |
| IT | Tickets, requests | Priority, complexity, SLA deadline |

**Properties to define for each entity:**
- How do they arrive? (rate, pattern, distribution)
- Do they have types/categories that affect processing?
- Do they have patience/timeout behaviour?
- What attributes determine their routing?

### Brick 2: Resources

The things entities **need to use**. They have limited capacity and are the source of bottlenecks.

| Domain | Resource Examples | Key Properties |
|--------|-------------------|----------------|
| Retail | Tills, staff, fitting rooms | Capacity, schedule, speed |
| Healthcare | Doctors, nurses, beds, scanners | Capacity, shift pattern, speciality |
| Manufacturing | Machines, operators, tools | Capacity, breakdown rate, changeover time |
| Logistics | Loading bays, drivers, vehicles | Capacity, schedule, location |
| IT | Agents, servers, licences | Capacity, skill level, availability |

**Properties to define for each resource:**
- Capacity (how many entities can it serve simultaneously?)
- Schedule (when is it available?)
- Setup/changeover time between entities?
- Breakdown/failure probability?
- Does skill level affect processing time?

### Brick 3: Processes

The **actions** that happen when an entity successfully seizes a resource. Always takes time.

| Domain | Process Examples | Key Properties |
|--------|------------------|----------------|
| Retail | Serving, checkout, returns | Duration distribution, steps |
| Healthcare | Triage, consultation, treatment | Duration, resource requirements |
| Manufacturing | Machining, assembly, inspection | Duration, yield rate, sequence |
| Logistics | Loading, sorting, delivery | Duration, capacity consumed |
| IT | Diagnosis, resolution, escalation | Duration by complexity tier |

**Properties to define for each process:**
- Duration (fixed, range, or statistical distribution?)
- Does duration depend on entity type or resource skill?
- Are there multiple sequential steps?
- What triggers the process to start?
- What happens when it completes?

---

## The Data-Assumption Spectrum

### Level 1: Known Facts
- Source: Internal systems, databases, ERP
- Examples: Number of orders yesterday, shift rota, machine count
- Action: Use directly with high confidence

### Level 2: Direct Observations
- Source: Stopwatch studies, manual counts, logs
- Examples: Timed processes, counted arrivals over a period
- Action: Use with awareness of sample size and observer effect

### Level 3: Educated Guesses
- Source: Expert interviews, colleague estimates, industry benchmarks
- Examples: "It usually takes about 10 minutes, could be 5-20"
- Action: Document the assumption, plan sensitivity testing, note source

### Level 4: Wild Guesses
- Source: No data, no expert, pure invention
- Examples: Numbers with no basis in observation or expertise
- Action: Danger zone. Try to improve to Level 3 before using. Flag prominently.

### Documentation Template

For each assumption, record:

```
Parameter: [name]
Value: [number or range]
Level: [1-4]
Source: [where this came from]
Confidence: [high/medium/low]
Sensitivity: [how much does the output change if this is wrong?]
```

---

## Model Fidelity Levels (The Helicopter Analogy)

### Low Fidelity (5,000 feet / Napkin Sketch)
- Strategic overview
- Broad flows, aggregate behaviour
- Useful for: Bounding the problem, initial scoping
- Example: Arrivals -> Department -> Departures

### Medium Fidelity (500 feet / Detailed Blueprint)
- Specific capacities, distributions, queue logic
- Individual entity types and resource pools
- Useful for: Most business questions (start here)
- Example: Patient arrives -> Queue -> Triage (3 nurses, 5 min) -> Queue -> Doctor (5 doctors, 12 min) -> Discharge

### High Fidelity (Street Level / Digital Twin)
- Individual staff patterns, physical layout, dozens of sub-types
- Useful for: Only when medium fidelity is insufficient
- Warning: Expensive, slow, data-hungry. Start simple and add complexity only if needed.

**Rule of thumb**: Start at medium fidelity. Only go higher if your results are insensitive to simplifications that clearly matter.
