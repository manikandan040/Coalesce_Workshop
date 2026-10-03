# Workshop - Coalesce Pipeline

How to rerun the pipeline after a failure or when data needs refreshing.

## Before you start

- Access to the Coalesce environment: `TRAINING`
- Snowflake role/warehouse used by the pipeline: `[ROLE]` / `[WAREHOUSE]`
- Check the failed run's error first (Coalesce UI > Runs/Scheduler > select the run)

## Option 1: Coalesce UI (most common)

1. Open Coalesce and go to **Deploy > TRAINING**.
2. Confirm the latest deployment is current (redeploy only if node/SQL changes were merged).
3. Go to **Jobs** (or **Scheduler**) and select `[JOB_STAGING]` or `[JOB_DIMENSION]` or `[JOB_FACT]`.
4. Click **Run / Refresh**. To resume a failed run, open it and choose **Rerun** to restart from the failed nodes only.
5. Monitor progress in the run view until all nodes show success.

## Option 2: Coalesce CLI (`coa`)

```bash
# Set credentials (or use a .env / config file)
export COALESCE_TOKEN=[TOKEN]

# Refresh the environment (optionally limit with a job or node selector)
coa refresh --environmentID [ENV_ID] --jobID [JOB_ID]
```

## Option 3: API / orchestrator

- Start a new run: `POST /scheduler/startRun` with `environmentID` and `jobID`.
- Rerun a failed run: `POST /scheduler/rerun` with the failed `runID`.
- If triggered via Airflow/dbt Cloud/other: rerun the task `[TASK_NAME]` in `[DAG_NAME]`.

## After the rerun

- Verify row counts / freshness on key tables: `[TABLE_1]`, `[TABLE_2]`
- Confirm downstream dashboards/consumers have updated data.

## Troubleshooting

| Problem | Fix |
|---|---|
| Run fails on the same node | Check node SQL and the Snowflake error; fix, redeploy, then rerun |
| Credentials/auth error | Regenerate the Coalesce token or check Snowflake role grants |
| Warehouse timeout | Resize `[WAREHOUSE]` or rerun off-peak |

## Contacts

Owner: `MANIKANDAN S`  |  Channel: `SYSTECH SOLUTIONS`
