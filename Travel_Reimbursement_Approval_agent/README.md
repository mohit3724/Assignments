# README

## Overview

This project implements a Travel Reimbursement Approval Agent using a single CrewAI agent and Google gemini-3.1-flash-lite. The agent evaluates travel reimbursement claims by retrieving policy context, validating receipts, checking reimbursement limits, and producing structured reimbursement decisions.

The system is designed to be lightweight, reliable, and easy to demonstrate. It prioritizes policy compliance and routes ambiguous or policy-exception cases to Manual Review instead of forcing incorrect decisions.

The agent returns one of the following decisions:

- APPROVE
- PARTIAL_APPROVE
- REJECT
- MANUAL_REVIEW


---

## Setup Steps

### Step 1: Clone the Repository

```bash
git clone <github_repository_url>

cd Travel-Reimbursement-Agent
```


### Step 2: Create a Virtual Environment (Optional)

```bash
python -m venv venv
```

Activate the virtual environment:

Windows:

```bash
venv\Scripts\activate
```

Mac/Linux:

```bash
source venv/bin/activate
```


### Step 3: Install Dependencies

```bash
pip install -r requirements.txt
```

or

```bash
pip install crewai
pip install crewai-tools
pip install google-generativeai
pip install langchain-google-genai
pip install pandas
pip install matplotlib
pip install pydantic
pip install jupyter
```


---

## Required Environment Variables

Only one environment variable is required if Gemini reasoning is enabled.

```bash
GEMINI_API_KEY=<YOUR_GEMINI_API_KEY>
```

Example:

Windows:

```bash
set GEMINI_API_KEY=YOUR_API_KEY
```

Linux / Mac:

```bash
export GEMINI_API_KEY=YOUR_API_KEY
```


> Note:
>
> The notebook includes a deterministic fallback mechanism. Therefore, it can still execute successfully without an API key. Gemini is used only to enhance the agent's reasoning capabilities.


---

## How to Run the Demo

Open Jupyter Notebook:

```bash
jupyter notebook
```

or

```bash
jupyter lab
```

Open:

```text
MohitSaiNalam_Travel_Reimbursement_Agent.ipynb
```

Run all notebook cells sequentially.

The notebook will automatically:

1. Load the travel reimbursement policy.
2. Load the provided sample claims.
3. Initialize the Travel Reimbursement Approval Agent.
4. Perform policy validation.
5. Validate receipts.
6. Check reimbursement limits.
7. Perform timeliness checks.
8. Route policy exceptions to Manual Review.
9. Generate structured JSON outputs.
10. Display the results dashboard.


---

## Agent Workflow

```text
                    User Claim
                         |
                         |
            Travel Reimbursement Agent
                         |
                    CrewAI Agent
                         |
                  gemini-3.1-flash-lite
                         |
                  decides which tools
                     should be used
                         |
      --------------------------------------------
      |                 |                 |
 Policy Lookup      Receipt Check      Limit Check
      |                 |                 |
      --------------------------------------------
                         |
                  Timeliness Check
                         |
                  Output Validation
                         |
                   Policy Grounding
                         |
                  Gemini Reasoning
                         |
                     Final Decision
                         |
             --------------------------------
             |              |               |
          APPROVE      PARTIAL          REJECT
                       APPROVE
                             |
                       MANUAL REVIEW
                         |
                     JSON Output
                         |
                       Dashboard

```


---

## Tools Used

The Travel Reimbursement Approval Agent uses the following tools:

| Tool | Purpose |
|------|--------|
| Policy Lookup Tool | Retrieves relevant policy rules and policy identifiers |
| Receipt Validation Tool | Checks receipt requirements and missing documents |
| Limit Checker Tool | Applies reimbursement limits and per-diem policies |
| Timeliness Checker | Validates claim submission timelines |
| Output Validator Tool | Ensures the final JSON output satisfies all assignment requirements |


---

## Key Design Choices

### Single Agent Architecture

The assignment requests a Travel Reimbursement Approval Agent. Therefore, a single CrewAI agent architecture was selected instead of multiple collaborating agents.

Benefits include:

- simpler implementation
- lightweight architecture
- easier debugging
- easier demonstration during interviews
- aligns closely with the assignment requirements


### Policy Grounding

Every reimbursement decision is grounded in the provided policy identifiers.

Examples include:

```text
POL-CAT-01

POL-PD-01

POL-RCT-02

POL-AIR-01

POL-APR-03
```

This improves transparency and auditability.


### Manual Review Preference

The solution intentionally prefers:

```text
MANUAL_REVIEW
```

instead of forcing reimbursement decisions when:

- receipts are missing
- policy exceptions exist
- approval thresholds are exceeded
- conflicting information is detected
- claims are submitted late

This approach improves reliability and aligns with the assignment requirements.


### Reliability

To improve robustness:

- output validation is performed before returning results
- policy limits are always enforced
- structured JSON outputs are validated
- reimbursement decisions are deterministic and reproducible
- Gemini reasoning is used only to enhance decision making and not to override policy constraints


### Dashboard

The notebook generates a minimal dashboard displaying:

- decision breakdown
- approved amounts
- deducted amounts
- sample outputs
- structured JSON results


---

## Assumptions

The following assumptions were made:

- The provided travel reimbursement policy is the only source of truth.
- All claims are represented using the provided assignment format.
- Receipt availability is represented using a boolean flag.
- Currency is assumed to be USD.
- The sample claims provided in the assignment are used for evaluation.


---

## Known Limitations

The current prototype does not include:

- OCR-based receipt extraction
- duplicate claim detection
- external policy databases
- real-time approval workflows
- multi-level approval notifications


---

## Future Improvements

Potential improvements include:

- OCR-based receipt validation
- MCP tool integrations
- duplicate claim detection
- conversational claim submission
- approval notification workflows
- integration with enterprise reimbursement systems


---

## Final Outputs

The notebook produces:

- Structured JSON outputs for all five claims.
- Policy grounded reimbursement decisions.
- Dashboard visualizations.
- Sample outputs for demonstration purposes.
- Manual Review recommendations for ambiguous or policy-exception cases.
