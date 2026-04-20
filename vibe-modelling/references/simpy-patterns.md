# SimPy Patterns Reference

## Common Simulation Architectures

### Pattern 1: Simple Queue (Single Resource)

The most basic pattern. Entities arrive, queue for a single resource, get processed, and leave.

**Use for**: Coffee shops, single-machine processes, one-person service desks.

```python
import simpy
import random

def customer(env, name, server, results):
    """Entity arrives, waits for resource, gets served, leaves."""
    arrival_time = env.now
    
    with server.request() as req:
        yield req  # Wait in queue
        wait_time = env.now - arrival_time
        
        # Process (service takes time)
        service_time = random.uniform(3, 7)
        yield env.timeout(service_time)
        
        results.append({
            'name': name,
            'wait': wait_time,
            'service': service_time
        })

def arrivals(env, server, results):
    """Generate entities at random intervals."""
    i = 0
    while True:
        yield env.timeout(random.expovariate(1/3))  # Average 3 min between arrivals
        i += 1
        env.process(customer(env, f'Customer_{i}', server, results))

# Setup and run
env = simpy.Environment()
server = simpy.Resource(env, capacity=1)
results = []
env.process(arrivals(env, server, results))
env.run(until=480)  # Run for 480 minutes (8 hours)
```

### Pattern 2: Balking (Customer Patience)

Entities check queue length before joining. If too long, they leave immediately.

**Use for**: Any system where customers/entities can choose to leave.

```python
def customer_with_balking(env, name, server, max_queue, results):
    """Entity checks queue length before joining."""
    arrival_time = env.now
    
    # Check if queue is too long (balking)
    if len(server.queue) + server.count >= max_queue:
        results['balked'] += 1
        return  # Leave immediately
    
    with server.request() as req:
        yield req
        service_time = random.uniform(4, 6)
        yield env.timeout(service_time)
        results['served'] += 1
        results['revenue'] += random.uniform(3.0, 5.0)
```

### Pattern 3: Multiple Resources in Sequence

Entities must pass through several resources in order (e.g. reception -> triage -> doctor).

**Use for**: Hospital pathways, manufacturing lines, multi-step service processes.

```python
def patient(env, name, reception, triage_nurse, doctor, results):
    """Entity flows through multiple resources sequentially."""
    arrival_time = env.now
    
    # Step 1: Reception
    with reception.request() as req:
        yield req
        yield env.timeout(random.uniform(2, 5))  # Check-in time
    
    # Step 2: Triage
    with triage_nurse.request() as req:
        yield req
        yield env.timeout(random.uniform(5, 10))  # Triage assessment
    
    # Step 3: Doctor consultation
    with doctor.request() as req:
        yield req
        yield env.timeout(random.normalvariate(12, 4))  # Consultation
    
    total_time = env.now - arrival_time
    results.append({'name': name, 'total_time': total_time})
```

### Pattern 4: Priority Queues

Different entity types get different priority levels.

**Use for**: A&E triage, priority support tickets, express lanes.

```python
def entity_with_priority(env, name, priority, server, results):
    """Entity with priority level (lower number = higher priority)."""
    arrival_time = env.now
    
    # PriorityResource gives preference to lower priority numbers
    with server.request(priority=priority) as req:
        yield req
        service_time = random.uniform(5, 15)
        yield env.timeout(service_time)
        
        results.append({
            'name': name,
            'priority': priority,
            'wait': env.now - arrival_time - service_time
        })

# Use PriorityResource instead of Resource
server = simpy.PriorityResource(env, capacity=3)
```

### Pattern 5: Resource with Breakdown

Resources that periodically fail and need repair.

**Use for**: Manufacturing machines, servers, equipment with downtime.

```python
def machine_breakdown(env, machine, repair_time_mean):
    """Periodically break the machine."""
    while True:
        # Time until next breakdown
        yield env.timeout(random.expovariate(1/120))  # Average 120 min between failures
        
        if machine.count > 0:  # Only break if in use
            machine.interrupt()  # Interrupt current user

def part_on_machine(env, name, machine, results):
    """Entity using a resource that might break down."""
    with machine.request() as req:
        yield req
        
        processing_time = random.uniform(8, 12)
        start = env.now
        
        while processing_time > 0:
            try:
                yield env.timeout(processing_time)
                processing_time = 0  # Completed successfully
            except simpy.Interrupt:
                # Machine broke down
                processing_time -= (env.now - start)
                # Wait for repair
                yield env.timeout(random.uniform(10, 30))
                start = env.now
        
        results.append({'name': name, 'completed': env.now})
```

### Pattern 6: Shift Patterns

Resources available only during certain times.

**Use for**: Staff with shifts, machines with maintenance windows, opening hours.

```python
def shift_controller(env, resource, shift_start, shift_end):
    """Make resource available only during shift hours."""
    while True:
        # Wait until shift starts
        current_hour = env.now % 1440  # Minutes in a day
        if current_hour < shift_start * 60:
            yield env.timeout(shift_start * 60 - current_hour)
        elif current_hour >= shift_end * 60:
            yield env.timeout((24 - current_hour/60 + shift_start) * 60)
        
        # Shift is active - resource available
        shift_duration = (shift_end - shift_start) * 60
        yield env.timeout(shift_duration)
        
        # Shift ended - seize all capacity to block access
        requests = []
        for _ in range(resource.capacity):
            req = resource.request()
            requests.append(req)
        
        # Hold until next shift
        yield env.timeout((24 - shift_end + shift_start) * 60)
        
        # Release for next shift
        for req in requests:
            resource.release(req)
```

