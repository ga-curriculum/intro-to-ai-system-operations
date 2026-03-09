# SME Feedback: Introduction to AI System Operations
**Module:** Week 1, Class 1 — Introduction to AI System Operations  
**Reviewer:** Sumit Rahman  
**Documents Reviewed:** README.md (student-facing), Instructor Guide  

---

## Overall Assessment

The lesson is well-structured and conceptually sound. It covers the three Competency Map learning objectives in a logical sequence, and the instructor guide provides solid delivery scaffolding. There are, however, several gaps and inconsistencies that should be addressed before this module goes live.

---

## 1. Alignment with Syllabus and Competency Map

| Learning Objective (Competency Map) | Covered in README? | Covered in Instructor Guide? | Notes |
|---|---|---|---|
| Describe the key components of AI-enabled systems and their interdependencies | ✅ Yes | ✅ Yes | Well covered with the dynamic pricing example |
| Explain the lifecycle of AI models from development to production | ✅ Yes | ✅ Yes | Good Mermaid diagrams; minor inconsistency noted below |
| Analyse how operational practices maintain performance and reliability in deployed AI systems | ✅ Yes | ✅ Yes | KS test example is a good anchor |

**Verdict:** All three LOs are addressed. The module aligns with the Competency Map entry for this lesson.

---

## 2. README vs. Instructor Guide Consistency

**Issue 2.1 — Duplicate Content Without Differentiation**  
The README and Instructor Guide share the same content verbatim for Sections 1–6. This is workable if the README is intended purely as the student-facing lecture notes and the Instructor Guide is additive. However, the current structure risks students accidentally accessing instructor-only materials if both are hosted in the same repository folder without access controls. Recommend clearly separating into a `student/` and `instructor/` directory structure, or clearly labelling the README as "Lecture Notes" and keeping the Instructor Guide strictly internal.

**Issue 2.2 — Mermaid Diagrams Are Not Equivalent**  
The README (Section 3) shows a vertical `graph TD` lifecycle diagram (Problem Definition → Monitoring → back to Problem Definition). The Instructor Guide shows a horizontal `graph LR` component architecture diagram (Data Pipeline → Feature Store → … → Feedback Loop). These are two *different* diagrams serving two different purposes. The instructor guide should explicitly note that the `graph LR` diagram is an *additional* architectural walkthrough diagram for live whiteboarding, separate from the lifecycle diagram in the student notes. As written, it looks like a replacement rather than a complement, which could confuse instructors.

**Issue 2.3 — Lab Section Mismatch**  
The README lab section (Section 5) says only: *"refer to the dedicated Lab Guide."* The Instructor Guide contains the full lab guide inline. This is fine as a structure, but the README should at least state the lab deliverable clearly (e.g., "Your deliverable is a Draw.io architecture diagram submitted as a PNG") so students know what they are working toward before opening the lab guide.

---

## 3. Pacing Guide Alignment

The Instructor Guide allocates time as follows:

| Segment | Allocated | Assessment |
|---|---|---|
| Overview & Intro | 10 min | Appropriate |
| Key Components | 30 min | Appropriate — 5 components is a lot; instructor tip to use whiteboard is critical |
| Model Lifecycle | 30 min | Slightly generous for material; could free 5–10 min for more Q&A |
| Operational Practices | 30 min | **Tight.** Live code demo + concept drift discussion + AI prompt exercise in 30 min is ambitious |
| Lab | 15 min | **Insufficient.** Draw.io diagramming of an 8-component system with dependency mapping is a 30–45 minute task at minimum |
| Wrap-up | 5 min | Appropriate |

**Recommendation:** The lab is under-resourced at 15 minutes. The Pacing Guide allocates 60 minutes of lab time per class session (1 hour lecture, 1 hour lab). The current instructor guide conflates lecture + lab into a single 120-minute block without preserving the 60-minute lab allocation. The Instructor Guide should be restructured to reflect the actual pacing: 60 minutes instruction, 60 minutes lab.

---

