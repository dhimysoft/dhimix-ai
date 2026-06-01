# Model Card: DHIMIX AI Recommendation Engine

**Platform:** DHIMIX AI — AI-Native Music Intelligence Platform
**Deployment:** Live — [dhimix-ai.vercel.app](https://dhimix-ai.vercel.app)
**Version:** 1.0.0 · May 2026
**Maintainer:** Dhimy Jean · Dhimix Entertainment LLC

---

## 1. Model Overview

DHIMIX AI uses a proprietary rule-based recommendation engine to rank and surface music tracks from the Jamendo open catalog and Cloudflare R2 Cloud Showcase. The engine combines multi-factor scoring with retrieval-augmented evidence and a Music DNA personalization layer.

The system is **not a trained machine learning model**. It applies explicit, deterministic scoring logic calibrated from real music selection expertise — enabling full auditability and transparent explainability at every step.

**Core capabilities:**
- Multi-factor composite scoring across genre, mood, energy, acoustic profile, temporal context, and catalog metadata
- Three selectable scoring modes — Genre-First · Mood-First · Energy-Focused — shifting weight emphasis per listener intent
- Retrieval-augmented recommendations with knowledge-base evidence surfaced as tags
- Per-recommendation confidence scoring — HIGH / MEDIUM / LOW — with honest degradation on weak signals
- Music DNA preference vector derived from listening history and feedback

---

## 2. Intended Use

**Intended:**
- Live music recommendation for individual listeners via the DHIMIX AI web platform
- Demonstration of explainable, auditable AI recommendation architecture in a deployed consumer product
- Foundation for future personalization extensions (Advanced Music DNA, AI Listening Journeys)

**Not intended:**
- High-stakes decisions (health, legal, financial, safety)
- Large-scale enterprise personalization without human review
- Inference of sensitive personal characteristics from music preferences

---

## 3. System Architecture Summary

| Component | Description |
|-----------|-------------|
| **Catalog Sources** | Jamendo API (Creative Commons, primary) · Cloudflare R2 Cloud Showcase (curated, secondary) |
| **Scoring Engine** | Proprietary multi-factor scoring — 10 signal dimensions · mode-weighted · Music DNA bias |
| **Confidence Model** | Normalized signal strength + knowledge-base evidence retrieval · calibrated tier classification |
| **Explainability** | Per-recommendation: score dial · confidence bar · evidence tags · Why[] reasoning panel |
| **Personalization** | Music DNA preference vector — cross-session learning from feedback and listening history |
| **Fallback Behavior** | Low-confidence output triggers transparent degradation messaging — never silent failure |

---

## 4. Limitations

- **Catalog coverage:** Recommendations are constrained to tracks available in the Jamendo catalog and R2 Cloud Showcase. Obscure subgenres or regional music outside these catalogs will have lower coverage.
- **Cold start:** New users without listening history or explicit feedback receive lower Music DNA signal; recommendations rely more heavily on stated preferences until sufficient feedback accumulates.
- **Rule-based ceiling:** Deterministic scoring cannot model latent preference patterns the way a trained collaborative filtering model can. This is a deliberate trade-off for transparency and auditability.
- **Metadata quality:** Jamendo metadata quality varies by artist submission. Genre and mood tags are catalog-sourced; inconsistent tagging can affect scoring precision.
- **Contradictory inputs:** Internally contradictory preference profiles (e.g., lofi genre + high energy target) are handled stably but produce medium-confidence outputs by design — not an error state.

---

## 5. Bias and Risk Considerations

| Risk | Description | Mitigation |
|------|-------------|------------|
| **Catalog bias** | Genres overrepresented in Jamendo may appear more frequently | Artist + genre diversity constraints applied during ranking |
| **Energy over-fit** | Energy proximity can dominate on extreme input values | Mode-weighted scoring distributes emphasis across dimensions |
| **Feedback loop** | Strong Music DNA signal can narrow recommendations over time | User can reset DNA; source filters allow catalog-wide discovery |
| **Low-coverage inputs** | Rare genre/mood combinations may produce low-confidence results | Confidence tier + fallback messaging surfaces uncertainty rather than hiding it |
| **Preference inference** | Mood/energy inputs are used only for music matching within the session | No server-side storage of user profiles; all data localStorage-only |

---

## 6. Evaluation

**Reliability testing — v1.0.0:**

| Profile | Result | Confidence |
|---------|--------|------------|
| Happy / Pop / High Energy | PASSED | 0.75 |
| Chill / Lofi / Acoustic | PASSED | 0.74 |
| Deep Intense / Rock | PASSED | 0.77 |
| Adversarial: Lofi + Intense + energy=1.0 | PASSED — stable | 0.62 |

- **4/4 profiles passed** genre and mood alignment assertions
- **Average confidence: 0.599** across all evaluation profiles
- **Adversarial stability:** Internally contradictory inputs did not collapse the ranking — multi-factor scoring produced coherent, ranked output at medium confidence
- **pytest suite:** 4/4 unit tests passed

Evaluation is performed against predefined preference profiles that include both standard and adversarial inputs. Results are reviewed manually before each release.

---

## 7. Responsible AI Design

| Principle | Implementation |
|-----------|----------------|
| **Transparency** | Every recommendation surfaces score, confidence tier, evidence tags, and structured Why[] reasoning |
| **Honest uncertainty** | Confidence is a first-class output — never hidden, never inflated |
| **Graceful degradation** | Low-confidence results trigger explicit messaging rather than silent failure |
| **Input guardrails** | Invalid or out-of-range values are rejected or normalized at intake before scoring |
| **Privacy by design** | All user data (favorites, history, Music DNA) stored in browser localStorage — no server-side user profiles |
| **License transparency** | Per-track license status surfaced in the License Center for every catalog source |
| **Scope constraints** | System is explicitly scoped to entertainment recommendation — not diagnostic, clinical, or high-stakes |

---

## 8. AI-Assisted Development

DHIMIX AI was built using an AI-assisted development workflow with Claude Code as the primary engineering collaborator. Architectural decisions, system design, and product direction were made by the human engineer throughout.

**Workflow principles applied:**
- All AI-generated code was reviewed and accepted, modified, or rejected on a case-by-case basis
- The human engineer retained architectural authority; AI tooling was used for implementation acceleration
- AI suggestions that conflicted with design intent were explicitly rejected and documented
- The engineering journey is documented in full in [`docs/AI_Agent_Engineering_Journey.pdf`](./docs/AI_Agent_Engineering_Journey.pdf)

**Example — accepted suggestion:**
Integrating retrieval-aware evidence attachment into the main recommendation runtime, adding stage-level observability that improved evaluator traceability.

**Example — rejected/corrected suggestion:**
An initial AI-generated evaluation harness referenced profile data before the shared constant was defined, creating a broken dependency. Corrected by defining shared evaluation profiles in the main module and validating with the full pytest suite.

---

## 9. Future Development

| Area | Direction |
|------|-----------|
| **Embedding-based retrieval** | Semantic similarity search to complement rule-based scoring |
| **Confidence calibration** | Feedback-driven confidence recalibration over time |
| **Advanced Music DNA** | Session modeling and cross-session preference trajectory |
| **Collaborative signals** | Optional collaborative filtering layer for aggregate preference patterns |
| **Fairness metrics** | Diversity and coverage evaluation across genres, moods, and catalog sources |

---

## Reflection

The central design insight of DHIMIX AI is that transparency and engineering quality are not in tension — they reinforce each other. Rule-based scoring is auditable. Confidence surfacing builds user trust. Explicit fallback behavior communicates honesty rather than hiding uncertainty.

The result is a recommendation system where both the engineer and the user can understand exactly why each track was chosen. That interpretability is not a limitation of the approach — it is the product.

---

*For questions, partnership inquiries, or licensing: [dhimixentertainment@gmail.com](mailto:dhimixentertainment@gmail.com)*
