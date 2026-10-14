# AmtEase AI — Short Technical Documentation

**Status:** architecture and implementation guidance for the prototype; not a claim that all components or APIs are deployed.  
**App:** https://amtease.lovable.app  
**Repository:** https://github.com/helshahaby/AmtEase

## 1. Architecture

```text
Responsive web UI
  ├─ Text chat and transcript
  ├─ Optional voice input/output adapter
  ├─ Language and accessibility controls
  └─ Checklist and source-link presentation
          │
          ▼
Conversation orchestrator
  ├─ Input normalization / language handling
  ├─ Intent resolution
  ├─ Clarification policy
  ├─ Service catalogue retrieval
  ├─ Workflow and checklist state
  ├─ Plain-language explanation
  ├─ Safety / provenance checks
  └─ User-reviewed handoff summary
          │
          ▼
Service adapters
  ├─ Curated official-source catalogue
  ├─ Deterministic mock workflow API
  └─ Future approved government integrations (not assumed live)
```

The components are logical modules. They do not require separate autonomous agents or microservices. Keep consequential branching and checklist rules deterministic and testable; use a language model, if configured, to assist with interpretation and explanation within explicit guardrails.

## 2. Agent coordination / orchestration

1. **Normalize:** accept text or a voice transcript; preserve the user's ability to correct transcription.
2. **Resolve intent:** map the request to a supported journey or return an ambiguity state. Do not force a match when confidence is inadequate.
3. **Clarify:** ask one focused question when a missing detail changes the route.
4. **Retrieve:** query the curated service catalogue and carry source metadata alongside any result.
5. **Guide:** load a deterministic workflow, track confirmed answers separately from unknown or inferred values, and generate a checklist.
6. **Guard:** check for unsupported legal claims, missing provenance, sensitive-data overcollection, and claims of completed transactions.
7. **Respond or hand off:** return a structured response; offer a user-reviewable summary for human support when needed.

A shared orchestrator should own the state machine. Do not allow independent agents to silently overwrite one another's conclusions. Record the source and status for claims that could affect a citizen's next action.

## 3. Mock API contract

These are **proposed contracts** for deterministic demo behavior. They may be implemented as local functions instead of HTTP routes. They are not confirmed as deployed endpoints.

| Method and route | Purpose | Example response shape |
|---|---|---|
| `POST /api/intent/resolve` | Resolve intent or request clarification | `{ "intent": "residence-registration", "needsClarification": true }` |
| `GET /api/services/search?q=move&lang=en` | Search curated catalogue | `{ "results": [{ "id": "residence-registration", "sourceUrl": "https://www.oesterreich.gv.at/en", "sourceStatus": "needs-verification" }] }` |
| `GET /api/services/:id` | Return one catalogue record | Localized title, summary, URL, language, verification metadata |
| `POST /api/workflows/:id/start` | Start a guided workflow | Workflow ID, first step, demo flag |
| `POST /api/workflows/:id/answer` | Validate an answer and choose next step | Next step or plain-language validation message |
| `GET /api/workflows/:id/checklist` | Build checklist | Confirmed items, missing items, source links |
| `POST /api/handoff/summary` | Produce a reviewable summary | Summary text and explicit not-sent status |
| `GET /api/demo/scenarios` | List scripted scenarios | Scenario IDs and descriptions |
| `POST /api/demo/reset` | Reset demo state | `{ "ok": true, "demoMode": true }` |

**Common error shape:** `{ "error": { "code": "SERVICE_UNAVAILABLE", "message": "The service could not be loaded.", "retryable": true }, "demoMode": true }`.

Use appropriate HTTP status codes if implemented over HTTP. Validate input length and allowed workflow IDs. Mock responses must visibly identify themselves as simulated in the UI; never resemble a successful government submission.

## 4. Data sources and provenance

Primary discovery starting points:
- https://www.oesterreich.gv.at/en
- https://www.oesterreich.gv.at/en/formsearch
- https://www.oesterreich.gv.at/en/landingpages

