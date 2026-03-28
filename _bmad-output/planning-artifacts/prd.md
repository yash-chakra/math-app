---
stepsCompleted: ['step-01-init', 'step-02-discovery', 'step-02b-vision', 'step-02c-executive-summary', 'step-03-success', 'step-04-journeys', 'step-05-domain', 'step-06-innovation', 'step-07-project-type', 'step-08-scoping', 'step-09-functional', 'step-10-nonfunctional', 'step-11-polish', 'step-12-complete']
inputDocuments:
  - '_bmad-output/brainstorming/brainstorming-session-20260328-193241.md'
workflowType: 'prd'
documentCounts:
  briefCount: 0
  researchCount: 0
  brainstormingCount: 1
  projectDocsCount: 0
classification:
  projectType: desktop_app
  domain: edtech
  complexity: medium
  projectContext: greenfield
workflowStatus: complete
completedAt: '2026-03-28'
---

# Product Requirements Document - math-app

**Author:** Yashashree Chakrabor
**Date:** 2026-03-28





## Executive Summary

This product is a story-driven desktop math learning app for children, starting with a 10-year-old learner profile and expanding to similar age groups. It teaches arithmetic, algebra, and introductory calculus through short narrative lessons followed by guided problem practice. The core loop is: read a story, confirm reading completion, solve on paper, submit answer, receive immediate correctness feedback, and accumulate progression rewards. The product aims to improve math confidence and consistency by turning practice into an adventure structure rather than worksheet repetition.

### What Makes This Special

The primary differentiator is cartoon-led, narrative-first instruction with puzzle framing and emotional feedback, instead of conventional drill-first UX. The concept uses a themed world (Zagoomba jungle), character guidance (Donald Duck + companion dog), and reward systems (treasure chests, stars, crowns, expert ceremony) to sustain motivation across repeated practice cycles. The key insight is that children persist longer and learn more effectively when math challenges are embedded in playful story context with visible progression and identity-based milestones (for example, becoming Queen of a mastered area).

## Project Classification

- Project Type: `desktop_app`
- Domain: `edtech`
- Complexity: `medium`
- Project Context: `greenfield`


## Success Criteria

### User Success

- Learner reports noticeable math improvement within 30 days.
- Learner practices at least 3 days per week.
- Learner completes at least 1 full story-to-problem cycle per session.
- Weekly streak target: at least one 3-days-per-week streak by month 2.

### Business Success

- Minimum validation milestone: 1 active learner in first 3 months.
- Target validation signal: 5+ weekly active learners by month 3.
- Retention signal: at least 50% of active learners return the following week.

### Technical Success

- Desktop app initial load time < 5 seconds.
- Session crash or error rate < 1%.
- Progress save integrity: no lost progress in normal use; answers, points, and mastery persist at checkpoints.

### Measurable Outcomes

- 30-day learner self-improvement milestone achieved.
- Weekly usage target sustained: 3+ active practice days.
- Performance and reliability thresholds met: load < 5s and crash/error sessions < 1%.

## Product Scope

### MVP - Minimum Viable Product

1. Story-based learning loop for arithmetic, algebra, and calculus.
2. Practice and feedback loop: done-reading unlock, answer check, right/wrong feedback, and points.
3. Motivation and progression loop: difficulty levels, stars, badges, fresh stories/problems, and expert progression.
4. Local-first AI routing and budget manager to minimize cloud token usage.

### Growth Features (Post-MVP)

- Expanded world-building zones, tribe variants, and mission diversity.
- Advanced reward events including rare chests and seasonal modes.
- Enhanced learner support with deeper hints and progress insights.

### Vision (Future)

- A complete adventure learning universe where children progress from beginner to expert in each subject through endless story missions and identity milestones.


## User Journeys

### Learner - Success Path

A 10-year-old learner opens the app to improve math through fun story adventures in the Zagoomba jungle zone. The learner selects a topic, reads a story lesson, clicks done reading, solves the puzzle on paper, submits an answer, and receives immediate feedback. On correct answers, Donald gives a thumbs-up and a "quack quack" celebration. Correct answers increase points, stars, and mastery progression toward Expert rank. Journey checkpoints are recorded as `story_completed`, `problem_attempted`, `answer_submitted`, and `progress_saved`.

### Learner - Edge Case / Recovery

