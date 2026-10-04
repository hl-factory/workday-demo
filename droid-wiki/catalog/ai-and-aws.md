# AI and AWS apps

Active contributors: tony-gilfillan, ekwuno

## Purpose

Five catalog Extend apps show how to call AI services from Workday: three use the Workday AI Gateway and two use the Extend integration with Amazon Web Services. The matching agent skill is `examples/workday-ai-gateway-skill/SKILL.md`, which lists each of these folders as the reference for its scenario (see [Workday agent skills](../examples/workday-agent-skills.md)).

## The apps

| Entry | Service | What it does | Files |
| --- | --- | --- | --- |
| `catalog/documentIntelligenceWithTheAIGateway/` | AI Gateway Document Intelligence | Scan resumes, receipts, and other documents and capture text; separate view pages per document kind | 15 |
| `catalog/generateWQL/` | AI Gateway | Build WQL statements through a UI or from natural language | 10 |
| `catalog/charitableDonationsWithSentimentAnalysis/` | AI Gateway sentiment analysis | The Charitable Donations app with a `catalog/charitableDonationsWithSentimentAnalysis/presentation/cards/Rating.card` that scores charity descriptions | 40 |
| `catalog/AWSStarterKit/` | AWS Lambda connectors: Translate, Textract, Comprehend | Starter patterns: bulk translation, text detection, PII detection, sentiment, and a generic "call any AWS API" flow | 23 |
| `catalog/AWSBadgeGenerator/` | AWS with EventBridge routing | Extends onboarding so new hires upload a badge photo, get validation feedback, and the HR approval decision is automated | 25 |

## AWS prerequisites

Both AWS apps rely on the Extend-AWS native integration. The `catalog/AWSStarterKit/README.md` says this requires an Innovation Service Agreement opt-in and AWS account access granted by a company administrator, uses the AWS SAM CLI to deploy the Lambda code, and expects the `us-west-2` (US West Oregon) region. The Lambda code and `template.yaml` are not in this repo; the README links to a developer forum post for them.

`catalog/AWSStarterKit/` orchestrations: `callAWSTranslate`, `bulkTranslations`, `detectDocumentText`, `detectPII`, and `callAnyAWS`. It also ships French and English labels under `catalog/AWSStarterKit/presentationLabels/`.

`catalog/AWSBadgeGenerator/` orchestrations: `validatePhoto`, `generateBadge`, `completeOnboardingBadgeStep`, and `inboundBadgeComplete`. `catalog/AWSBadgeGenerator/presentation/testing/` holds one mock config (`validatePhoto.mock_config`) and three mock responses, so the pages can be tried without calling AWS.

## AI Gateway pattern

`catalog/documentIntelligenceWithTheAIGateway/orchestration/ScanDocumentOrchestration.orchestration` makes the Gateway call server-side and the pages render the result. `catalog/generateWQL/` has no orchestration; its single page `catalog/generateWQL/presentation/queryBuild.pmd` and its pods build and run queries. The AI Gateway skill's rules ("call the Gateway from an orchestration or an Extend endpoint inside the Workday boundary", "treat model output as untrusted") describe these apps.

One small inconsistency: the folder is `documentIntelligenceWithTheAIGateway` while the AMD's `applicationId` is `documentIntelligenceWithTheAiGateway` (lowercase "i").

## Key source files

| File | Purpose |
| --- | --- |
| `catalog/AWSStarterKit/README.md` | AWS setup steps (SAM, region, forum links) |
| `catalog/AWSStarterKit/orchestration/callAnyAWS.orchestration` | Generic AWS call pattern |
| `catalog/AWSBadgeGenerator/orchestration/validatePhoto.orchestration` | Photo validation step |
| `catalog/documentIntelligenceWithTheAIGateway/orchestration/ScanDocumentOrchestration.orchestration` | AI Gateway call |
| `catalog/generateWQL/presentation/queryBuild.pmd` | Natural language to WQL page |

## Related pages

- [HR and workforce apps](hr-and-workforce.md) for the base Charitable Donations app
- [Security](../security.md)
