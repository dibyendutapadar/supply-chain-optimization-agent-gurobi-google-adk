# Supply Chain Optimization Agent

This project implements a multi-agent system for solving supply chain optimization problems using the Gurobi Python API. The system leverages AI agents to interpret user queries, generate optimization models, execute them, and present results in a user-friendly format.

---

## Features

- **Interpreter Agent**: Converts user queries into structured optimization model specifications.
- **Optimizer Agent**: Generates Python code for the Gurobi solver, executes it, and captures results.
- **Result Formatter Agent**: Formats the optimization results into a clear and concise report.
- **Sequential Agent Pipeline**: Orchestrates the workflow of the agents to deliver end-to-end functionality.

---

## Code Structure

- **`agent.py`**: Main implementation of the multi-agent system.
- **`test.ipynb`**: Jupyter notebook for testing and debugging optimization models.
- **`solution.sol`**: Stores the solution of the optimization problem.
- **`data/`**: Contains input CSV files for demand, production, and transportation costs.
- **`requirements.txt`**: Lists all dependencies required for the project.

---

## How It Works

1. **User Query**: The user provides a supply chain optimization problem description.
2. **Interpreter Agent**: Parses the query and generates a structured model specification.
3. **Optimizer Agent**: 
   - Converts the model specification into Python code using the Gurobi API.
   - Executes the code and captures the results.
4. **Result Formatter Agent**: Formats the results into a user-friendly report.
5. **Output**: The system provides the optimal solution, decisions, and constraint metrics.

---

## Installation

1. Clone the repository:
   ```bash
   git clone https://github.com/your-repo/supply-chain-optimization.git
   cd supply-chain-optimization
2. Create a virtual environment
    ```bash
    python -m venv .venv
    source .venv/bin/activate
    ```
3. Install dependencies
    ```bash
    pip install -r requirements.txt
    ```
4. Add API keys to `.env`
5. Run by
    ```bash
    adk web
    ```
---

## Example Problem
The system can solve problems like:

- Minimizing transportation costs between production facilities and distribution centers, Ensuring production and demand constraints are met.
- Optimizing multi-tier supply chains with factories, wholesalers, and retailers.

---
### Dependencies
Python 3.8+
Gurobi 13.0.0
Rich
dotenv
Google ADK

---

## Acknowledgments
[Gurobi Optimization](https://docs.gurobi.com/current/)
[Google ADK](https://google.github.io/adk-docs/)