The learner submits a wrong answer and feels uncertain. The app responds with an encouraging message, Donald's "soooorrrryyyyy" empathy cue, a clue-style hint, and a retry path. If needed, the app provides a simplified fallback problem to restore confidence and momentum. Recovery is successful when the learner returns to active solving in the same session.

### Content Admin - ChatGPT / Grok

An AI content admin generates and updates fresh stories and math problems for each subject and difficulty level. Every generated item is passed through quality gates: age-appropriate language, correct subject/difficulty tagging, duplication checks, and curriculum-fit checks. Only content that passes all checks is published to the learner experience.

### Support Helper - ChatGPT / Grok

When a learner faces confusion or a mismatch in score/progression, AI support classifies the issue (scoring, hints, progression, or content). It explains the issue in child-friendly language, applies a correction or retry guidance, and confirms resolution so the learner can continue quickly.

### Journey Requirements Summary

- Story -> done reading -> problem -> answer check core loop
- Supportive wrong-answer recovery with retry and fallback
- Persistent progression state and reliable checkpoint saving
- AI-assisted content generation pipeline with quality gates
- AI troubleshooting flow for learner-facing issues


## Domain-Specific Requirements

### Compliance & Regulatory

- COPPA-aware child data handling with minimum data collection.
- FERPA-aware readiness for future school adoption scenarios.
- Parent/guardian consent path for future account-linked data sharing.
- Child-safe content policy for all generated stories and problems.

### Technical Constraints

- Data minimization for learner profile and progress storage.
- Reliable persistence of score/mastery checkpoints.
- AI output filtering for age-appropriate language and correctness.
- Session reliability target maintained: crash/error rate below 1%.

### Integration Requirements

- AI content generation interface for ChatGPT/Grok using structured prompts.
- Content moderation layer before learner-facing rendering.
- Offline-friendly local persistence for desktop-first usage.

### Risk Mitigations

- Risk: incorrect or unsafe generated content.
  - Mitigation: quality gates including tagging checks, duplication checks, safety filtering, and review rules.
- Risk: learner frustration after incorrect answers.
  - Mitigation: supportive feedback, clue-hint recovery, and easier fallback problems.
- Risk: score/progression mismatch.
  - Mitigation: checkpoint saves after each answer and validation on session resume.


## Innovation & Novel Patterns

### Detected Innovation Areas

- Narrative-learning fusion: story scenes and problem-solving are merged into one loop.
- Emotion-responsive feedback through character reactions for motivation.
- Identity-based mastery progression with role transformation at expert level.
- Dynamic variation engine using weather modes and fresh mission generation.
- AI content operations via ChatGPT/Grok with quality-gated publishing.

### Market Context & Competitive Landscape

- Most math apps center on drill repetition and streak mechanics.
- This concept differentiates through immersive narrative continuity and identity-based progression.
- Competitive durability depends on safe, high-quality, and consistently fresh generated content.

### Validation Approach

- Compare baseline problem loop vs story-integrated loop for completion and retention.
- Track story-to-problem completion rate.
- Track wrong-answer recovery success rate.
- Track repeat-session rate under dynamic weather and event modes.
- Track expert milestone completion per subject.

### Risk Mitigation

- Risk: novelty fatigue over time.
  - Mitigation: rotating templates, seasonal events, and freshness controls.
- Risk: generated content inconsistency.
  - Mitigation: strict quality gates, safety filters, and curated fallback packs.
- Risk: game elements overshadow learning goals.
  - Mitigation: rewards tied directly to solved tasks and mastery progression.


## Desktop App Specific Requirements

### Project-Type Overview

The product will launch as a Windows-first desktop application, with optional future expansion to macOS/Linux. It uses a minimal system integration surface and requires active internet connectivity for operation.

### Technical Architecture Considerations

- Desktop runtime must support Windows-first packaging and distribution.
- Network connectivity is a runtime requirement for core functionality.
- Local persistence is used for progress reliability, while the core learner flow remains online-first.
- Auto-update mechanism is out of scope.

### Platform Support

- MVP target platform: Windows desktop.
- Future platform expansion: macOS and Linux after MVP stabilization.

### Update Strategy

- No auto-update workflow in current scope.
- Updates use manual release and install flow.

### System Integration

- Minimal integration surface in MVP.
- Required integrations: AI content generation pipeline (ChatGPT/Grok), content moderation/quality gates, and local progress persistence.

### Online Connectivity Model

- Online-only operation model.
- App detects connectivity loss and blocks session start with clear messaging.
- Reconnect flow resumes learner context after internet recovery.

