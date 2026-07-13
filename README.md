# Outpatient Clinic Patient-Flow and Staffing Optimisation

This repository contains a notebook-based case study that uses synthetic outpatient clinic data to assess doctor staffing across four 2-hour clinic blocks. The analysis compares patient consultation demand against available doctor capacity, identifies workload pressure, evaluates staffing tradeoffs, and simulates waiting times under alternative staffing scenarios.

## Repository Contents

- `outpatient_clinic_or_case_study.ipynb`: main analysis notebook.
- `doctor_availability_synthetic.csv`: synthetic doctor capacity by clinic day and time block.
- `patient_visits_synthetic.csv`: synthetic patient appointments, arrivals, consultation times, and no-show indicators.
- `staffing_scenarios_synthetic.csv`: alternative staffing assignments used in the scenario comparison.
- `model_parameters_synthetic.csv`: cost and service-level assumptions used in the optimisation logic.

## Requirements

The notebook uses Python with `pandas` and `numpy`. `jupyter` and `ipykernel` are included so the case study can be run locally as a notebook.

Install dependencies with:

```bash
pip install -r requirements.txt
```

## How to Run

1. Install the dependencies.
2. Start Jupyter:

   ```bash
   jupyter notebook
   ```

3. Open `outpatient_clinic_or_case_study.ipynb`.
4. Run the notebook from top to bottom.

## Analysis Scope

The notebook is organized around five tasks:

1. Load and validate the synthetic clinic datasets.
2. Profile outpatient demand, consultation duration, attendance, and no-show patterns.
3. Measure workload pressure by comparing consultation demand with doctor capacity by time block.
4. Evaluate staffing options using a simple cost-penalty optimisation rule.
5. Simulate queue waiting times under two doctor-allocation scenarios.

## Data Preparation and Assumptions

- No-show patients are retained for demand profiling but excluded from consultation workload and queue simulation because they do not enter the service process.
- Patient IDs are checked for duplicates before downstream analysis.
- Capacity is evaluated in four 2-hour time blocks:
  - `08:00-10:00`
  - `10:00-12:00`
  - `13:00-15:00`
  - `15:00-17:00`
- Workload gap is defined as total consultation demand minus available doctor minutes.
- Positive workload gap indicates a shortage of capacity; negative gap indicates spare capacity.

## Notebook Workflow

### 1. Data quality checks

The notebook loads the four CSV files into pandas DataFrames and performs basic completeness checks for:

- doctor availability,
- patient demand and arrivals,
- consultation duration,
- no-show behavior,
- staffing scenarios, and
- model parameters.

### 2. Operational demand analysis

The analysis explores:

- high-demand clinic days and time blocks,
- service types with the longest average consultation time,
- no-show rate,
- arrival timing relative to appointment time, and
- likely pressure points in the clinic schedule.

The notebook notes, for example, that:

- Clinic A shows the highest appointment demand, especially in the morning blocks.
- `Complex Care Review` has the highest average consultation duration.
- The synthetic data produces an approximately 10% no-show rate.

### 3. Capacity and utilisation analysis

Patient demand is merged with doctor availability to calculate:

- total consultation minutes,
- available doctor minutes,
- workload gap, and
- utilisation rate.

Blocks with utilisation above 100% are flagged as capacity-stressed periods. The notebook also identifies blocks with spare capacity that may support reallocation, subject to operational constraints.

### 4. Staffing optimisation logic

The case study uses the model parameters file to apply a simple objective based on:

- doctor cost per hour,
- waiting-time penalty per patient-minute, and
- overtime penalty assumptions.

Two related recommendation views are built:

- a workload-gap adjustment view that estimates whether current staffing should be maintained, reviewed, or reallocated; and
- a cost comparison across one-doctor, two-doctor, and maximum-doctor options for each time block.

When one-doctor and two-doctor options tie on objective score, the notebook favors two doctors to reduce workload pressure without automatically selecting the maximum staffing level.

### 5. Queue simulation scenario comparison

The notebook finishes with a simple first-available-doctor simulation for the same patient cohort in one time block under two scenarios:

- `Baseline_2_Doctors_All_Day`
- `Extra_3_Doctors_All_Day`

Patients are processed in actual arrival order and assigned to the doctor who becomes available first. The simulation calculates start time, waiting time, and consultation end time for each patient.

The notebook's stated expectation is that the three-doctor scenario reduces average and upper-percentile waiting times compared with the baseline two-doctor scenario, and that the final decision should weigh service improvement against added staffing cost.

## Intended Use

This repository is useful as a compact case study for:

- outpatient operations analysis,
- staffing and capacity planning,
- queueing and waiting-time illustration, and
- notebook-based healthcare analytics prototyping with synthetic data.

## Notes

- All data in this repository is synthetic.
- The optimisation and simulation logic are intentionally lightweight and intended for case-study demonstration rather than production scheduling.