# 📊 Diagram: Exam Lifecycle & State Machine Flow

End-to-end operational flow of a TNPSC Group 4 mock examination attempt: from blueprint curation and live simulation runner to server-side evaluation and automated mistake rectification.

```mermaid
flowchart TD
    subgraph Blueprint["1. Blueprint Curation & Mock Assembly"]
        CURATE["Admin Curates 200 Questions<br/>(Part A: 100 Tam, Part B: 75 GS, Part C: 25 Apt)"]
        PUBLISH["Mock Test Published & Marked Live<br/>(status: isPublished = true)"]
        CURATE --> PUBLISH
    end

    subgraph CandidateStart["2. Exam Session Initialization"]
        START["Student Launches Mock Exam"]
        CREATE_ATTEMPT["Create MockAttempt Record<br/>(status: IN_PROGRESS, startTime: now)"]
        FETCH_LIVE["Call /api/mock-tests/[id]/live<br/>Sanitizes Correct Options & Explanations"]
        LOAD_PALETTE["Initialize 200-Question OMR Palette & 180-min Timer"]
        
        PUBLISH --> START
        START --> CREATE_ATTEMPT
        CREATE_ATTEMPT --> FETCH_LIVE
        FETCH_LIVE --> LOAD_PALETTE
    end

    subgraph ActiveRunner["3. Live 180-Minute Execution Loop"]
        NAV["Candidate Navigates Sections & Questions"]
        SELECT["Select Option (A, B, C, D) or Mark for Review"]
        AUTOSAVE["Debounced Autosave to Server<br/>(POST /api/mock-tests/[id]/autosave)"]
        BUFFER["Local Buffer Sync<br/>(Resilient to momentary offline dropouts)"]
        
        LOAD_PALETTE --> NAV
        NAV --> SELECT
        SELECT --> AUTOSAVE
        SELECT --> BUFFER
        AUTOSAVE --> NAV
    end

    subgraph Submission["4. Atomic Submission & Server Scoring"]
        SUBMIT["Submission Triggered<br/>(Manual Submit or 180m Timer Expiry)"]
        EVAL_SRV["evaluation.service.ts Fetches Authoritative Keys"]
        COMPUTE["Calculate Score:<br/>- Correct * 1.5 Marks<br/>- Part A, B, C Sub-scores<br/>- Accuracy % & Time-per-Q"]
        UPDATE_ATTEMPT["Update MockAttempt Record<br/>(status: SUBMITTED, score, endTime)"]
        
        NAV --> SUBMIT
        SUBMIT --> EVAL_SRV
        EVAL_SRV --> COMPUTE
        COMPUTE --> UPDATE_ATTEMPT
    end

    subgraph PostExam["5. Diagnostic Analytics & Mistake Remediation"]
        SCORECARD["Display Instant Detailed Scorecard & Rank Band"]
        INJECT_MISTAKES["Mistake Book Automation:<br/>Insert/Increment Wrong Answers"]
        SPACED_REV["Mistake Book Spaced-Repetition Loop:<br/>Targeted Re-test until Mastered"]
        
        UPDATE_ATTEMPT --> SCORECARD
        UPDATE_ATTEMPT --> INJECT_MISTAKES
        INJECT_MISTAKES --> SPACED_REV
    end
```