### Implementation Considerations

- Prioritize startup reliability and session continuity under normal network conditions.
- Add clear connectivity-state UX: online, reconnecting, offline blocked.
- Ensure score/mastery checkpoints remain consistent across online operations.


## Project Scoping & Phased Development

### MVP Strategy & Philosophy

- MVP approach: problem-solving MVP focused on proving measurable learner improvement in 30 days.
- MVP validation goal: confirm story-led practice drives consistency (3+ days/week) and retention.
- Resource requirements: minimum team of one full-stack developer, one QA engineer, and one product owner.

### MVP Feature Set (Phase 1)

**Core User Journeys Supported:** learner success path, learner recovery path, AI content admin path, and AI support helper path.

**Must-Have Capabilities:**

1. Launch with all three subjects: arithmetic, algebra, and calculus.
2. Full Donald + jungle themed experience in MVP.
3. Story -> done-reading gate -> problem -> answer check core loop.
4. Supportive incorrect-answer recovery with hint and fallback.
5. Points, stars, badges, mastery progression, and expert milestone.
6. AI content generation from day 1 with quality and safety gates.
7. Online-only runtime with clear connectivity handling.
8. Reliable checkpoint persistence for score and progress integrity.
9. Local-first AI routing with token budget controls and controlled cloud fallback.

### Post-MVP Features

**Phase 2 (Post-MVP):**

- Expanded zones, tribe variants, and seasonal event mechanics.
- Deeper hint intelligence and learner analytics.
- Enhanced content variety controls and anti-repetition tuning.

**Phase 3 (Expansion):**

- Platform expansion beyond Windows to macOS and Linux.
- Broader curriculum depth and advanced challenge modes.
- Rich identity customization and long-term progression systems.

### Risk Mitigation Strategy

**Technical Risks:** AI content inconsistency.
- Mitigation: pre-publish quality gates, moderation filters, curated fallback packs.

**Market Risks:** novelty attraction without sustained learning outcomes.
- Mitigation: monitor 30-day improvement, weekly practice consistency, and return rates.

**Resource Risks:** full themed AI-first MVP may stretch delivery timeline.
- Mitigation: lock non-core expansion items to post-MVP phases and protect core loop delivery.


## Functional Requirements

### Learning Flow & Session Control

- FR1: Learner can start a learning session from the desktop app home experience.
- FR2: Learner can choose a subject from arithmetic, algebra, and calculus.
- FR3: Learner can view a story lesson before practice is unlocked.
- FR4: Learner can explicitly confirm story completion via a done-reading action.
- FR5: System can gate practice availability until story completion is confirmed.
- FR6: Learner can request the next problem within an active session.

### Problem Generation & Practice

- FR7: System can generate subject-appropriate practice problems for the selected subject.
- FR8: System can generate problems according to selected difficulty level.
- FR9: System can present fresh problem variations across repeated sessions.
- FR10: Learner can submit an answer for each generated problem.
- FR11: System can evaluate submitted answers and determine correctness.
- FR12: System can provide immediate correctness feedback after answer submission.

### Recovery, Hints, and Learning Support

- FR13: System can provide supportive feedback after incorrect answers.
- FR14: System can provide hint-style recovery guidance after incorrect answers.
- FR15: System can offer a retry path for incorrect answers.
- FR16: System can provide simplified fallback problems when repeated struggle is detected.
- FR17: Learner can continue the same session after recovery without forced restart.

### Progression, Rewards, and Mastery

- FR18: System can assign points based on completed and correct practice outcomes.
- FR19: System can track stars and badge progression for learners.
- FR20: System can track subject-level mastery progression over time.
- FR21: System can determine when expert status is reached for a subject.
- FR22: System can trigger expert milestone recognition events.
- FR23: System can expose learner progress state within the app session.

### Story World, Theme, and Experience Identity

- FR24: System can deliver Donald + jungle themed learning experiences in MVP.
- FR25: System can map stories, rewards, and progression to themed world constructs.
- FR26: System can include themed variation signals that alter challenge/reward context.
- FR27: System can preserve narrative continuity between story and problem phases.
- FR28: System can present story content in comic-panel style during learning sessions.
- FR29: System can use Zagoomba as the initial themed learning zone at launch.
- FR30: System can include friendly/funny tribe personas, helpers, and enemies in mission context.
- FR31: System can apply weather challenge rules where rain lowers challenge, thunder increases challenge, and rainbow enables bonus treasure rounds.
- FR32: System can map treasure rewards to defined categories including gold/silver, gemstones, and sea treasure.
- FR33: System can trigger Donald celebration cues for correct answers and empathy cues for incorrect answers.

