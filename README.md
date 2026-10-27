# Telco AI Analytics Advisory

Three anonymized advisory case studies on applying AI and automation to analytics workflows at a large Indonesian telecommunications company, with synthetic-data reference implementations of the key design ideas.

> **Scope note.** These case studies come from an advisory engagement through [FM Meridian](https://www.linkedin.com/in/febrianamisdianti). The work delivered was architecture, approach, and risk recommendations. I did not implement or measure these solutions in the client's environment, so this repository makes no claims about business outcomes. All client details are anonymized. All data in the reference implementations is synthetic.

## The three problems

| # | Problem | Core design decision | Case study | Reference implementation |
|---|---|---|---|---|
| 1 | A regional performance dashboard shows figures but no insight | Flag deterministically in SQL, and let the LLM only write the narrative | [Read](case-studies/01-ai-insight-automation.md) | `01-insight-pipeline` (planned) |
| 2 | Business cases are reviewed manually against policy that exists only as prose | Codify policy as rules, then backtest against past human reviews before trusting the tool | [Read](case-studies/02-business-review-rule-engine.md) | `02-rule-engine` (planned) |
| 3 | The team wants market insight from social media, but access and target insight are undefined | Verify platform access first, pick one insight type, prove the pipeline on legal data | [Read](case-studies/03-external-insight-generation.md) | `03-review-sentiment` (planned) |

## Design principles across all three

- **Deterministic logic decides, the model explains.** Anything that must be consistent and auditable (what gets flagged, what violates policy) is computed by explicit rules, not left to a language model.
- **Keep the existing stack, add one step.** Recommendations extend what the team already runs rather than replacing it.
- **Validate before trusting.** Automated verdicts are backtested against known human decisions before anyone relies on them.
- **Check constraints before building.** Data access, governance clearance, and platform terms are verified before build time is spent.
- **Sequence by risk.** Start with the narrowest, most testable feature. Treat the hardest analysis (correlating sentiment with business metrics) as a separate, later undertaking whose result may be weak.

## Repository structure

```
.
├── README.md
├── case-studies/
│   ├── 01-ai-insight-automation.md
│   ├── 02-business-review-rule-engine.md
│   └── 03-external-insight-generation.md
├── reference-implementations/
│   ├── 01-insight-pipeline/      # SQL flagging layer and prompt template (synthetic data)
│   ├── 02-rule-engine/           # Python rules, YAML policy, unit tests, backtest
│   └── 03-review-sentiment/      # Public app review pipeline, Bahasa Indonesia
├── docs/
│   └── prd-template.md
└── LICENSE
```

## Tech referenced

BigQuery, BigQuery ML, Vertex AI, Power BI, SQL, Python, Excel and PowerPoint automation, YAML-based rule definitions, NLP for informal Bahasa Indonesia.

## How to read this repository

1. Start with the case study that matches your interest. Each opens with a summary table and the key design decisions.
2. Open the matching reference implementation to see the idea in code.
3. Each reference implementation has its own README with run instructions.

## Status

| Component | Status |
|---|---|
| Case studies | Complete |
| Reference implementation 1 | Planned |
| Reference implementation 2 | Planned |
| Reference implementation 3 | Planned |

*Update this table as each implementation is added. Remove the "planned" label from the table above at the same time.*

## About

I am Febriana Misdianti, a data professional with 10+ years of experience across fintech, e-commerce, and the public sector, working in data strategy, data quality, and fraud and risk analytics. I run FM Meridian, a boutique data consultancy.

- LinkedIn: [linkedin.com/in/febrianamisdianti](https://www.linkedin.com/in/febrianamisdianti)
- Email: febriana.misdianti@gmail.com

## License

Documentation is released under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). Code is released under the MIT License.
