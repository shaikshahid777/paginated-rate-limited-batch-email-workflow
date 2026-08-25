# Paginated Rate-Limited Batch Email Workflow — Lesson 5 Assessment

Production-style n8n workflow for retrieving records from a paginated JSONPlaceholder API, aggregating the dataset, processing records in controlled batches, tolerating record-level failures, pausing between batch cycles, and generating execution metrics.

## Workflow Architecture

```text
Manual Trigger
      ↓
Config
(pageSize=10, batchSize=5, waitSeconds=2)
      ↓
Init State
      ↓
Prepare Page Request
      ↓
Fetch Page (JSONPlaceholder)
      ↓
Accumulate & Check
      ↓
More Pages?
   ↙         ↘
 Yes         No
  ↓           ↓
Prepare     Combine All Pages
Page Request      ↓
  ↑          Split In Batches
  │               ↓
  └────────  Send Email (Simulated)
                   ↓
              Check Success
                   ↓
             Rate Limit Pause
                   ↺
              Split In Batches
                   ↓
                Summary
```

## Configuration

- `pageSize`: 10
- `batchSize`: 5
- `waitSeconds`: 2
- API: `https://jsonplaceholder.typicode.com/users`
- Pagination parameters: `_page` and `_limit`

## Pagination

The workflow requests pages from JSONPlaceholder and accumulates valid user records. Pagination continues while records are returned and stops when the retrieved page contains no valid records.

## Batch Processing

The aggregated dataset is processed using Split In Batches with a configurable batch size of 5. The workflow supports a smaller final batch when the total record count is not evenly divisible by the batch size.

## Simulated Email Processing

Each record is sent through a simulated HTTP POST request. The request uses the record's email, name, and user ID. Record-level processing is configured to continue when an individual request fails.

The simulated failure path uses an invalid endpoint for selected records so that the workflow can demonstrate failure handling and success/failure classification without stopping the complete run.

## Metrics

The Summary stage reports:

- `totalRecords`
- `totalSucceeded`
- `totalFailed`
- `generatedAt`

A validated run demonstrated 10 records with 8 successful and 2 failed outcomes.

## Rate Limiting

The Rate Limit Pause Wait node uses the reusable `waitSeconds` configuration and pauses for 2 seconds between batch cycles.

## Testing Evidence

The final repository should include screenshots for:

- Pagination page retrieval and loop behavior
- Aggregated dataset
- Batch sequencing
- Successful simulated email processing
- Failed simulated email processing
- Continue-on-fail behavior
- Rate-limit wait behavior
- Final Summary metrics
- Complete end-to-end execution

## Repository Structure

```text
paginated-rate-limited-batch-email-workflow/
├── README.md
├── workflow/
│   └── paginated-rate-limited-batch-email-workflow.json
├── documentation/
│   └── Paginated_Rate_Limited_Batch_Email_Documentation.pdf
├── screenshots/
│   ├── workflow-overview.png
│   ├── pagination.png
│   ├── batch-processing.png
│   ├── failure-handling.png
│   ├── rate-limit.png
│   └── summary.png
└── test-results/
    └── execution-summary.txt
```

## Setup Instructions

1. Import the exported n8n workflow JSON into an n8n workspace.
2. Confirm the JSONPlaceholder URL is reachable.
3. Verify `pageSize`, `batchSize`, and `waitSeconds` in the Config node.
4. Run the workflow with the Manual Trigger.
5. Inspect pagination, batching, failure classification, rate limiting, and Summary output.
6. Capture the final execution evidence and export the latest workflow JSON before submission.

## Submission Deliverables

- Loom/YouTube demonstration video
- Public GitHub repository
- PDF/DOCX documentation package
- Exported n8n workflow JSON
- Execution/test screenshots
- Setup instructions

> Never commit passwords, API keys, SMTP secrets, or other private credentials to this repository.
