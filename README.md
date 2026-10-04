# Assignment 2 — Design of a Vector Embedding for Capability Composition

## Student Information

**Name:** Sabeel T  
**Register Number:** LTCR24CS076

---

## Assignment Details

**Assignment:** Assignment 2  
**Title:** Design of a Vector Embedding for Capability Composition

This project implements a problem-specific vector representation for formally specified application states, goals, and executable capabilities.

The implementation focuses on representing capabilities and their relationships required for compatibility, composition, goal relevance, and operational reasoning.

---

## Objective

The objective of this assignment is to design and implement a vector embedding that represents application states, goals, and capabilities while preserving the relationships required for capability composition.

A capability is represented using:

- Capability type
- Inputs
- Outputs
- Preconditions
- Effects
- Constraints
- Resources
- Cost
- Risk
- Reliability
- Availability
- Execution mechanism

The proposed approach uses a hybrid structured vector representation rather than directly applying a generic pretrained embedding model.

---

# Proposed Approach

The proposed representation combines structured and numerical features into a fixed-dimensional vector.

The capability embedding consists of four main components:

1. **Capability Type**
2. **Structural Features**
3. **Semantic Hashed Features**
4. **Operational Features**

The resulting capability vector has **37 dimensions**.

States and goals are represented using deterministic feature encoding and have **24 dimensions**.

### Capability Embedding

The capability embedding is represented as:

