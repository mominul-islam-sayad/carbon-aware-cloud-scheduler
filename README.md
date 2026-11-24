# Carbon-Aware Cloud Scheduler

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/mominul-islam-sayad/carbon-aware-cloud-scheduler/blob/main/Green_Project.ipynb)

A smart cloud scheduling simulation (`Green_Project.ipynb`) that shifts workloads toward periods with lower expected carbon intensity. It evaluates how organizations can reduce carbon emissions while balancing scheduling constraints and operational requirements using real cloud cluster traces and historical grid carbon data.

## Features

- **Carbon-Aware Workload Scheduling:** Shifts deferrable tasks toward low-carbon grid windows to minimize total estimated emissions.
- **Real Cloud Trace Extraction:** Queries task execution and resource usage records from Google Cloud's public cluster dataset (`google.com:google-cluster-data.clusterdata_2019_a.instance_events`) via BigQuery.
- **Grid Carbon Intensity Integration:** Fetches historical hourly carbon intensity data (`gCO2eq/kWh`) via the Electricity Maps v4 API.
- **Constraint-Aware Optimization:** Evaluates feasible execution windows while respecting task deadlines, priorities, and resource requirements.
- **In-Memory Simulation & Visualization:** Compares baseline vs. carbon-aware scheduling performance directly inside the notebook without external file dependencies.

## How It Works

1. Workload traces and task constraints (arrival times, durations, resource requests, and deadlines) are extracted from Google BigQuery.
2. Historical grid carbon intensity profiles are retrieved from the Electricity Maps API.
3. Candidate execution windows are evaluated against each workload's deadline and priority constraints.
4. The scheduler selects feasible placements that minimize estimated carbon impact.
5. Baseline and optimized carbon emissions are compared and visualized directly within the notebook.

## Prerequisites

- **Google Cloud Platform (GCP) Account:** A GCP project with the **BigQuery API** enabled to query the public Google Cluster Data dataset.
- **Electricity Maps API Key:** An active API key from [Electricity Maps](https://www.electricitymaps.com/) with access to the `/v4/carbon-intensity/history` endpoint.
- **Python Environment:** Google Colab (recommended) or a local Jupyter environment with `google-cloud-bigquery`, `google-cloud-bigquery-storage`, `db-dtypes`, and `requests`.

## Getting Started

### Option 1: Run in Google Colab (Recommended)

1. Click the **Open In Colab** badge above to open `Green_Project.ipynb`.
2. **Add Your Electricity Maps API Key:**
   - Open the **Secrets** panel (key icon in the left sidebar).
   - Create a secret named `ELECTRICITY_MAPS_API_KEY` and paste your API key as the value.
   - Toggle **Notebook access** ON.
3. **Set Your GCP Project ID:**
   - In the setup cells at the top of the notebook, replace the default `GCP_PROJECT_ID` with your own BigQuery-enabled GCP project ID:
     ```python
     GCP_PROJECT_ID = "your-gcp-project-id"
     ```
4. **Authenticate & Execute:**
   - Run all cells sequentially (`Runtime` -> `Run all`) and approve the Google Cloud authentication prompt when requested.

### Option 2: Run Locally

1. **Clone the repository:**
   ```bash
   git clone [https://github.com/mominul-islam-sayad/carbon-aware-cloud-scheduler.git](https://github.com/mominul-islam-sayad/carbon-aware-cloud-scheduler.git)
   cd carbon-aware-cloud-scheduler
   ```
2. **Install dependencies:**
   ```bash
   python -m venv .venv
   source .venv/bin/activate
   pip install google-cloud-bigquery google-cloud-bigquery-storage db-dtypes requests pandas matplotlib jupyter
   ```
3. **Configure credentials:**
   Never commit real API keys or credentials to source control. Export your Electricity Maps API key as an environment variable and authenticate with GCP:
   ```bash
   export ELECTRICITY_MAPS_API_KEY="your-api-key"
   gcloud auth application-default login
   ```
4. **Launch the notebook:**
   ```bash
   jupyter notebook Green_Project.ipynb
   ```
   Update `GCP_PROJECT_ID` in the setup cells to your GCP project ID and run all cells.

## Scheduling Considerations

- Deferrable workloads provide the greatest opportunity for carbon reduction through temporal shifting.
- Non-deferrable workloads must execute immediately based on current grid carbon intensity and resource availability.
- Every scheduling decision must remain feasible within the workload's specified deadline.
- High-priority jobs should be protected from excessive execution delay.

## Repository Structure

```text
carbon-aware-cloud-scheduler/
├── Green_Project.ipynb   # BigQuery trace extraction, Electricity Maps integration, scheduler simulation, and plots
└── README.md             # Project overview and setup instructions
```

## Contributing

Contributions are welcome:

1. Fork the repository.
2. Create a feature branch: `git checkout -b feature/my-feature`
3. Commit your changes: `git commit -m "Add my feature"`
4. Push the branch: `git push origin feature/my-feature`
5. Open a pull request.

Please keep API keys, GCP project credentials, and personal secrets out of committed notebook cells.

## License

No license is currently declared in the repository. Add an appropriate `LICENSE` file before using this project outside an internal or research setting.

```
