# Case Study 1: AI-Driven Insight Automation for a Regional Performance Dashboard

| | |
|---|---|
| **Engagement type** | Advisory (architecture and approach recommendations) |
| **Client profile** | Indonesian telecommunications company with nationwide regional operations |
| **Team** | Commercial strategy and business planning analytics |
| **Stack in scope** | BigQuery, Power BI, Vertex AI (Gemini) |
| **Status** | Recommendations delivered. Not implemented or measured by me. |

## Problem

The analytics team supports multiple business units through a Power BI dashboard covering several hundred regions, fed by a BigQuery datamart. Each user sees only their own region. The requirement was to move the dashboard beyond raw figures to generated insight: a short summary of what changed and what needs attention, per region. The company already had an enterprise generative AI license that was not yet used for this workflow.

## Key design decisions

1. **Keep the existing stack and add one step.** BigQuery and Power BI stay as they are. A narrative-generation step is inserted between them.
2. **Do not connect Power BI directly to the LLM.** Power BI has no native Vertex AI connector, and calling an API on every refresh across hundreds of regions would slow refreshes and tie dashboard reliability to a live external call.
3. **Flag deterministically, narrate with the model.** What counts as "needs attention" is computed in SQL before the model is called. The model only writes the narrative from already-flagged, structured figures.
4. **Prompt engineering over fine-tuning.** Business rules are supplied as structured context, not trained into the model.
5. **Scheduled generation, not on-demand.** Cadence is set by how often the underlying metrics move enough to justify a new narrative.

## Recommended architecture

```
BigQuery datamart
      |
      v
SQL flagging layer (thresholds, week-over-week change, target vs actual)
      |
      v
Vertex AI (narrative generation from structured, flagged input)
      |
      v
BigQuery output table (insight text per region and period)
      |
      v
Power BI (reads metrics and insight table side by side)
```

Routing the AI step through BigQuery limits Power BI to what it already does well: reading a table.

### Refresh schedule

Generate insights on a fixed schedule. A weekly cadence is worth testing first. Daily generation across that many regions risks narrative noise without matching change in the numbers, and weekly keeps API call volume predictable.

### Why business rules are computed before the model runs

- The same flagging criteria apply consistently to every region.
- The output is auditable. If a regional manager asks why their area was flagged, the answer is a SQL rule, not a model judgment.
- Each model call is small and cheaper, because it does not reason over raw, unfiltered data.

### Implementation options

| Option | Description | Trade-off |
|---|---|---|
| BigQuery ML remote model (`ML.GENERATE_TEXT`) | Aggregation and generation both stay inside BigQuery as scheduled SQL | Lower effort for a SQL-first team and the faster path to a first working version |
| Cloud Function or Cloud Run job with the Vertex AI SDK | Reads the flagged view, builds the prompt, calls the model, writes back to BigQuery | More flexible for multi-step prompting, but one more service to build and maintain |

**Recommendation:** start with the BigQuery ML option. Move to the second only if prompting logic outgrows what is comfortable to express in SQL.

## Open items identified before build

- Confirm an active cloud project and service account with the AI API enabled, or whether provisioning with IT is still needed.
- Confirm data governance clearance for routing commercial and network performance data through the AI service, including data residency and whether prompts or outputs are retained or used for training under the enterprise agreement.
- Define flagging thresholds per metric with the business team. This ruleset is what makes output consistent and defensible across all regions.
- Decide refresh cadence based on the team's actual business review cycle.

## Reference implementation

See [`reference-implementations/01-insight-pipeline`](../reference-implementations/01-insight-pipeline) for a synthetic-data version of the flagging SQL and prompt template.

---

*All client details have been anonymized. Figures and data in the reference implementation are synthetic.*
