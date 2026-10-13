# 📸 TargetTNgrp4 — UI Previews & Interface Catalog

This document catalogues the primary user interfaces, student exam runners, and administrative portals powering TargetTNgrp4.

---

## 🏛️ Brand & Identity

<p align="center">
  <img src="targettn-logo.png" alt="Target TN Group 4 Official Platform Logo" width="340" style="border-radius: 12px; box-shadow: 0 4px 20px rgba(0,0,0,0.12);" />
</p>

---

## 🖥️ Platform Interface Catalog

### 1. Bilingual Landing Page (`/`)
- **Hero Section:** High-impact bilingual headline in Tamil and English, highlighting the 200-question authentic blueprint simulation.
- **Real-Time Stats:** Live counters showcasing total registered aspirants, questions authored, and mock tests conducted.
- **Syllabus & Blueprint Showcase:** Visual breakdown of Part A (General Tamil 100Q), Part B (General Studies 75Q), and Part C (Aptitude 25Q).
- **Automated Mistake Book Teaser:** Interactive explainer demonstrating how wrong answers are converted into mastered concepts.

---

### 2. Student Portal & Dashboard (`/(student)/dashboard`)
- **KPI Summary Cards:** Total tests attempted, cumulative marks, and overall accuracy percentage.
- **Daily Target Widget:** 15-question daily routine curated for the current IST date with instant completion rewards.
- **Streak Tracker:** Visual streak counter motivating candidates to maintain unbroken daily study streaks.
- **Weak Area Diagnosis Alert:** Dynamic warnings identifying specific syllabus topics requiring immediate revision.

---

### 3. Live 200-Question Exam Simulation Runner (`/(student)/mock-tests/[id]/live`)
- **Full Blueprint Layout:** Seamless switching between Part A, Part B, and Part C with question counters.
- **180-Minute Countdown Timer:** Sticky header timer with color shifts at 15 minutes and 5 minutes remaining.
- **200-Bubble OMR Palette:** Color-coded status circles (Answered: Emerald, Marked for Review: Indigo, Unanswered: Amber, Not Visited: Slate).
- **Persistent Autosave:** Debounced background synchronization with local buffer fallback resilient to mobile network drops.

---

### 4. Comprehensive Scorecard & Diagnostic Review (`/(student)/mock-tests/[id]/results`)
- **Performance Summary:** Total marks achieved out of 300, accuracy percentage, and time spent.
- **Section-Wise Breakdown:** Individual scores for Part A (Tamil /150), Part B (General Studies /112.5), and Part C (Aptitude /37.5).
- **Cutoff Predictor:** Projected selection probability against historical district and communal category cutoffs.
- **Question-by-Question Review:** Bilingual question viewer with correct answer indicators and comprehensive explanations.

---

### 5. Automated Spaced-Repetition "Mistake Book" (`/(student)/mistake-book`)
- **Mistake Inventory:** Filterable repository of all historically missed questions across mock tests and practice tests.
- **Mastery Progress Indicators:** Visual badges indicating `wrongCount` and `consecutiveCorrect` count towards the 2-in-a-row mastery threshold.
- **Targeted Re-Test Launcher:** 1-click launcher generating custom re-tests drawn exclusively from unmastered questions.

---

### 6. Admin Enterprise Question Bank Management (`/(admin)/admin/questions`)
- **Universal Enterprise Table Standard (TETS):** Server-side SQL pagination with dynamic page sizes (10, 20, 50, 100).
- **Live Status Tab Counts:** Real-time counts for All, Published, Draft, Part A, Part B, and Part C tabs via atomic `db.$transaction`.
- **400ms Debounced Search:** Stutter-free search across English and Tamil question texts.
- **Bilingual Action Modals:** Question preview, edit, and deletion modals with baseline Tamil typography.

---

### 7. Bulk CSV Ingestion Pipeline (`/(admin)/admin/bulk-import`)
- **PapaParse Stream Parser:** Drag-and-drop CSV importer with automated header mapping.
- **Zod Validation Pipeline:** Pre-commit validation highlighting formatting errors row-by-row with line numbers.
- **Batch Database Commit:** Transactional batch insertion (`createMany`) ensuring zero partial imports.

---

### 8. Payment Reconciliation & Manual UPI Approval (`/(admin)/admin/payments`)
- **Dual Gateway Ledger:** Unified transaction table displaying Razorpay orders and direct manual UPI submissions.
- **1-Click UPI Approval:** Administrator inspects candidate UTR reference and activates subscription passes with a single click.
- **Audit Trail:** Comprehensive audit logging tracking administrator actions in `admin_audit_logs`.