### AI Content Operations & Quality Governance

- FR34: AI content admin can generate story content for each subject and difficulty.
- FR35: AI content admin can generate problem sets for each subject and difficulty.
- FR36: System can apply quality gates to generated content before learner delivery.
- FR37: System can detect and prevent duplicate or low-variation generated content.
- FR38: System can enforce age-appropriate language constraints on generated content.
- FR39: System can enforce curriculum-fit tagging for generated learning content.
- FR40: System can publish only approved content to learner-facing flows.

### AI Support Operations

- FR41: AI support helper can classify learner-reported issues by issue type.
- FR42: AI support helper can provide child-friendly explanations for detected issues.
- FR43: AI support helper can guide learner recovery actions for scoring/progression/content problems.
- FR44: AI support helper can confirm issue resolution state.

### Data, State, and Reliability Behavior

- FR45: System can persist learner progress checkpoints during session flow.
- FR46: System can restore learner progress state on session resume.
- FR47: System can keep score and mastery state consistent across retries and resumed sessions.
- FR48: System can operate in online-only mode for learner sessions.
- FR49: System can detect loss of internet connectivity during session flow.
- FR50: System can communicate connectivity status and block incompatible actions when offline.
- FR51: System can resume learner context after connectivity is restored.

### Safety, Privacy, and Compliance Controls

- FR52: System can minimize personal data collection for child learners.
- FR53: System can apply child safety policy checks to learner-facing content.
- FR54: System can support consent-governed data handling patterns for future guardian-linked features.
- FR55: System can maintain auditable content quality decisions for generated learning artifacts.

### AI Routing & Cost Optimization

- FR56: System can route AI requests to local Ollama qwen3 by default.
- FR57: System can evaluate local response quality before deciding fallback.
- FR58: System can escalate requests to ChatGPT/Grok when local quality is below threshold.
- FR59: System can apply retry-then-escalate logic (one local retry before cloud fallback).
- FR60: System can enforce per-session and per-day cloud token budgets.
- FR61: System can support budget modes (`Saver`, `Balanced`, `Boost`) that change fallback strictness.
- FR62: System can apply different cloud budget rules by subject and difficulty.
- FR63: System can log routing decisions (`local`, `retry-local`, `cloud-fallback`) for audit and tuning.
- FR64: System can show learner-safe messaging when cloud fallback is blocked by budget limits.
## Non-Functional Requirements

### Performance

- NFR1: App startup time shall be less than 5 seconds under normal network conditions.
- NFR2: Learner-visible actions (topic load, story load, answer result) should complete within 2 seconds for standard operations.
- NFR3: System shall maintain stable interaction flow during repeated problem attempts within a session.

### Security & Privacy

- NFR4: Child-related data shall follow data-minimization principles.
- NFR5: Learner progress data shall be protected in transit and at rest.
- NFR6: Generated content shall pass safety and age-appropriateness checks before display.
- NFR7: System shall support COPPA-aware handling and FERPA-aware future compatibility.

### Reliability

- NFR8: Session crash/error rate shall remain below 1%.
- NFR9: Progress checkpoints (score, stars, mastery) shall persist without loss during normal use.
- NFR10: Reconnect behavior shall recover learner context without duplicate or missing progression updates.

### Accessibility

- NFR11: UI shall provide clear readable text and contrast suitable for child learners.
- NFR12: Core actions shall be operable via keyboard and pointer input.
- NFR13: Feedback states shall be communicated with text, not color alone.

### Integration

- NFR14: AI content generation integration shall use predictable request/response contracts.
- NFR15: Content moderation and quality gates shall execute before learner-facing publication.
- NFR16: Integration failures shall trigger safe fallback behavior and clear learner-facing messaging.



### AI Efficiency & Routing Quality

- NFR17: At least 80% of AI generation requests should be served by local qwen3 in `Balanced` mode under normal operation.
- NFR18: Router decision overhead should be less than 200 ms per request.
- NFR19: Cloud budget enforcement must be deterministic and consistent across retries and resumes.
- NFR20: Routing logs must include timestamp, route path, and fallback reason for 100% of AI calls.