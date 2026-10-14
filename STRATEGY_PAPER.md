# AmtEase AI — Short Strategy Paper

**Challenge:** Digital Assistants: Evolution to 2030 — Public Sector  
**Project lead:** Hossam Elshahaby  
**Live prototype:** https://amtease.lovable.app  
**Repository:** https://github.com/helshahaby/AmtEase  
**Pitch:** https://youtu.be/N0n5MQYxjcQ

## 1. Vision

By 2030, interacting with public services should feel understandable and navigable regardless of a person's confidence with digital systems, familiarity with administrative language, or preferred way of communicating. AmtEase AI aims to be a trusted guidance layer between a citizen's everyday-language goal and the relevant official Austrian service. It explains, clarifies, organizes next steps, and points back to authoritative sources; it does not replace government decision-makers or official portals.

## 2. Target personas

- **Time-constrained, digitally confident citizen:** knows the desired outcome but not the right service name, form, or sequence. Needs fast routing and a concise checklist.
- **Person with lower digital confidence:** benefits from a guided, step-by-step experience, visible recovery options, plain language, and a human-support route. Voice must remain optional.
- **German learner or non-native speaker:** may use simple, colloquial, or mixed German and English. Needs respectful clarification and understandable explanations.
- **Citizen with an unusual or complex case:** has missing details or circumstances not covered by the standard journey. Needs explicit uncertainty, safe boundaries, and referral to an appropriate official channel rather than a guess.
- **Public-service team (secondary stakeholder):** may benefit when people arrive with clearer expectations, a structured checklist, and a concise user-reviewed summary of their question.

These are design personas to validate through user research, not claims about measured user segments.

## 3. Value proposition

**For citizens:** “Tell AmtEase what you need in your own words; understand the likely next step, see the official source, and leave with a practical checklist.”

**For public administrations:** a potential front-door guidance layer that can improve findability and comprehension without requiring citizens to know the administrative structure. If validated, it may reduce avoidable repeat searches and poorly prepared enquiries while preserving human support for complex cases.

**Differentiators:**
- Natural-language entry instead of requiring official terminology.
- German/English-oriented experience with voice and equivalent text interaction.
- Plain-language explanation and task checklists.
- Source links and verification status near relevant guidance.
- Clarification and human fallback when the answer is uncertain.
- Clear separation of real official information, simulated demo behavior, and future integrations.

## 4. Public-sector impact and measurement

AmtEase should be evaluated as an inclusion and service-navigation aid, not merely as a chatbot. A pilot should measure:

| Outcome | Suggested measure | Validation approach |
|---|---|---|
| Findability | Time to reach the correct official service | Compare assisted and unassisted task completion |
| Comprehension | User can explain the next step in their own words | Short comprehension check after a task |
| Completion readiness | Checklist completeness and number of unknowns identified | Reviewer-scored task scenarios |
| Inclusion | Task success by language preference, digital confidence, and assistive-technology use | Moderated usability sessions with diverse participants |
| Trust and safety | Source-link correctness, unsupported-claim rate, and appropriate handoff rate | Expert review of scripted cases and sampled sessions |
| Operational value | Change in avoidable repeat enquiries or time spent explaining basic steps | Baseline and post-pilot comparison with service teams |

No impact figures are claimed at this concept stage. Establish a baseline, define privacy-preserving measurement, and agree success thresholds with the participating authority before a pilot.

## 5. Delivery strategy

1. **Start narrow:** validate residence registration/change of address and one additional high-frequency journey using official sources.
2. **Ground every service:** maintain a curated catalogue with source URL, language, last-checked date, and verification status.
3. **Keep the workflow deterministic:** use structured steps and explicit branching for consequential questions; use generative AI for explanation, not as the authority for legal facts.
4. **Design for recovery:** support text-only use, transcript correction, retries, reset, and human fallback.
5. **Pilot responsibly:** conduct accessibility testing, public-service expert review, privacy/security assessment, and user research before using real personal data or connecting to government systems.
6. **Scale through governance:** assign owners for source updates, incident response, evaluation, translations, and model/provider changes.

## 6. Public-value safeguards

AmtEase must not make binding legal or eligibility decisions, fabricate deadlines or fees, claim that an application has been submitted, or imply that a human contact has occurred when it has not. The assistant should minimize personal data, encourage redaction of official letters, disclose simulated functions, and direct people to the official authority whenever information is ambiguous or the case falls outside supported workflows.

## 7. Strategic position

AmtEase is best positioned as an **accessible guidance and service-discovery companion** that complements existing Austrian digital public services. Its public value depends on source integrity, accessible design, transparent limitations, and demonstrated improvement in citizen comprehension—not on replacing official channels or maximizing automation.

## Reference starting points

- Austrian government portal: https://www.oesterreich.gv.at/en
- Official digital-services and forms search: https://www.oesterreich.gv.at/en/formsearch
- Official digital-service landing pages: https://www.oesterreich.gv.at/en/landingpages