Catalogue records should include: stable ID, localized title and summary, official URL, language, workflow ID, verification status, last-checked date, owner/reviewer, and demo/production flag. A source link alone does not prove the linked page's requirements have been checked. Do not invent fees, deadlines, eligibility criteria, or required documents. Where sources are stale, conflicting, or unavailable, say so and link to the official portal or a human contact.

## 5. Security and privacy

- Use synthetic data in demos; avoid collecting identity numbers, account credentials, or unnecessary personal details.
- Do not commit secrets. Any provider key must be kept in secure server-side configuration; never expose privileged keys in browser bundles.
- Do not log raw conversations or audio by default. If diagnostics are needed, minimize content, redact identifiers, document retention, and restrict access.
- Treat pasted letters and retrieved page content as untrusted input, not instructions that override system policy.
- Escape/sanitize rendered content and avoid injecting raw model-generated HTML.
- Validate requests, rate-limit public endpoints, and use secure transport for deployed services.
- Use least-privilege access and separate development/demo credentials from production credentials.
- Before a real public-sector pilot, complete the relevant privacy, security, accessibility, procurement, and legal reviews; define controller/processor responsibilities and retention policy.
- Authentication with ID Austria or any government backend is future work unless a real approved integration is implemented and tested.

## 6. Error handling and recovery

| Failure | Required behavior |
|---|---|
| Speech recognition fails | Keep text input available; allow transcript correction or retry |
| Intent is ambiguous | Ask a focused question; do not guess a consequential route |
| Catalogue result missing | Offer official portal search and human fallback |
| Official page cannot be reached | State that it could not be checked; do not fabricate a substitute |
| Mock API times out | Show a retry action and a clear demo/simulation state |
| Invalid workflow answer | Explain what needs changing without blaming the user |
| Provider unavailable | Degrade to text and curated static links where possible |
| Contradictory source information | Disclose the conflict and refer to the responsible authority |
| Unsupported or complex case | Explain the boundary and offer a user-reviewed handoff summary |
| Reset fails | Preserve the current transcript, show an error, and offer manual restart |

Avoid silent failure. Errors should be understandable, actionable, and accessible to keyboard and screen-reader users.

## 7. Limitations

- The prototype is not a government authority and does not make binding legal decisions.
- Real application submission, identity verification, case tracking, appointment booking, and callbacks are not implied.
- Mock API routes and example schemas in this document are design contracts, not proof of live endpoints.
- Coverage is limited by the curated catalogue and reviewed workflows.
- Voice performance depends on browser/device permissions, network, language, and provider availability.
- Generated summaries can be wrong; official-source verification and human review remain necessary for consequential cases.
- The current repository listing contains documentation, a deck, a challenge brief, and a demo video; a reproducible local source-code build cannot be confirmed from those listed files alone.

## 8. Deployment guidance

### Current demo
Use the hosted app at https://amtease.lovable.app. This is the simplest way to review the user experience; it does not prove that all future integrations are active.

### Local development
The repository should be checked for a `package.json`, lockfile, and source directory before running a local build. If source code is added and the project is Vite/Node-based, the typical sequence is:

```bash
git clone https://github.com/helshahaby/AmtEase.git
cd AmtEase
# Only if package.json exists in this directory:
npm install
npm run dev
```

Use only scripts actually declared in `package.json`. Run `npm run` to inspect available scripts; do not assume `build` or `test` exists. If `package.json` is in a subdirectory, run the commands there.

### Production readiness checklist
1. Pin dependencies and enable automated dependency/security checks.
2. Configure environment secrets in the hosting platform, not in source control.
3. Build and test in CI; run accessibility checks and scripted end-to-end journeys.
4. Use HTTPS, restrictive content-security policy where appropriate, and secure headers.
5. Configure monitoring without storing unnecessary personal data.
6. Assign owners for official-source freshness, incident handling, release approval, and rollback.
7. Complete public-sector privacy, security, accessibility, and legal review before live personal-data processing or backend integration.

Cloudflare Pages/Workers or AWS are possible future hosting choices, not verified deployment facts for the current app.
