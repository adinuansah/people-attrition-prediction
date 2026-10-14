# Data Dictionary — Employee Retention Intelligence

## Dataset overview

This project uses simulated HRIS-style data for learning People Analytics and predictive modeling.

**Observation window:** January 2022 – December 2025

**Population:** 2,000 synthetic employee records, including employees hired before and during the observation window.

**Employment model:** One employment spell per employee; no rehires.

**Important:** All workforce distributions, salaries, departures, and exit reasons are simulated and should not be interpreted as real organizational findings.

## 1. employees.csv

**Grain:** One row per employee.

**Primary key:** `employee_id`

| Column | Type | Description |
|---|---|---|
| employee_id | String | Unique employee ID |
| hire_date | Date | Original hire date |
| initial_department | String | Department assigned at hiring |
| initial_job_level | String | Job level assigned at hiring, L1–L6 |
| initial_monthly_salary | Float | Simulated starting monthly salary in USD |

## 2. employment_exits.csv

**Grain:** One row per employment exit.

**Key:** `employee_id` is unique under the project's no-rehire assumption.

| Column | Type | Description |
|---|---|---|
| employee_id | String | Employee identifier |
| exit_date | Date | Effective departure date |
| exit_type | String | Voluntary or Involuntary |
| exit_reason | String | Broad simulated exit reason |

**Business rules:**
- Exit dates must occur after hire dates.
- No employee may have more than one exit in this version.
- Exit dates must fall within the observation window.

## 3. monthly_snapshots.csv

**Grain:** One row per active employee at each month-end.

**Composite primary key:** `employee_id`, `snapshot_date`

| Column | Type | Description |
|---|---|---|
| employee_id | String | Employee identifier |
| hire_date | Date | Original hire date |
| department | String | Department at observation |
| job_level | String | Job level at observation |
| snapshot_date | Date | Month-end observation date |
| tenure_years | Float | Approximate elapsed tenure |
| monthly_salary | Float | Monthly salary in USD |

**Business rules:**
- Each employee-month combination must be unique.
- Snapshot date must be on or after hire date.
- Snapshot date must be earlier than the effective exit date.
- Salary must be positive.
- Tenure must be nonnegative.

## Relationships

- `employees` → `monthly_snapshots`: One-to-many.
- `employees` → `employment_exits`: One-to-zero-or-one.

## Known limitations

- Employment departures use a constant-hazard simulation.
- Department and job level remain static in the current version.
- Salary remains unchanged over time.
- No rehires, leave-of-absence states, or part-time work schedules.
- The simulation does not establish real-world workforce relationships or predictive performance.

## Future target

`voluntary_attrition_next_3m`

Indicates whether a voluntary exit occurs during the 90 days following an eligible snapshot date. Incomplete follow-up periods and competing involuntary departures require explicit handling.