## 4. Coding Examples — Sufficiency for the Lab

The lecture contains one coding example: a Kolmogorov-Smirnov (KS) test using `scipy.stats.ks_2samp` and `numpy`.

**What the example covers well:**
- Comparing two distributions using a statistical test
- Interpreting a p-value threshold for drift alerting
- The conceptual bridge between data drift and operational action

**Gaps relative to the lab:**
- The lab (System Landscape Mapping) is primarily a diagramming exercise in Draw.io. The KS test snippet is not directly used in Draw.io — it is useful for understanding *why* monitoring components exist in the architecture, but students are not told how it connects to what they will diagram.
- There is no example showing how to load or inspect data files programmatically. If the lab notebook requires students to load CSV or JSON data, they have no lecture reference for `pandas.read_csv()` or `json.load()` — which are basic but should not be assumed for an intermediate audience that may have varied Python exposure.
- The example uses `np.random.normal()` to generate fake data. The lab notebook will use real pre-generated data files. Students should be shown at least one example of loading a CSV and accessing a column before being asked to do so independently.

**Recommendation:** Add a 5-line code snippet to Section 4 of the README demonstrating how to load and inspect a feature distribution CSV. This directly scaffolds the lab without adding lecture time.

---

## 5. Theory Coverage for the Lab

The lab asks students to map a production AI system's architecture. The following concepts are taught and are sufficient for the guided portion:

- ✅ The 5 core component types (data ingestion, feature store, model registry, serving infra, monitoring)
- ✅ Dependency relationships between components
- ✅ The concept of a feedback loop (retraining trigger)
- ✅ SLA/latency concepts via the pricing engine example

**Gap for the stretch component:**  
The stretch problem in the lab asks students to reason about a *single point of failure* (SPOF) in their architecture and propose a redundancy strategy. The lecture does not cover redundancy, failover, or high availability — those topics appear in Week 6. This is an appropriate stretch problem: students should be able to reason about SPOFs conceptually even without formal HA coverage, and this is explicitly a "learn without supervision" task. The lab README should call this out as a stretch challenge explicitly.

---

## 6. Library Installation

**Issue — Missing Installation Instructions**  
The README uses `scipy` and `numpy` in the code example but provides no installation instructions. These are not standard library modules. For an intermediate audience, some students will have them from prior work; others will not. The README should include a note such as:

```bash
pip install numpy scipy
```

Or reference a `requirements.txt`. As per our production standard, a `requirements.txt` and setup instructions will be included in the lab README going forward.

---

## 7. Minor Issues

- **Instructor Guide — AI Prompt Bank:** The prompts provided are good. The Page-Hinkley test prompt is a solid stretch suggestion. However, the prompt about "concept drift using the Page-Hinkley test on a streaming dataset" is more advanced than the KS test in the README — instructors should be advised that this is a stretch reference, not a required follow-up.
- **Section 6 (Tools):** AWS SageMaker is mentioned as a stretch goal for the lab (tying components to real AWS services). However, the Competency Map lists GitHub, Draw.io, and AWS Free Tier as required tools. The README treats AWS Free Tier casually ("for referencing service documentation"), but the Competency Map expects active use. The lab README should make AWS service mapping an explicit task, not an optional one.

---

## Summary of Action Items

| Priority | Issue | Owner |
|---|---|---|
| High | Restructure Instructor Guide pacing to respect the 60-min lecture / 60-min lab split | Content Author |
| High | Lab section in README must state the deliverable explicitly | Content Author |
| High | Add library installation note or reference to requirements.txt in README | Content Author |
| Medium | Clarify that the two Mermaid diagrams serve different purposes | Content Author |
| Medium | Add a CSV-loading code snippet to the README to scaffold the lab notebook | Content Author |
| Medium | Stretch problem (SPOF/redundancy) should be clearly labelled in the lab as beyond lecture scope | Lab Author |
| Low | Standardise British/Canadian English spelling throughout | Editor |
| Low | Clarify the AWS Free Tier expectation — component mapping should be an explicit task | Content Author |
