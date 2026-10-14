# AmtEase AI

**A voice-first, accessible digital public-service assistant concept**  
**Project lead:** Hossam Elshahaby  
**Challenge:** Digital Assistants: Evolution to 2030 — Public Sector

## Project links

- **Challenge:** Digital Assistants: Evolution to 2030 — Public Sector
- **Live app:** [AmtEase AI](https://amtease.lovable.app)
- **Pitch video:** [Watch the project pitch on YouTube](https://youtu.be/N0n5MQYxjcQ)
- **Strategy paper:** [Vision, personas, value proposition, and public-sector impact](STRATEGY_PAPER.md)
- **Technical documentation:** [Architecture, mock APIs, security, and deployment](TECHNICAL_DOCUMENTATION.md)

## 1. Overview

AmtEase AI helps people navigate public services using everyday language rather than requiring them to know official administrative terminology. It combines conversational intent understanding, plain-language explanations, official service discovery, guided checklists, uncertainty handling, and a human-support fallback.

The prototype is designed around a simple principle:

> Help people understand what to do next — without pretending to be a government authority or making legal decisions on their behalf.

The initial experience is German- and English-capable, with voice and text as complementary ways to interact. A non-voice path remains available for accessibility and reliability.

## 2. Problem

Citizens may know their real-world goal — for example, reporting a move or understanding a letter — but not the correct service name, official form, prerequisites, or sequence of steps. Information can be spread across pages and written in administrative language. This creates avoidable uncertainty, repeated searching, incomplete submissions, and poorly prepared enquiries to public-service staff.

Digital access alone does not guarantee an understandable journey. A useful assistant must resolve intent, identify missing information, point to authoritative sources, and communicate uncertainty clearly.

## 3. Solution

AmtEase AI acts as a guided entry point to public-service information:

1. **Understand:** interpret colloquial, incomplete, or mixed-language input.
2. **Clarify:** ask a focused question when key details are missing or ambiguous.
3. **Discover:** search a curated service catalogue and link to official sources.
4. **Explain:** convert complex language into plain-language summaries.
5. **Guide:** present a sequence of steps, prerequisites, and a personalized checklist.
6. **Verify:** label simulated content and distinguish verified source links from generated explanations.
7. **Hand off:** prepare a user-reviewable summary for a human support channel when the assistant cannot safely resolve the request.

## 4. Initial use cases

### A. Residence registration / change of address
A citizen says they have moved and asks what to do. The assistant clarifies the nature of the registration, surfaces official information, identifies the relevant checklist, and avoids making assumptions about the person's legal status or circumstances.

### B. Passport renewal
A citizen asks how to renew a passport. The assistant determines which information is missing, points to official guidance, and explains the next steps in accessible language. Requirements, fees, and timing must be checked against authoritative sources rather than invented.

### C. Explain an official letter
A citizen pastes or paraphrases a letter. The assistant explains the language in simpler terms, separates what the letter explicitly says from its own interpretation, identifies deadlines only when visible in the supplied text, and suggests contacting the issuing authority when meaning is uncertain.

### D. Service discovery
A user describes a goal without knowing the official service name. The assistant maps the request to a likely service, states confidence, and asks a clarifying question when several services could apply.

## 5. Product experience

- Voice-first interaction with an equivalent text-based route.
- German and English interface and conversation support.
- Clear transcript and editable text input.
- Short responses, plain-language summaries, and numbered steps.
- Official-source links placed near the claims they support.
- Checklists that distinguish confirmed details from details still needed.
- Visible demo/simulation labels wherever mock data or mock API responses are used.
- Human-support fallback with a reviewable summary; no false claim that a call or appointment has been booked.
- Reset/demo scenario controls for repeatable judging.

## 6. Architecture

The prototype can be implemented as a modular web application. These are logical components; they do not require separate autonomous agents or microservices.

```text
Web / Mobile-responsive UI
  ├── Voice input + speech output adapter
  ├── Text chat + transcript
  ├── Accessibility and localization layer
  └── Session state
          |
          v
Conversation Orchestrator
  ├── Intent resolution
  ├── Clarification policy
  ├── Service catalogue search
  ├── Plain-language explanation
  ├── Workflow / checklist engine
  ├── Confidence and guardrail checks
  └── Human handoff summary
          |
          v
Service Adapter Layer
  ├── Official public-service links
  ├── Curated service catalogue
  ├── Simulated workflow API
  └── Optional future government/API connectors
```

### Recommended implementation stack

- **Frontend:** React + TypeScript + Vite.
- **UI:** Tailwind CSS and a reusable accessible component system.
- **Voice:** ElevenLabs Agents or another replaceable speech/agent adapter.
- **Service catalogue:** versioned JSON data with source URLs and last-verified metadata.
- **Prototype APIs:** mock service/workflow endpoints with deterministic fixtures.
- **Deployment:** Cloudflare Pages/Workers or AWS, subject to the chosen implementation.
- **Testing:** unit tests for intent and workflow rules; scripted end-to-end scenarios; accessibility checks.

The stack is a recommendation, not a claim that every integration is already implemented. See `PROJECT_SPEC.md` for boundaries between current prototype scope and future integrations.

## 7. Official information and source integrity

Use the official Austrian government portal as the initial service-discovery reference:

- [Austrian government — English home](https://www.oesterreich.gv.at/en)
- [Official form search](https://www.oesterreich.gv.at/en/formsearch)
- [Official topic landing pages](https://www.oesterreich.gv.at/en/landingpages)

A link in the catalogue is not proof that all details on the page have been verified. Store source URL, title, language, last-checked date, and verification status. Never fabricate official links, legal requirements, fees, eligibility, or deadlines. If a source is unavailable or ambiguous, say so and direct the user to the official portal or a human contact.

## 8. Uncertainty, safety, and guardrails

- Ask a clarifying question when intent is ambiguous or a necessary detail is missing.
- Do not infer citizenship, residence status, age, family status, or other sensitive facts from weak signals.
- Do not provide binding legal determinations or claim that an application has been submitted.
- Do not request identity documents or unnecessary personal data in the demo.
- Keep user-provided letter text separate from the assistant's interpretation.
- Label generated summaries as explanations, not official decisions.
- When sources conflict, disclose the conflict and prefer the relevant official source rather than silently reconciling it.
- If a service is unknown, provide a safe search path and human fallback.
- If speech recognition fails, show the transcript for correction and keep text input available.
- If an API or voice provider fails, degrade gracefully to text and curated catalogue results.
- Do not claim to book a callback, appointment, or contact unless a real integration confirms it.

If numeric confidence scores are used, document how they are produced. If they are only heuristic, label them as such and do not present them as calibrated probabilities.

## 9. Accessibility and inclusion

Target WCAG 2.2 AA practices, including:

- Keyboard navigation and visible focus.
- Semantic headings and labels.
- Screen-reader-friendly status updates.
- Sufficient contrast and scalable text.
- No voice-only interactions.
- Short, understandable sentences and progressive disclosure.
- Clear error messages with recovery instructions.
- Reduced-motion support where animation is used.
- Human support path for users who cannot complete a digital journey.
- Language selection that is visible and reversible.

## 10. Data model (illustrative)

A service entry should contain at least:

```json
{
  "id": "residence-registration",
  "title": {
    "en": "Residence registration",
    "de": "Wohnsitz anmelden"
  },
  "summary": {
    "en": "Official guidance for residence registration.",
    "de": "Offizielle Informationen zur Wohnsitzanmeldung."
  },
  "sourceUrl": "https://www.oesterreich.gv.at/en",
  "sourceStatus": "needs-verification",
  "lastChecked": null,
  "supportedLanguages": ["en", "de"],
  "workflowId": "residence-registration-v1",
  "demoOnly": true
}
```

Replace sample text and status only after reviewing the official source. Do not imply that this illustrative record is a verified legal summary.

## 11. Demo script (2–3 minutes)

1. Start with the citizen's natural-language request: “I moved last week. Where do I tell the authorities, and what do I need?”
2. Show intent resolution and a clarifying question rather than an overconfident answer.
3. Select the relevant branch and display official guidance.
4. Build a checklist with missing details clearly marked.
5. Demonstrate an ambiguous or unusual circumstance; show the assistant asking for clarification and avoiding a legal conclusion.
6. Demonstrate the human handoff summary.
7. End by showing the source link, demo-mode label, and text alternative to voice.

### Repeatable demo scenarios

Use these scenarios to show both the happy path and safe failure behavior. Reset the app between scenarios where a reset control is available.

| Scenario | Suggested prompt | What to demonstrate |
|---|---|---|
| Residence registration | “I moved last week. Where do I tell the authorities, and what do I need?” | Intent routing, a clarifying question if needed, official source link, checklist, and unknown details |
| Passport renewal | “My passport expires soon. How do I renew it?” | Service discovery, simple explanation, and no invented fees or processing times |
| Explain a letter | “Can you explain this official letter in plain English?” | Separate quoted letter facts from the assistant's explanation; remind users to redact personal identifiers |
| Ambiguous request | “I need a new document.” | Ask what kind of document rather than guessing |
| Unsupported complex case | “My residence situation is unusual. Am I eligible?” | State the limitation and offer official/human support instead of a legal determination |
| Voice/API failure | Deny microphone permission or simulate an unavailable service if the demo supports it | Text fallback, clear error, retry, and no false success claim |

These are recommended walkthrough scenarios, not a claim that every branch has been tested in the current hosted build. Mark any simulated response clearly during the demo.

## 12. Evaluation plan

Map tests to the challenge rubric:

- **End-to-end journey and UX:** complete the chosen task from an everyday-language request to a useful next step.
- **Conversational design and accessibility:** colloquial input, mixed language, text-only use, keyboard navigation, and readable explanations.
- **Accuracy and solution quality:** correct intent, source link, workflow branch, and structured checklist.
- **Uncertainty and guardrails:** ambiguous requests, missing details, unsupported claims, and unusual circumstances.
- **Robustness and polish:** speech failure, network/API failure, unknown service, reset, and consistent demo behavior.

Report measured test results only after actually running the tests. Distinguish implemented behavior, simulated behavior, and future work.

## 13. Repository structure (suggested)

```text
amtease-ai/
├── README.md
├── PROJECT_SPEC.md
├── docs/
│   ├── ARCHITECTURE.md
│   ├── SAFETY_AND_GUARDRAILS.md
│   ├── ACCESSIBILITY.md
│   └── DEMO_SCRIPT.md
├── src/
│   ├── app/
│   ├── components/
│   ├── features/
│   │   ├── conversation/
│   │   ├── service-discovery/
│   │   ├── workflows/
│   │   └── handoff/
│   ├── adapters/
│   │   ├── voice/
│   │   └── api/
│   ├── data/
│   │   ├── services/
│   │   └── workflows/
│   └── i18n/
├── tests/
│   ├── intent/
│   ├── workflows/
│   ├── guardrails/
│   └── e2e/
└── public/
```

This is a suggested structure; adjust it to the actual repository and do not claim files exist until they do.

## 14. Setup and local development

### Try the live demo

Open [AmtEase AI](https://amtease.lovable.app). The live app is the easiest way to explore the concept without installing dependencies.

### Clone the repository

```bash
git clone https://github.com/helshahaby/AmtEase.git
cd AmtEase
```

**Repository note:** the repository listing currently exposes documentation, the pitch deck, challenge brief, and demo video; a local application source tree and `package.json` could not be confirmed from the listed files. Therefore, local execution is conditional on the source code and package manifest being present.

If `package.json` exists in the repository root, inspect its scripts first:

```bash
npm install
npm run
```

Then run the development command actually declared by the project (commonly `npm run dev` for a Vite app). Only run build or test commands if matching scripts are listed. If the application source is in a subdirectory, change into that directory first. Do not create a `.env` file unless the source code documents the required variables; keep all privileged API keys server-side and never commit secrets.

For architecture, proposed mock API contracts, security, error handling, and deployment guidance, see [`TECHNICAL_DOCUMENTATION.md`](TECHNICAL_DOCUMENTATION.md). These describe intended design and must not be interpreted as proof that every endpoint or integration is live.

## 15. Limitations and roadmap

### Prototype boundaries
- Government backends may be simulated.
- The initial catalogue may contain only a small set of reviewed services.
- Voice quality depends on the selected provider, network, language, and device.
- The assistant is not a legal representative and does not submit official applications.
- Service coverage and translations need ongoing maintenance.

### Next steps
1. Verify the first 1–2 workflows with official sources and a public-service subject-matter reviewer.
2. Build a deterministic workflow engine with explicit required/optional fields.
3. Add source freshness monitoring and provenance in the UI.
4. Test with people across age groups, language proficiency, disability, and digital confidence.
5. Add authenticated government integrations only after privacy, security, legal basis, and authorization are established.
6. Publish a reproducible test suite and a short demo recording.

## 16. Project status discipline

When presenting the project, use three labels consistently:

- **Implemented:** works in the running prototype and has been tested.
- **Simulated:** represented by mock data or a fake API for demonstration.
- **Planned:** a future integration or capability.

This avoids overstating readiness and makes the concept credible to technical and public-sector reviewers.
