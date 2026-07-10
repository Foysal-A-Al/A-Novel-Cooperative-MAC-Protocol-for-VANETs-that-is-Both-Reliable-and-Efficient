# Cooperative Connectivity and Mobility Simulation for VANETs

MATLAB simulations for studying **vehicle-to-vehicle (V2V)** communication, **vehicle-to-infrastructure (V2I)** connectivity, and freeway mobility behaviour in a Vehicular Ad Hoc Network.

The repository contains interactive connectivity demonstrations and a larger multi-lane traffic simulation intended to support experimentation around reliable and efficient cooperative communication in VANET environments.

---

## Overview

Vehicular Ad Hoc Networks allow vehicles and roadside infrastructure to exchange information without relying entirely on a fixed cellular network.

This repository models three related parts of a VANET environment:

- vehicle-to-vehicle communication
- vehicle-to-roadside-unit communication
- freeway mobility and changing network connectivity

The simulations use vehicle positions and Euclidean distance to determine whether communication links are available within a defined transmission range.

```text
Vehicle mobility
      ↓
Position and distance calculation
      ↓
Communication-range check
      ↓
V2V / V2I connection decision
      ↓
Connectivity visualization and measurement
```

---

## Repository Contents

```text
.
├── V2V.m
├── V2I.m
├── VANET_mobility.m
├── CNN_hands_on_ipynb_txt.ipynb
└── Predictive_Analytics_for_Psychological_Outcomes_with_Blockchain_Data_Integrity.ipynb
```

### Core VANET files

| File | Purpose |
|---|---|
| `V2V.m` | Interactive simulation of communication among moving vehicles |
| `V2I.m` | Interactive simulation of communication between a moving vehicle and roadside units |
| `VANET_mobility.m` | Multi-lane freeway mobility and connectivity simulation |

### Additional notebooks

The repository also currently contains two independent Google Colab notebooks:

- `CNN_hands_on_ipynb_txt.ipynb`
- `Predictive_Analytics_for_Psychological_Outcomes_with_Blockchain_Data_Integrity.ipynb`

These notebooks are not required to run the MATLAB VANET simulations. For a cleaner project structure, they may be moved into a separate `notebooks/` directory or an independent repository.

---

## Main Features

- interactive road creation in MATLAB
- animated movement of vehicles
- V2V distance-based link detection
- V2I communication with roadside units
- communication-range visualization
- random vehicle speed generation
- multi-lane freeway traffic simulation
- vehicle arrival and departure modelling
- entry and exit ramp behaviour
- car-following logic
- lane-changing behaviour
- varying traffic-density experiments
- target-vehicle neighbour analysis
- repeated stochastic simulation runs

---

## V2V Simulation

`V2V.m` demonstrates communication among three moving vehicles.

The user defines two roads by selecting points on a MATLAB figure. Vehicles are then animated along the selected road segments.

At every simulation step, pairwise distances are calculated:

```matlab
ab = pdist([vehicleA; vehicleB], 'euclidean');
bc = pdist([vehicleB; vehicleC], 'euclidean');
ac = pdist([vehicleA; vehicleC], 'euclidean');
```

A temporary communication link is drawn when two vehicles are within the configured range:

```matlab
if ab <= 100
    plot([xa(k), xb(k)], [ya(k), yb(k)], '--g');
end
```

### V2V workflow

```text
Select road geometry
        ↓
Generate random vehicle speeds
        ↓
Move three vehicles
        ↓
Calculate pairwise distances
        ↓
Display active links within 100 units
```

### Run

```matlab
V2V
```

Follow the dialog instructions and click the requested road endpoints.

---

## V2I Simulation

`V2I.m` demonstrates communication between a moving vehicle and two roadside units.

The user interactively defines:

- three connected road segments
- the location of RSU 1
- the location of RSU 2

The vehicle travels along the route while its distance from both roadside units is evaluated.

When the vehicle is within range, a green dashed line represents the active V2I link. When both RSUs are available, the simulation selects the closer unit.

### V2I workflow

```text
Create a three-segment route
          ↓
Place two roadside units
          ↓
Move the vehicle along the route
          ↓
Measure vehicle-to-RSU distances
          ↓
Connect to an available or nearest RSU
```

### Communication rule

The implementation uses a nominal communication threshold of:

```matlab
100
```

in the coordinate system used by the plot.

### Run

```matlab
V2I
```

Select all requested road and RSU points inside the figure.

---

## Freeway Mobility Simulation

`VANET_mobility.m` provides the most detailed simulation in the repository.

It models a four-lane freeway with dynamic vehicle movement and changing network connectivity.

### Default parameters

