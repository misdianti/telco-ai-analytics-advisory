# Case Study 3: Insight Generation from External, Unstructured Sources

| | |
|---|---|
| **Engagement type** | Advisory (scoping, risk assessment, and MVP recommendations) |
| **Client profile** | Indonesian telecommunications company with nationwide regional operations |
| **Team** | Analytics team with one SQL/light-Python analyst and no dedicated engineer |
| **Stack in scope** | Social media and app store data, NLP for informal Bahasa Indonesia |
| **Status** | Early exploratory stage. Recommendations delivered. Not implemented or measured by me. |

## Problem

The goal was to read customer and market trends from external sources: what is generating attention in the market, what opportunities may be worth pursuing, what challenges the company currently faces, and how competitors are positioned. Candidate sources were Instagram, Threads, and X, not yet finalized. The target insight output was still undefined.

## Key design decisions

1. **Verify platform access before committing build time.** API restrictions are a policy constraint, not a skill constraint.
2. **Rule out scraping explicitly.** It generally violates platform Terms of Service, with account suspension risk and legal exposure under the company's name.
3. **Choose one insight type first.** Topic detection, competitor analysis, sentiment analysis, and sentiment-to-business-metric correlation are not parallel options.
4. **Sequence correlation last.** It is a harder, separate analytical problem, and a weak result is a legitimate outcome.
5. **Prove the pipeline on legal, low-risk data first.** App store reviews can validate the mechanics, but they do not answer the market question.

## Recommended approach

### 1. Data source: verify access first

Each candidate platform exposes an API, but access for third-party monitoring has become significantly more restricted in recent years. Some free tiers are unusable for meaningful monitoring volume, public-content access on some platforms is largely limited to formal research programs, and newer APIs may be built mainly for posting rather than pulling other users' content at scale. Current terms and cost tiers should be checked for each platform before any extraction work starts.

If self-built access is infeasible or too limited, a third-party social listening vendor is the fallback. That is a recurring cost decision, not a one-time build cost.

### 2. Structured thinking before execution

Write a short PRD that forces clarity on the objective, then choose among the candidate insights:

| Insight type | Suggested order | Rationale |
|---|---|---|
| Topic detection | Early | Tractable first version with existing analytical skills |
| Sentiment analysis | Early | Same as above |
| Competitor analysis | After pipeline and source are stable | Depends on a reliable source |
| Sentiment vs. churn, acquisition, revenue | Last | Churn and revenue are driven by pricing, network quality, competitor promotions, and macro factors. Social sentiment is one noisy signal among many confounders. |

Expectations should be set from the start that the correlation work may return a weak or inconclusive result. That would not mean the build failed.

### 3. MVP with legal, low-risk data

Start with public app store reviews to build and test extraction, processing, and insight generation without the access and compliance uncertainty of social platforms. This is a technical proof of pipeline only. Reviews reflect app experience and usability, not broader market sentiment or competitor positioning. The social source is the step that answers the real question once access is resolved.

### 4. Language handling

Social media commentary in Indonesia is often informal and full of slang. Whether the analysis must handle this affects model and prompt choice for both sentiment and topic detection.

## Open items identified before build

- Current API terms and cost tiers for each candidate platform.
- If self-built access is infeasible, vendor options and recurring cost against budget.
- The first insight type to deliver, selected through the PRD process.
- Agreement that correlation with business metrics is a later phase with an uncertain result.
- Whether informal Bahasa Indonesia must be supported.

## Reference implementation

See [`reference-implementations/03-review-sentiment`](../reference-implementations/03-review-sentiment) for a pipeline built on public app store reviews.

---

*All client details have been anonymized. The reference implementation uses only publicly available data.*
