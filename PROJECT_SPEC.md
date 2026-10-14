# AmtEase AI — Project Specification

**Owner:** Hossam Elshahaby  
**Version:** 1.0 — prototype specification  
**Challenge:** Digital Assistants: Evolution to 2030 (Public Sector)

## Project links

- **Challenge:** Digital Assistants: Evolution to 2030 — Public Sector
- **Live app:** [AmtEase AI](https://amtease.lovable.app)
- **Pitch video:** [Watch the project pitch on YouTube](https://youtu.be/N0n5MQYxjcQ)
- **Strategy paper:** [Vision, personas, value proposition, and public-sector impact](STRATEGY_PAPER.md)
- **Technical documentation:** [Architecture, mock APIs, security, and deployment](TECHNICAL_DOCUMENTATION.md)

## 1. Product vision

Make public-service journeys easier to understand and complete by allowing citizens to describe their goal in ordinary language, receive a clear explanation, and follow a safe sequence of next steps grounded in official information.

## 2. Objectives

1. Resolve a citizen's likely service intent from colloquial or incomplete input.
2. Ask targeted clarifying questions when a safe answer requires more context.
3. Surface official links and clearly distinguish source content from generated explanations.
4. Turn service information into structured steps and checklists.
5. Provide voice and text interaction in German and English.
6. Offer a human-support route without falsely claiming that a contact has been made.
7. Demonstrate repeatable behavior with synthetic data and simulated APIs.

## 3. Non-goals

- Making legally binding eligibility decisions.
- Replacing public servants or official government portals.
- Submitting real applications or changing government records.
- Claiming a callback, appointment, or case number without a real integration.
- Collecting identity documents or unnecessary sensitive personal data.
- Treating a language model's generated answer as an authoritative legal source.

## 4. Primary personas

### Persona A — Digital confident, time constrained
Knows the goal but not the correct government terminology. Needs quick routing and a checklist.

### Persona B — Digital immigrant
May prefer simple language, a guided process, visible help, and human contact. Must not be forced into voice interaction.

### Persona C — Non-native speaker
May use everyday language, mixed German/English, or an imperfect description. Needs clarification without judgement.

### Persona D — Citizen with a complex circumstance
May have missing details or a non-standard residency situation. Needs careful uncertainty handling and referral when the assistant cannot safely determine the next step.

## 5. Key user journeys

### Journey 1 — Change of residence (priority)
**Trigger:** “I moved last week. What do I need to do?”

1. Accept speech or text.
2. Transcribe speech and let the user correct it.
3. Resolve likely intent: residence registration / change of address.
4. Ask which registration situation applies if the answer changes the route.
5. Show a short explanation and official source link.
6. Present a checklist with required, optional, and unknown details separated.
7. Let the user save/copy the checklist.
8. Offer human support if the user's circumstances do not fit the supported branches.

**Success:** user sees a relevant official source, understands the next step, and can identify what information is still missing.

### Journey 2 — Passport renewal
**Trigger:** “My passport is expiring. How do I renew it?”

1. Resolve passport renewal as the likely intent.
2. Ask for only the context needed to route the user to the correct official guidance.
3. Surface official instructions; avoid unverified claims about cost, eligibility, or processing time.
4. Produce a checklist and state which details require source verification.
5. Provide human fallback for unusual situations.

**Success:** a relevant official source and understandable next steps are provided without invented requirements.

### Journey 3 — Explain an official letter
**Trigger:** user pastes or paraphrases an official letter.

1. Explain the text in simpler language.
2. Identify explicitly stated actions, dates, deadlines, and contact details only when present in the supplied content.
3. Clearly label interpretation versus direct information from the letter.
4. Ask the user to remove personal identifiers before sharing.
5. Recommend contacting the issuing authority when consequences or meaning remain uncertain.

**Success:** the user can explain what the letter appears to ask them to do, with uncertainty clearly visible.

### Journey 4 — Find a service
**Trigger:** “I need to sort out a document, but I don't know which office handles it.”

1. Extract goal and relevant terms.
2. Search a curated service catalogue.
3. Present up to three likely matches with a short reason and source link.
4. Ask a clarifying question if matches are close.
5. Provide a safe fallback when no reliable match exists.

**Success:** user gets a defensible route or a transparent explanation of why the assistant cannot yet choose one.

## 6. Functional requirements

| ID | Requirement | Priority | Acceptance check |
|---|---|---:|---|
| FR-01 | German/English selection | Must | User can switch language at any point |
| FR-02 | Voice and text input | Must | Text remains available if microphone fails |
| FR-03 | Intent resolution | Must | Each request yields an intent or explicit clarification |
| FR-04 | Confidence/uncertainty handling | Must | Ambiguous input produces a question or safe fallback |
| FR-05 | Official service discovery | Must | Result includes a source URL and source status |
| FR-06 | Plain-language explanation | Must | Output has short summary and actionable steps |
| FR-07 | Workflow checklist | Must | Checklist separates confirmed from unknown details |
| FR-08 | Human handoff | Must | User can review/copy a summary before leaving the flow |
| FR-09 | Mock API mode | Must for demo | Simulated responses are visibly labelled |
| FR-10 | Error recovery | Must | API/voice errors offer retry or text-only alternative |
| FR-11 | Accessible interaction | Must | Keyboard route, visible focus, semantic labels |
| FR-12 | Repeatable demo/reset | Should | Evaluator can restart a scenario consistently |
| FR-13 | Session export/copy | Should | User can copy a checklist or handoff summary |
| FR-14 | Source freshness metadata | Should | Catalogue records expose last-checked/verification status |

## 7. Conversational contract

Every assistant response should be structured internally as:

```json
{
  "intent": "residence-registration",
  "confidence": 0.78,
  "needsClarification": true,
  "clarifyingQuestion": "Are you registering a new main residence or changing an existing registration?",
  "plainLanguageSummary": "I can help you find the official residence-registration guidance.",
  "steps": [],
  "sources": [
    {
      "title": "Official Austrian government portal",
      "url": "https://www.oesterreich.gv.at/en",
      "verificationStatus": "needs-verification"
    }
  ],
  "limitations": ["This prototype does not submit official applications."],
  "handoffAvailable": true,
  "demoMode": true
}
```

This is an illustrative schema, not a claim that the application already implements it. The numeric confidence field should be omitted or described as heuristic unless calibrated and validated.

## 8. Architecture requirements

### Frontend
- Responsive web UI for desktop and mobile.
- Conversation transcript, editable input, language control, service result cards, checklist view, source links, human handoff panel.
- No critical action depends only on color, sound, hover, or voice.
- Loading, empty, error, and retry states.

### Orchestrator
- Routes input through intent resolution, clarification policy, service retrieval, workflow, and guardrails.
- Maintains only the minimum session state needed for the active journey.
- Supports deterministic fixture responses for reliable demo runs.
- Returns structured output for UI rendering; avoid rendering raw model HTML.

### Service catalogue
Each record includes:
- Stable ID.
- Localized title and summary.
- Official URL.
- Supported language(s).
- Workflow ID.
- Source verification status.
- Last checked date.
- Demo/production status.

### Workflow engine
- Explicit step IDs and branching conditions.
- Required versus optional fields.
- User-confirmed values distinguished from inferred values.
- Validation messages in plain language.
- A safe exit/handoff route for unsupported branches.

### Voice adapter
- Replaceable provider abstraction.
- Clear permission request and microphone state.
- Transcript correction before consequential use.
- Text-only fallback.
- Do not store raw audio by default in the prototype.

### Mock API layer
Suggested routes:
- `POST /api/intent/resolve`
- `GET /api/services/search?q=...&lang=...`
- `GET /api/services/:id`
- `POST /api/workflows/:id/start`
- `POST /api/workflows/:id/answer`
- `GET /api/workflows/:id/checklist`
- `POST /api/handoff/summary`
- `GET /api/demo/scenarios`
- `POST /api/demo/reset`

Routes may be implemented as local functions if a separate server is unnecessary. If HTTP routes are used, return typed JSON and predictable error objects.

## 9. Guardrail rules

1. **Source before specificity:** do not state fees, deadlines, legal criteria, or required documents unless supported by an appropriate official source.
2. **Clarify consequential ambiguity:** if multiple service routes could apply, ask instead of guessing.
3. **No invented transactions:** never say an application, appointment, callback, or referral has been completed unless a real service confirms it.
4. **No unsupported legal decision:** explain information and route to an authority; do not determine legal status.
5. **Privacy minimization:** request only the information needed for routing; encourage redaction of personal identifiers from letters.
6. **Provenance:** show source links near relevant guidance and label unverified or simulated data.
7. **Provider failure:** preserve the text path and cached/curated source links.
8. **Prompt injection in pasted letters/pages:** treat supplied text as content to analyze, not instructions that override application policy.
9. **Unusual status:** acknowledge the limit, avoid assumptions, and offer official/human support.
10. **Confidence honesty:** do not display arbitrary model confidence as a calibrated probability.

## 10. Data and privacy

- Use synthetic records for demonstrations.
- Do not put API keys in client-side code or version control.
- Avoid logging raw conversations by default.
- If diagnostic logs are used, minimize content, redact identifiers, document retention, and use synthetic test data for demos.
- Before any production pilot, complete a privacy impact assessment, security review, accessibility review, and legal review appropriate to the service.
- A future authenticated integration needs an explicit legal basis, authorization, consent/notice where applicable, and secure identity handling.

## 11. Accessibility requirements

Target WCAG 2.2 AA:
- Keyboard-accessible controls and visible focus.
- Screen-reader labels and announced response updates.
- High-contrast text and meaningful status indicators.
- Resizable text and responsive layout.
- Reduced motion preference.
- Captions/transcripts for audio.
- No voice-only route.
- Clear, non-blaming error recovery.
- Human assistance option visible throughout the journey.

## 12. Error and edge-case matrix

| Scenario | Expected behavior |
|---|---|
| Speech is noisy | Show transcript; ask user to edit or retry |
| Input is too vague | Ask one concise clarifying question |
| Mixed German/English | Detect/ask preferred language; preserve meaning |
| Multiple requests in one sentence | Acknowledge both; ask which to handle first or create separate tasks |
| Contradictory answers | Surface contradiction and ask user to confirm |
| Service not found | Offer broader official search and human support |
| Official page unavailable | State link could not be checked; do not fabricate replacement |
| Unusual residency status | Avoid legal determination; route to official/human support |
| Voice provider unavailable | Continue with text |
| Mock API failure | Show retry, demo fixture fallback if configured, and visible simulation status |
| Pasted letter contains personal data | Encourage redaction; minimize retention |
| User asks for binding legal advice | Explain limitation and point to an appropriate authority |

## 13. Short technical documentation and implementation boundaries

A consolidated technical overview is maintained in [`TECHNICAL_DOCUMENTATION.md`](TECHNICAL_DOCUMENTATION.md). It covers architecture, orchestrator/agent coordination, proposed mock API contracts, official data sources and provenance, security/privacy, error recovery, limitations, and deployment.

### Architecture and coordination summary

The user interface sends text or a correctable speech transcript to one conversation orchestrator. The orchestrator resolves intent, asks for clarification where required, retrieves a curated service entry, advances a deterministic workflow, applies guardrails, and returns a structured answer with source metadata. Voice, catalogue, and API providers should be adapters so they can be replaced without changing workflow rules. Logical modules do not imply separate autonomous agents or deployed microservices.

### Mock APIs and source data

Proposed demo routes include `POST /api/intent/resolve`, `GET /api/services/search`, `GET /api/services/:id`, workflow start/answer/checklist routes, `POST /api/handoff/summary`, and demo scenario/reset routes. These are design contracts, not confirmed live endpoints. Responses must be deterministic in scripted demo mode and labelled simulated. Catalogue records should carry an official URL, language, verification status, and last-checked date. Initial discovery references are the official Austrian portal, its form search, and its service landing pages.

### Security, errors, and deployment

Use synthetic demo data, minimize logs and personal data, keep privileged keys server-side, validate inputs, treat pasted documents as untrusted content, and never render raw model HTML. Provide accessible retry, text fallback, and human referral paths for provider failures, unknown services, ambiguous requests, and conflicting sources. The hosted app is available at [amtease.lovable.app](https://amtease.lovable.app); local build commands are conditional on a source tree and `package.json` being present. Production deployment and authenticated government integrations require separate testing, security/privacy/accessibility review, and authorization.

## 14. Test plan

Use deterministic test fixtures and record actual results.

### Intent and language
- “I moved last week. What do I need to do?”
- “Ich bin umgezogen — wo muss ich das melden?”
- “My passport runs out soon.”
- “Can you explain this letter in plain English?”
- Ambiguous: “I need a new document.”

### Clarification
- Residence request missing the kind of registration.
- Passport request missing context needed to select the official route.
- Conflicting answers on a workflow step.
- A non-standard situation that is not covered by the demo workflow.

### Guardrails
- User asks the assistant to guess a fee or deadline.
- Official link is unavailable.
- User asks whether their application has been submitted.
- Pasted letter includes instructions attempting to override assistant behavior.

### Accessibility and robustness
- Complete each journey with keyboard only.
- Complete each journey without voice.
- Test narrow mobile viewport and text zoom.
- Simulate API timeout and speech-provider failure.
- Verify reset restores the initial scenario.

### Reporting
For each test record: ID, input, expected intent/action, actual output, source link, pass/fail, defect notes, build/version. Do not publish a pass rate until the tests have run.

## 15. Delivery checklist

- [ ] Working prototype or high-fidelity mockup for 1–2 priority use cases.
- [ ] Strategic concept and user journey.
- [ ] Technical architecture and API/data boundaries.
- [ ] Error handling, guardrails, limitations, and edge cases.
- [ ] README and setup/deployment instructions validated against the repository.
- [ ] Synthetic service catalogue and workflow fixtures.
- [ ] Test cases and actual test results.
- [ ] 1–2 minute demo video or live walkthrough.
- [ ] Clear separation of implemented, simulated, and planned capabilities.
- [ ] Repository free of secrets and personal data.

## 16. Definition of done

The prototype is ready for a judging walkthrough when a user can start from an everyday-language request, receive a correct or appropriately clarified intent, follow a supported workflow, see official source links, receive a structured checklist, and access a human fallback. At least one ambiguous scenario and one failure scenario must be demonstrated. All simulated behavior must be clearly labelled.