### Pattern 7: Batch Processing

Multiple entities processed together.

**Use for**: Ovens, batch chemical processes, shuttle buses, group training.

```python
def batch_processor(env, batch_size, process_time, input_store, results):
    """Collect entities into batches, process together."""
    while True:
        batch = []
        # Collect batch_size entities
        for _ in range(batch_size):
            item = yield input_store.get()
            batch.append(item)
        
        # Process entire batch
        yield env.timeout(process_time)
        
        results['batches_processed'] += 1
        results['items_processed'] += len(batch)
```

### Pattern 8: What-If Scenario Runner

Run multiple scenarios and compare results.

**Use for**: Any comparative analysis, sensitivity testing.

```python
def run_scenario(params):
    """Run a single scenario with given parameters."""
    env = simpy.Environment()
    # ... set up simulation with params ...
    env.run(until=params['duration'])
    return results

# Define scenarios
scenarios = [
    {'name': 'Baseline', 'service_time': 5, 'num_servers': 1},
    {'name': 'Faster Service', 'service_time': 3, 'num_servers': 1},
    {'name': 'Extra Staff', 'service_time': 5, 'num_servers': 2},
]

# Run all scenarios
all_results = {}
for scenario in scenarios:
    all_results[scenario['name']] = run_scenario(scenario)

# Compare
for name, result in all_results.items():
    print(f"{name}: Served={result['served']}, Revenue=£{result['revenue']:.2f}")
```

---

## Statistical Distributions Quick Reference

| Real-World Pattern | Distribution | SimPy/Python Code | When to Use |
|---|---|---|---|
| Random arrivals | Exponential | `random.expovariate(1/mean)` | Customer arrivals, breakdowns |
| "About X, could be Y-Z" | Uniform | `random.uniform(low, high)` | When you only know min/max |
| Centred around average | Normal | `random.normalvariate(mean, std)` | Service times, processing |
| Mostly quick, sometimes long | Log-normal | `random.lognormvariate(mu, sigma)` | Repair times, complex tasks |
| Fixed time | Constant | `yield env.timeout(fixed_value)` | Machine cycles, fixed steps |
| "Between X and Y, usually near Z" | Triangular | `random.triangular(low, high, mode)` | Expert estimates with mode |

**Tip for non-technical users**: When someone says "it takes about 10 minutes but can be anywhere from 5 to 20", use `random.triangular(5, 20, 10)`.

---

## Visualisation Patterns

### Basic KPI Dashboard

```python
import matplotlib.pyplot as plt

fig, axes = plt.subplots(2, 2, figsize=(12, 8))
fig.suptitle('Simulation Results Dashboard', fontsize=14)

# Queue length over time
axes[0, 0].plot(time_points, queue_lengths)
axes[0, 0].set_title('Queue Length Over Time')
axes[0, 0].set_xlabel('Time (minutes)')
axes[0, 0].set_ylabel('Queue Length')

# Cumulative customers
axes[0, 1].plot(time_points, served, label='Served')
axes[0, 1].plot(time_points, balked, label='Balked', linestyle='--')
axes[0, 1].set_title('Cumulative Customers')
axes[0, 1].legend()

# Wait time distribution
axes[1, 0].hist(wait_times, bins=20, edgecolor='black')
axes[1, 0].set_title('Wait Time Distribution')
axes[1, 0].set_xlabel('Wait Time (minutes)')

# Resource utilisation
axes[1, 1].bar(resource_names, utilisation_pcts)
axes[1, 1].set_title('Resource Utilisation (%)')
axes[1, 1].set_ylim(0, 100)

plt.tight_layout()
plt.show()
```

### Scenario Comparison Chart

```python
scenarios = ['Baseline', 'Faster Service', 'Extra Staff']
metrics = [served_baseline, served_faster, served_extra]

plt.figure(figsize=(10, 6))
bars = plt.bar(scenarios, metrics, color=['#2196F3', '#4CAF50', '#FF9800'])
plt.title('Customers Served by Scenario')
plt.ylabel('Customers Served')

# Add value labels on bars
for bar, val in zip(bars, metrics):
    plt.text(bar.get_x() + bar.get_width()/2, bar.get_height() + 1,
             str(val), ha='center', fontsize=12)

plt.tight_layout()
plt.show()
```

---

## Troubleshooting Common Issues

| Symptom | Likely Cause | Fix |
|---------|-------------|-----|
| Queue grows infinitely | Arrival rate > service rate | Reduce arrivals or increase capacity |
| Zero wait times | Resource capacity too high | Check capacity matches reality |
| Negative times | Distribution can go negative | Use `max(0, value)` or bounded distribution |
| Results vary wildly between runs | Small sample / short duration | Run longer or use multiple replications |
| Entity accounting doesn't balance | Counting at wrong point | Ensure: arrived = served + balked + in_system |
| All entities served instantly | Resource not being seized properly | Check `with resource.request() as req: yield req` pattern |
| Model runs forever | Infinite loop in process | Check all `yield` statements are present |

## Verification Checklist

Before presenting results to stakeholders:

- [ ] Entity accounting: arrivals = served + balked + in_progress
- [ ] Queue lengths stay within defined bounds
- [ ] Resource utilisation is between 0% and 100%
- [ ] Service times match the distributions specified
- [ ] Results are stable across multiple random seeds
- [ ] Extreme scenarios produce sensible results
- [ ] Time units are consistent throughout (all minutes OR all hours, not mixed)
- [ ] The model matches the conceptual design (no missing steps)