```text
E(Ci) = [ T(Ci) || Fstruct(Ci) || Fsem(Ci) || Fop(Ci) ]

where:

T(Ci) = capability type representation
Fstruct(Ci) = structural features
Fsem(Ci) = semantic hashed features
Fop(Ci) = operational features
Capability Representation

Each capability follows the formal representation:

Ci = (Ti, Ii, Oi, Pi, Ei, Ki, Ri, Qi, Reli, Ai, Mi)

where:

Symbol	Description
Ti	Capability type
Ii	Inputs
Oi	Outputs
Pi	Preconditions
Ei	Effects
Ki	Constraints
Ri	Resources
Qi	Cost / quality attributes
Reli	Reliability
Ai	Availability
Mi	Execution mechanism
Vector Dimensions
Component	Dimensions	Purpose
Capability type	9	Represents API, DATABASE, GUI, EVENT, FUNCTION, FILE, COMPUTATION, MESSAGE, SERVICE
Structural features	6	Represents structural properties of the capability
Semantic hashed features	16	Represents structured feature keys and semantic information
Operational features	6	Represents time, resource cost, monetary cost, risk, reliability, and availability
Total	37	Complete capability vector

State and goal representations use 24-dimensional vectors.

Compatibility

Capability compatibility is evaluated using symbolic relationships between the effects and preconditions of capabilities, along with output-input relationships.

The compatibility score is:

Compat(Ci,Cj) =
0.60 × PScore(Ci,Cj) +
0.40 × IOScore(Ci,Cj)

where:

PScore = precondition-effect compatibility score
IOScore = input-output compatibility score

A pair is considered compatible when:

Compatibility Score >= 0.5

Vector similarity is not treated as a replacement for symbolic compatibility.

Similarity

Cosine similarity is used to compare capability vectors.

The combined similarity is:

Sim(Ci,Cj) =
0.70 × Cos(E(Ci),E(Cj)) +
0.30 × OpSim(Ci,Cj)

Operational similarity considers the following attributes:

Time cost
Resource cost
Monetary cost
Risk
Reliability
Availability

This allows capabilities with similar functionality but different operational characteristics to remain distinguishable.

Capability Composition

A valid sequence of capabilities is represented as:

C1 → C2 → ... → Cn

A composite capability is created only when adjacent capabilities satisfy the required compatibility conditions.

The main composition used in the experiments is:

CreateOrder → MakePayment → SendNotification

The composite capability aggregates:

Inputs
Preconditions
Outputs
Effects
Constraints
Resources
Time cost
Resource cost
Monetary cost
Risk
Reliability
Availability
Execution sequence

For example, reliability is aggregated as the product of the component reliabilities:

0.99 × 0.99 × 0.98 = 0.960498
Experiments

Five experiments were implemented.

Experiment 1 — Capability Compatibility

This experiment evaluates whether compatible and incompatible capability pairs can be distinguished.

Tested pairs
CreateOrder → MakePayment
CreateOrder → CancelCart
Results
Capability Pair	Compatibility Score	Decision
CreateOrder → MakePayment	1.000000	Compatible
CreateOrder → CancelCart	0.400000	Incompatible

The compatibility threshold is 0.5.

The results show that the implementation correctly accepts the compatible pair and rejects the incompatible pair.

Experiment 2 — Capability Composition

This experiment constructs a composite capability from three compatible capabilities:

CreateOrder → MakePayment → SendNotification

The composite capability is then compared with each atomic component.

Results
Comparison	Similarity
Composite ↔ CreateOrder	0.742610
Composite ↔ MakePayment	0.830463
Composite ↔ SendNotification	0.703753

The composite does not have an identical vector to each component. Instead, it has different similarity values depending on the component.

Composite Properties
Composite:
CreateOrder → MakePayment → SendNotification

Final effect:

Notification.sent = true

Resources include:

Authentication token
Database
External service
Network
Payment gateway

Composite reliability:

0.960498
Experiment 3 — Alternative Implementations

This experiment evaluates different implementation mechanisms for functionally related capabilities.

The implementations include:

API
DATABASE
GUI

The objective is to maintain functional relationships while still distinguishing implementation types.

Results
Pair	Similarity
API ↔ DATABASE	0.741516
API ↔ GUI	0.864717
DATABASE ↔ GUI	0.700885

The results show that the implementations are related but are not represented as identical capabilities.

Experiment 4 — Irrelevant Capabilities

This experiment evaluates whether capabilities relevant to the target goal can be distinguished from an irrelevant capability.

Results
Capability	Goal Relevance
CreateOrder	1.000
MakePayment	1.000
SendNotification	1.000
PlayMusic	0.000

The three order-processing capabilities are relevant to the specified goal, while PlayMusic is irrelevant.

Experiment 5 — Operational Attributes

This experiment evaluates the effect of operational attributes on capability representation.

Two payment capabilities are compared:

FastPayment
SlowPayment

They perform a similar functional role but have different operational characteristics.

Results
Measure	Value
Combined similarity	0.789137
Operational similarity	0.578333

The results show that the capabilities remain functionally related while their operational differences are reflected in the representation.

Experimental Results Summary
Experiment	Result
Capability Compatibility	Compatible = 1.000, Incompatible = 0.400
Capability Composition	Similarities = 0.743, 0.830, 0.704
Alternative Implementations	Similarities = 0.742, 0.865, 0.701
Irrelevant Capabilities	Relevant = 1.000, Irrelevant = 0.000
Operational Attributes	Combined = 0.789, Operational = 0.578
Project Structure
Assignment-2-Vector-Embedding/
│
├── README.md
├── .gitignore
├── requirements.txt
│
├── src/
│   ├── __init__.py
│   ├── models.py
│   ├── embedding.py
│   ├── compatibility.py
│   └── composition.py
│
├── data/
│   └── dataset.json
│
├── experiments/
│   └── run_all.py
│
├── results/
│   ├── results.csv
│   └── figures/
│       ├── experiment_1.png
│       ├── experiment_2.png
│       ├── experiment_3.png
│       ├── experiment_4.png
│       └── experiment_5.png
│
└── report/
    ├── generate_report.py
    └── technical_report.pdf
File Description
File / Folder	Description
src/models.py	Defines State, Goal, and Capability data models
src/embedding.py	Implements state, goal, and capability encoding and similarity
src/compatibility.py	Implements precondition-effect and input-output compatibility
src/composition.py	Implements capability composition
data/dataset.json	Experimental states, goals, capabilities, constraints, resources, and operational attributes
experiments/run_all.py	Runs all five experiments
results/results.csv	Stores numerical experimental results
results/figures/	Contains experiment result figures
report/generate_report.py	Generates the technical report
report/technical_report.pdf	Final technical report
requirements.txt	Python dependencies
.gitignore	Files excluded from Git
Technologies Used
Python
NumPy
Pandas
Matplotlib
ReportLab
Requirements

The project requires:

numpy
pandas
matplotlib

Report generation additionally requires:

reportlab
Installation
1. Clone the repository
git clone <repository-url>
2. Navigate to the project
cd Assignment-2-Vector-Embedding
3. Create a virtual environment
python3 -m venv venv
4. Activate the virtual environment
macOS / Linux
source venv/bin/activate
Windows
venv\Scripts\activate
5. Install dependencies
python -m pip install -r requirements.txt
Running the Experiments

Run:

python experiments/run_all.py

The experiment runner generates:

results/results.csv

and the five experiment figures:

results/figures/
Generating the Technical Report

Install ReportLab if it is not already installed:

python -m pip install reportlab

Run:

python report/generate_report.py

The generated report will be saved as:

report/technical_report.pdf
Technical Report

The technical report contains the following sections:

Problem Definition
Design Requirements
Related Embedding Approaches
Proposed Representation
Mathematical Formulation
Capability Composition Model
Implementation
Experimental Methodology
Results
Analysis
Limitations
Conclusion
Limitations

The current implementation has several limitations:

The semantic representation uses deterministic hashing, which can result in feature collisions.
Similarity and compatibility weights are manually selected.
The experiments use a relatively small dataset.
Operational attribute normalization uses fixed ranges.
Goal relevance is evaluated using explicit symbolic matching.
Capability composition is explicitly constructed rather than learned from data.
Availability is represented numerically rather than as a time-dependent function.

These limitations provide possible directions for extending the system to larger and more diverse capability datasets.

Conclusion

This project presents a problem-specific vector representation for formally specified application states, goals, and executable capabilities.

The implementation combines:

Capability type
Structural information
Semantic features
Operational attributes

into fixed-dimensional vectors.

The experiments demonstrate that the representation can:

Distinguish compatible and incompatible capabilities
Construct composite capabilities
Represent relationships between alternative implementations
Separate goal-relevant and irrelevant capabilities
Capture operational differences between capabilities

The project uses a hybrid approach in which vector similarity provides graded relationships while symbolic compatibility checks provide the logical conditions required for safe capability composition.