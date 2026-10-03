<!-- SHOWCASE_START --><div align="center">[![Typing](https://readme-typing-svg.demolab.com?font=Fira+Code&weight=700&size=22&duration=2800&pause=900&color=58A6FF&center=true&vCenter=true&width=900&lines=paginated%20rate%20limited%20batch%20email%20workflow;AI%20%7C%20Automation%20%7C%20Engineering;Explore%20the%20project%20%F0%9F%9A%80)](https://github.com/shaikshahid777/paginated-rate-limited-batch-email-workflow)<img src="https://capsule-render.vercel.app/api?type=waving&color=0:0D1117,50:161B22,100:58A6FF&height=110&section=header&text=paginated-rate-limited-batch-email-workflow&fontSize=26&fontColor=FFFFFF&animation=twinkling&fontAlignY=65" width="100%" alt="Animated project banner"/>

[![Repository](https://img.shields.io/badge/Repository-181717?style=for-the-badge&logo=github&logoColor=white)](https://github.com/shaikshahid777/paginated-rate-limited-batch-email-workflow) [![Issues](https://img.shields.io/badge/Report-Issue-red?style=for-the-badge&logo=github)](https://github.com/shaikshahid777/paginated-rate-limited-batch-email-workflow/issues/new) [![Stars](https://img.shields.io/github/stars/shaikshahid777/paginated-rate-limited-batch-email-workflow?style=for-the-badge&logo=github)](https://github.com/shaikshahid777/paginated-rate-limited-batch-email-workflow/stargazers) [![Fork](https://img.shields.io/github/forks/shaikshahid777/paginated-rate-limited-batch-email-workflow?style=for-the-badge&logo=github)](https://github.com/shaikshahid777/paginated-rate-limited-batch-email-workflow/fork) [![Profile](https://img.shields.io/badge/Profile-Visit-0A66C2?style=for-the-badge&logo=github)](https://github.com/shaikshahid777)</div>

> ✨ **Project Showcase Mode:** animated banner • interactive navigation • live repository actions

[🚀 Repository](https://github.com/shaikshahid777/paginated-rate-limited-batch-email-workflow) · [🐞 Report Issue](https://github.com/shaikshahid777/paginated-rate-limited-batch-email-workflow/issues/new) · [⭐ Star](https://github.com/shaikshahid777/paginated-rate-limited-batch-email-workflow/stargazers) · [🔱 Fork](https://github.com/shaikshahid777/paginated-rate-limited-batch-email-workflow/fork) · [👤 Profile](https://github.com/shaikshahid777)

<!-- SHOWCASE_END -->

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
