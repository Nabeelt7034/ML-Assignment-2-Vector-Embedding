where:

T(Ci) = capability type representation
Fstruct(Ci) = structural features
Fsem(Ci) = semantic hashed features
Fop(Ci) = operational features
Compatibility

Compatibility is evaluated using symbolic precondition-effect and input-output relationships.

The compatibility score is:

Compat(Ci,Cj) =
0.60 × PScore(Ci,Cj) +
0.40 × IOScore(Ci,Cj)

A capability pair is considered compatible when the score is at least 0.5.

Vector similarity and symbolic compatibility are treated as different concepts.

Similarity

Cosine similarity is used for vector comparison.

The combined similarity is:

Sim(Ci,Cj) =
0.70 × Cos(E(Ci),E(Cj)) +
0.30 × OpSim(Ci,Cj)

Operational similarity considers:

Time cost
Resource cost
Monetary cost
Risk
Reliability
Availability
Capability Composition

A valid sequence of capabilities is represented as:

C1 → C2 → ... → Cn

A composite capability is constructed only when adjacent capabilities satisfy the required compatibility conditions.

For the main experiment:

CreateOrder → MakePayment → SendNotification

The composite capability aggregates:

Inputs
Preconditions
Outputs
Effects
Constraints
Resources
Cost
Risk
Reliability
Availability
Execution sequence
Experiments

Five experiments were implemented as required by the assignment.

Experiment 1 — Capability Compatibility

This experiment checks whether compatible and incompatible capability pairs can be distinguished.

Results
Capability Pair	Compatibility Score	Result
CreateOrder → MakePayment	1.000000	Compatible
CreateOrder → CancelCart	0.400000	Incompatible

The compatibility threshold is 0.5.

Experiment 2 — Capability Composition

The following capabilities are composed:

CreateOrder → MakePayment → SendNotification

The composite capability is compared with each component.

Results
Comparison	Similarity
Composite ↔ CreateOrder	0.742610
Composite ↔ MakePayment	0.830463
Composite ↔ SendNotification	0.703753

The composite does not receive an identical vector to its components. Instead, it maintains different degrees of similarity to each component.

Experiment 3 — Alternative Implementations

Alternative implementations of similar functionality are compared using different execution mechanisms.

The implementations include:

API
DATABASE
GUI
Results
Pair	Similarity
API ↔ DATABASE	0.741516
API ↔ GUI	0.864717
DATABASE ↔ GUI	0.700885

The results show that functionally related capabilities can remain similar while their implementation types remain distinguishable.

Experiment 4 — Irrelevant Capabilities

This experiment evaluates whether capabilities contributing to the specified goal can be distinguished from an irrelevant capability.

Results
Capability	Goal Relevance
CreateOrder	1.000
MakePayment	1.000
SendNotification	1.000
PlayMusic	0.000

The order-processing capabilities are relevant to the target goal, while PlayMusic is irrelevant.

Experiment 5 — Operational Attributes

This experiment compares two payment capabilities with different operational properties.

FastPayment
SlowPayment
Results
Measure	Value
Combined similarity	0.789137
Operational similarity	0.578333

The results show that capabilities can remain functionally related while having different operational characteristics.

Project Structure
Assignment-2-Vector-Embedding/
│
├── README.md
├── .gitignore
├── requirements.txt
│
├── src/
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
│
└── report/
    ├── generate_report.py
    └── technical_report.pdf
Technologies Used
Python
NumPy
Pandas
Matplotlib
ReportLab
Running the Project
1. Create virtual environment
python3 -m venv venv
2. Activate virtual environment
source venv/bin/activate
3. Install dependencies
python -m pip install -r requirements.txt
4. Run experiments
python experiments/run_all.py

The experiment runner generates:

results/results.csv
results/figures/
5. Generate the technical report

Install ReportLab if required:

python -m pip install reportlab

Then run:

python report/generate_report.py

The report is generated at:

report/technical_report.pdf
Results Summary
Experiment	Main Result
Compatibility	Compatible pair = 1.000, incompatible pair = 0.400
Composition	Composite similarities = 0.743, 0.830, 0.704
Alternative implementations	Similarity = 0.742, 0.865, 0.701
Goal relevance	Relevant = 1.000, irrelevant = 0.000
Operational attributes	Combined = 0.789, operational = 0.578
Conclusion

The implementation provides a problem-specific vector representation for application states, goals, and executable capabilities.

The experiments demonstrate that the representation can:

Distinguish compatible and incompatible capabilities
Represent composite capabilities
Preserve relationships between alternative implementations
Separate relevant and irrelevant capabilities
Represent operational differences