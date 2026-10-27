# Case Study 2: Business Case Review Automation with a Rule Engine

| | |
|---|---|
| **Engagement type** | Advisory (approach, build sequence, and validation recommendations) |
| **Client profile** | Indonesian telecommunications company with nationwide regional operations |
| **Team** | Business planning, with one SQL/Python-capable analyst and no dedicated engineer |
| **Stack in scope** | Excel, PowerPoint, Python, GitHub, cloud deployment |
| **Status** | Recommendations delivered. Not implemented or measured by me. |

## Problem

Every business initiative requires a Business Case (BC) explaining its projected profit and loss. BCs are produced in Excel and PowerPoint, in formats that vary by author and often disagree between the two versions of the same BC. The business planning team reviews each BC manually against policy thresholds such as yield and margin. Review is sporadic, there is no standard way to compare one BC with another, and there is no consolidated view across submissions. Policy exists only as narrative documents, not as codified rules.

## Key design decisions

1. **Standardize input before automating review.** A single Excel template with automated export to PowerPoint removes the reformatting step that creates inconsistency.
2. **Codify policy as explicit rules.** Convert narrative policy into IF-THEN-ELSE logic. AI may draft rules, but every AI-drafted rule is reviewed against the source policy by the business planning team before it is trusted.
3. **Build the narrowest feature first.** The Excel-to-PowerPoint converter is bounded and testable. The rule engine and BC comparison come one at a time afterward.
4. **Write a PRD before any AI-assisted development.** A clear PRD (problem, interface mockups, technical framework, feature list) keeps AI-assisted output maintainable.
5. **Backtest before production use.** Validate the engine against BCs already reviewed manually.
6. **Separate compliance from ranking.** Compliance is binary logic. Ranking by potential is a scoring methodology that the business must define.

## Recommended approach

### 1. Standardize BC input format

Introduce one standard Excel template for all BCs, with an automated export to PowerPoint. This is as much an adoption question as a technical one. Someone on the business planning side needs the authority to mandate the template across business units, and there will be a transition period in which BCs in the old formats still need evaluation. That backlog should be planned separately, not assumed to be resolved by the new template.

### 2. Codify policy as rules

Where the number of rules or their interdependencies make manual conversion slow (for example, yield in one channel required to exceed yield in another), AI can help draft rule logic from policy text. A misread policy clause becomes a silent, systemic error once the engine runs against every BC, which is why human review against the source document is required.

### 3. Build method: AI-assisted development

With no dedicated engineer, AI-assisted development is a reasonable way to build without hiring. It should be preceded by basic familiarity with the workflow and by a written PRD.

### 4. Build sequence

```
Excel-to-PowerPoint converter  ->  rule engine  ->  BC comparison  ->  ranking
```

### 5. Versioning and deployment

Use GitHub from the start. With only one person able to read the code, version history and a well-documented PRD are what allow someone else to take over. Before writing code, confirm with an engineer whether local build with cloud deployment is feasible and what environment and access it requires.

## Added recommendations

### Validate before production use

Backtest the engine against previously reviewed BCs and compare the tool's conclusion to the known human conclusion for each. Only when the two consistently agree should its output be treated as authoritative. Skipping this risks approving a non-compliant initiative or wrongly blocking a good one, and either early failure costs the tool the team's trust.

### Define ranking methodology separately

Ranking requires weighing margin, yield, and other factors against each other. That is a business decision to be defined before it is built, and it does not follow automatically from the compliance rules.

## Open items identified before build

- Who has authority to mandate the standard template across business units, and how the backlog of old-format BCs will be handled.
- Which policy documents are current and authoritative, since overlapping or conflicting documents produce inconsistent rules regardless of conversion quality.
- Who owns keeping the rule engine in sync when policy is updated.
- Whether local development with cloud deployment is feasible, and the required environment.
- The BC ranking and scoring methodology.

## Reference implementation

See [`reference-implementations/02-rule-engine`](../reference-implementations/02-rule-engine) for a synthetic-data version with YAML rules, unit tests, and a backtest script.

---

*All client details have been anonymized. Rules, thresholds, and data in the reference implementation are synthetic and do not reflect any real policy.*
