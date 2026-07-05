# Proctorio (proctorio)

Proctorio is a remote proctoring and learning-integrity platform that secures online exams with automated recording (video, audio, screen, web traffic), identity verification, lockdown-browser behavior controls, and post-exam suspicion scoring with behavioral flags. It is delivered primarily as an LMS-embedded integration - **LTI 1.1 and LTI 1.3** for Canvas, Blackboard, Brightspace (D2L), Moodle, and ILIAS - plus a browser extension, so most institutions never touch a REST API directly. For assessment platforms that are not LMS-native, Proctorio also exposes a partner/integration **REST API (v2)** that generates signed exam launch URLs and delivers results via HMAC-signed **webhooks**.

**APIs.json:** [https://raw.githubusercontent.com/api-evangelist/proctorio/refs/heads/main/apis.yml](https://raw.githubusercontent.com/api-evangelist/proctorio/refs/heads/main/apis.yml)

## Access Model (Important)

Proctorio's API is **partner/institution-gated but publicly documented**:

- **LTI is the primary integration model.** Institutions integrate Proctorio through LTI 1.1 / 1.3 inside their LMS and the browser extension. Exams are configured in the LMS; students launch with their existing LMS credentials. Most institutions never call a REST API.
- **The v2 REST API is the alternative path** for third-party assessment platforms (e.g. Ans, Cirrus Assessment, Questionmark, Top Hat, McGraw Hill Connect, EvaExam, Derivita). It requires a **Proctorio-provisioned consumer key, secret key, and region-specific endpoint host** - you obtain these from a Proctorio representative; there is no public self-serve signup.
- **The endpoint paths, request bodies, and webhook payloads are documented publicly** in Proctorio's official Apache-2.0 .NET client at [github.com/proctorio/API](https://github.com/proctorio/API), and in third-party integration libraries (TAO/oat-sa `lib-proctorio`, GetCertified). This entry is modeled from those public sources - endpoints are real, but the base host is a placeholder and response schemas are modeled.

## Tags

- Online Proctoring
- Remote Proctoring
- Exam Integrity
- Assessment
- EdTech
- LTI
- LMS Integration
- Learning Integrity

## Timestamps

- **Created:** 2026-07-05
- **Modified:** 2026-07-05

## APIs

### Proctorio Launch API (v2)

Generates a signed exam launch URL. Given the exam's launch/start/take/end URLs plus an exam-settings object (which recording, verification, and lockdown behaviors to enforce), each endpoint returns a single signed URL that opens the proctored session. Three endpoints exist - **candidate** (test taker), **reviewer** (Review Center), and **live** (live/remote proctor). Requests are HMAC-signed with the Proctorio-provisioned consumer key and secret.

- `POST /v2/candidate/launch`
- `POST /v2/reviewer/launch`
- `POST /v2/live/launch`

- **Documentation:** [https://proctorio.com/about/integration](https://proctorio.com/about/integration)
- **Source Code (official .NET client):** [https://github.com/proctorio/API](https://github.com/proctorio/API)
- **Base URL:** `https://{region}{endpoint}.com` (provisioned per partner)
- [OpenAPI](openapi/proctorio-openapi.yml)
- [Postman Collection](collections/proctorio.postman_collection.json)
- [Open Collection](collections/proctorio.opencollection.json)

### Proctorio Result Webhooks (v2/v3)

After an attempt is submitted, Proctorio POSTs an HMAC-signed JSON webhook to the integrating platform carrying the attempt id, user id, an overall **suspicion score**, submission metadata, and weighted **behavioral flags** (unfocus, clipboard, browser resize, multiple faces, leaving the exam area, speaking, AI-assistance, printing, screenshot, hardware change, external action, webcam obscured, mobile phone, and more). This is server-to-endpoint HTTP delivery over HTTPS - **not a WebSocket** - and the payload can be re-emitted from the Review Center Export Options tab when behavior settings change. Payload shape (v2 and v3) is documented in the official .NET client.

## WebSocket Review

**Does Proctorio expose a documented public WebSocket API?** **No.** Proctorio's documented developer surface is request/response REST (the v2 launch API) plus HTTP result webhooks, alongside the LTI 1.1/1.3 LMS integration model. There is no documented `ws://`/`wss://` endpoint for integrators, so no AsyncAPI document was authored. See [review.yml](review.yml).

## Common Properties

- [GitHub Organization](https://github.com/proctorio)
- [LinkedIn](https://www.linkedin.com/company/proctorio-incorporated)
- [Website](https://proctorio.com)
- [Documentation](https://proctorio.com/about/integration)
- [Source Code](https://github.com/proctorio/API)
- [Plans](plans/proctorio-plans-pricing.yml)
- [Rate Limits](rate-limits/proctorio-rate-limits.yml)
- [Fin Ops](finops/proctorio-finops.yml)
- [Changelog](https://changes.proctorio.com/)

## Pricing

Proctorio does not publish a public self-serve price list. Pricing is quoted per institution/partner and is consumption-based on **proctored exam attempts**, invoiced to the institution or paid directly by students (student-pay costs the institution nothing). API/integration access is included with the agreement rather than sold as a separate metered API. Third-party sources report illustrative figures (e.g. ~$5/test, ~$60/student bundle, ~$17.50/student/term) - none are official published rates. See [plans/proctorio-plans-pricing.yml](plans/proctorio-plans-pricing.yml).

## Maintainers

**FN:** Kin Lane
**Email:** kin@apievangelist.com