| Parameter | Default value |
|---|---:|
| Simulation duration | 2 minutes |
| Communication range | 50 m |
| Minimum speed | 22.35 m/s |
| Maximum speed | 31.29 m/s |
| Minimum acceleration | 0 m/s² |
| Maximum acceleration | 5 m/s² |
| Safety distance | 10 m |
| Arrival probability/rate | 0.833 |
| Departure probability/rate | 0.833 |
| Road length | 5000 m |
| Traffic-density range | 100–500 vehicles per lane |
| Density increment | 50 vehicles |
| Repeated runs per density | 5 |

The speed range corresponds approximately to 50–70 mph.

### Mobility behaviour

The simulation includes:

- initialization of four traffic lanes
- random vehicle speed and acceleration
- stochastic vehicle entry
- stochastic vehicle departure
- three possible entry ramps
- three possible exit ramps
- road-border handling
- longitudinal freeway movement
- car-following constraints
- safety-distance checks
- lane-changing decisions
- communication-neighbour calculation
- target-vehicle connectivity analysis

### Traffic density

Experiments are performed over:

```matlab
traffic_density = 100:50:500;
```

This allows connectivity behaviour to be examined under increasingly dense traffic conditions.

### Time resolution

Vehicle state updates are performed using an inner simulation interval of:

```matlab
0.1 seconds
```

### Run

```matlab
VANET_mobility
```

The simulation is stochastic, so results can differ between runs.

For reproducibility, set the random seed before execution:

```matlab
rng(42);
VANET_mobility
```

---

## Requirements

### Software

- MATLAB
- Statistics and Machine Learning Toolbox

The code uses functions including:

```matlab
pdist
fzero
polyfit
ginput
randi
```

The notebook files require a separate Python or Google Colab environment and are not dependencies of the MATLAB simulations.

---

## Installation

Clone the repository:

```bash
git clone https://github.com/Foysal-A-Al/A-Novel-Cooperative-MAC-Protocol-for-VANETs-that-is-Both-Reliable-and-Efficient.git
```

Open the project directory:

```bash
cd A-Novel-Cooperative-MAC-Protocol-for-VANETs-that-is-Both-Reliable-and-Efficient
```

In MATLAB, add the repository directory to the path:

```matlab
addpath(genpath(pwd));
```

Then run one of the simulations:

```matlab
V2V
```

```matlab
V2I
```

```matlab
VANET_mobility
```

---

## Visual Interpretation

### V2V

- square markers represent vehicles
- green dashed lines indicate active V2V links
- a link exists when the vehicle separation is within the communication range

### V2I

- square marker represents the moving vehicle
- filled points represent roadside units
- green dashed lines indicate active infrastructure links
- green vehicle points indicate connectivity
- red vehicle points indicate that no RSU is within range

---

## Simulation Assumptions

The current implementation makes several simplifying assumptions:

- communication availability depends mainly on Euclidean distance
- radio propagation, fading, interference, and packet collisions are not explicitly modelled
- the interactive V2V and V2I figures use abstract Cartesian coordinates
- vehicles follow straight road segments
- communication thresholds are fixed
- mobility behaviour is stochastic
- road geometry and lane behaviour are simplified
- the MATLAB files demonstrate connectivity and mobility rather than a complete IEEE 802.11p or C-V2X protocol stack

These assumptions make the project suitable for conceptual simulation and extension, but not for direct prediction of real-world network performance.

---

## Relationship to a Cooperative MAC Protocol

The current code provides several building blocks needed for cooperative MAC research:

- vehicle mobility
- changing network topology
- neighbour discovery
- V2V availability
- V2I availability
- roadside-unit selection
- communication-range checks
- traffic-density variation

A complete cooperative MAC evaluation would additionally need explicit modelling of:

- channel access
- contention windows
- control and service channels
- packet generation
- packet queues
- packet collisions
- retransmission
- relay selection
- cooperative forwarding
- acknowledgement handling
- channel busy ratio
- latency
- packet delivery ratio
- throughput
- fairness
- protocol overhead

The repository should therefore be described as a **mobility and connectivity simulation foundation** unless those protocol mechanisms are implemented elsewhere.

---

## Recommended Experimental Metrics

Future evaluations can record:

| Metric | Purpose |
|---|---|
| Packet delivery ratio | Measures successful packet delivery |
| End-to-end delay | Measures communication latency |
| Throughput | Measures successful data transfer rate |
| Collision rate | Measures channel-contention impact |
| Connectivity probability | Measures the likelihood of having a link |
| Average neighbour count | Measures local network density |
| Link duration | Measures connection stability |
| Handover count | Measures V2I switching frequency |
| Control overhead | Measures protocol signalling cost |
| Channel busy ratio | Measures channel congestion |

Results should be reported across multiple random seeds using the mean, standard deviation, and confidence intervals.

---

## Known Limitations

### Interactive geometry

`V2V.m` and `V2I.m` require manual point selection with `ginput`. This is useful for demonstrations but makes automated benchmarking difficult.

A future version should accept road coordinates as function arguments.

### Vertical road segments

The road equations divide by the difference between x-coordinates. Selecting a perfectly vertical road can cause division-by-zero problems.

### Abstract coordinate units

The interactive simulations use axes from 0 to 1000 but do not formally define whether every unit represents one metre.

### Communication models differ

The interactive scripts use a threshold of 100 coordinate units, while the freeway mobility simulation defines a communication range of 50 metres.

These parameters should be centralized in a shared configuration.

### Limited radio model

Distance alone does not capture:

- path loss
- shadowing
- Doppler effects
- multipath fading
- interference
- antenna characteristics
- channel congestion

### Global variables

`VANET_mobility.m` relies on several global variables. This makes the simulation harder to test and maintain.

A configuration structure would be preferable.

### Reproducibility

The simulations use random values without setting a seed by default.

### Scalability

Repeated dynamic array modification and nested loops may become slow at high traffic densities.

### File organization

The two Jupyter notebooks are unrelated to the VANET MATLAB project and reduce repository clarity.

---

## Suggested Repository Structure

A clearer organization would be:

```text
.
├── README.md
├── LICENSE
├── matlab/
│   ├── V2V.m
│   ├── V2I.m
│   └── VANET_mobility.m
├── notebooks/
│   ├── CNN_hands_on_ipynb_txt.ipynb
│   └── Predictive_Analytics_for_Psychological_Outcomes_with_Blockchain_Data_Integrity.ipynb
├── results/
├── figures/
└── docs/
```

For stronger project coherence, consider moving the notebooks into separate repositories.

---

## Recommended Improvements

- separate simulation configuration from implementation
- replace global variables with structures or classes
- add configurable random seeds
- automate road and RSU placement
- export results to CSV or MAT files
- record connectivity statistics at every timestep
- add packet-level network behaviour
- implement a formal cooperative MAC state machine
- compare cooperative and non-cooperative baselines
- add statistical result aggregation
- add unit tests for mobility functions
- add plotting functions for density-versus-connectivity results
- document all helper functions
- improve error handling for invalid road selections
- support batch experiments without graphical interaction
- integrate SUMO mobility traces
- integrate Veins, OMNeT++, NS-3, or MATLAB network simulation components

---

## Example Batch-Experiment Design

A future automated experiment could use:

```matlab
seeds = 1:30;
densities = 100:50:500;

for density = densities
    for seed = seeds
        rng(seed);

        result = run_vanet_simulation( ...
            'TrafficDensity', density, ...
            'CommunicationRange', 50, ...
            'Duration', 120);

        save_experiment_result(result);
    end
end
```

The aggregated result could then report:

```matlab
mean_connectivity
std_connectivity
mean_neighbor_count
mean_link_duration
packet_delivery_ratio
end_to_end_delay
```

---

## Troubleshooting

### `Undefined function 'pdist'`

Install or enable the Statistics and Machine Learning Toolbox.

### The simulation stops after opening a figure

The V2V and V2I scripts are waiting for mouse input. Follow the dialog and click the requested points inside the MATLAB figure.

### Division-by-zero or invalid coordinates

Avoid selecting road endpoints with identical x-coordinates.

### Vehicles do not appear to connect

Place vehicles or roadside units closer together, or increase the communication threshold.

### Results differ every time

The simulation uses random speeds, arrivals, departures, and vehicle selection.

Set:

```matlab
rng(42);
```

before running the simulation.

### The mobility simulation is slow

Reduce:

- simulation duration
- traffic-density range
- number of repeated runs
- number of vehicles
- plotting frequency

---

## Research Use

This repository can support educational or exploratory work in:

- VANET connectivity
- cooperative communication
- intelligent transportation systems
- vehicular mobility modelling
- roadside infrastructure placement
- network-density analysis
- neighbour discovery
- connectivity-aware MAC design

For publication-quality results, the mobility, channel, MAC, and evaluation models should be fully specified and validated against recognised simulators or real mobility traces.

---

## Citation

When using this repository, cite the repository URL and clearly identify the version or commit used.

A `CITATION.cff` file can be added:

```yaml
cff-version: 1.2.0
message: "If you use this software, please cite it."
title: "Cooperative Connectivity and Mobility Simulation for VANETs"
type: software
authors:
  - family-names: "Abdullah Al"
    given-names: "Foysal"
repository-code: "https://github.com/Foysal-A-Al/A-Novel-Cooperative-MAC-Protocol-for-VANETs-that-is-Both-Reliable-and-Efficient"
```

---

## License

No license 

---

## Disclaimer

This repository is an academic and experimental simulation.

It does not model every physical-layer, MAC-layer, safety, timing, or regulatory requirement of a production vehicular communication system. It should not be used directly for safety-critical transportation decisions.
