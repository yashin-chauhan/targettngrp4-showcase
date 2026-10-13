# 📊 Diagram: Database Entity Relationship (ERD)

Core domain entities and relational mappings enforcing bilingual question management, exam simulation states, mistake tracking, and subscription governance.

```mermaid
erDiagram
    USER ||--o{ ADMIN_SESSION : establishes
    USER ||--o{ ADMIN_AUDIT_LOG : actions_by
    USER ||--o{ PRACTICE_SESSION : attempts
    USER ||--o{ MOCK_ATTEMPT : submits
    USER ||--o{ MISTAKE_BOOK : tracks_errors
    USER ||--o{ BOOKMARK : stars
    USER ||--o{ PAYMENT : makes
    USER ||--o{ SUBSCRIPTION : holds

    QUESTION ||--o{ DAILY_TARGET_ITEM : scheduled_in
    QUESTION ||--o{ PRACTICE_SESSION_QUESTION : answered_in
    QUESTION ||--o{ MOCK_TEST_QUESTION : curated_into
    QUESTION ||--o{ MOCK_ANSWER : recorded_in
    QUESTION ||--o{ MISTAKE_BOOK : cataloged_in
    QUESTION ||--o{ BOOKMARK : bookmarked_in

    PRACTICE_SESSION ||--|{ PRACTICE_SESSION_QUESTION : contains
    
    MOCK_TEST ||--|{ MOCK_TEST_QUESTION : composed_of
    MOCK_TEST ||--o{ MOCK_ATTEMPT : attempted_via
    
    MOCK_ATTEMPT ||--|{ MOCK_ANSWER : stores
    
    MISTAKE_BOOK ||--|{ MISTAKE_ATTEMPT : retested_via

    PLAN ||--o{ PAYMENT : billed_for
    PLAN ||--o{ SUBSCRIPTION : grants

    PAYMENT ||--o| SUBSCRIPTION : activates
```

---

## Key Model Attributes & Constraints

| Entity | Primary Keys & Indexes | Key Attributes | Invariant / Purpose |
|---|---|---|---|
| **`User`** | `id` (UUID), `email` (Unique) | `role`, `fullName`, `languagePref`, `district`, `targetExam` | Multi-role user model (`STUDENT`, `ADMIN`) with district demographic tracking. |
| **`AdminSession`** | `id` (UUID), `tokenHash` (Unique) | `userId`, `ipAddress`, `userAgent`, `expiresAt`, `revokedAt` | Stateful administrative session storing only SHA-256 hash of presented token. |
| **`Question`** | `id` (UUID) | `questionText`, `questionTextTa`, `optionA`–`D`, `optionATa`–`DTa`, `correctOption`, `section`, `unit`, `topic`, `difficulty` | Bilingual question entity. live exam APIs strictly sanitize `correctOption` and explanations. |
| **`MockTest`** | `id` (UUID), `slug` (Unique) | `title`, `totalQuestions` (200), `durationMinutes` (180), `totalMarks` (300.0), `isPremium`, `isPublished` | Curated 200-question mock test metadata matching official TNPSC blueprint. |
| **`MockAttempt`** | `id` (UUID) | `userId`, `mockTestId`, `status`, `score`, `tamilScore`, `aptitudeScore`, `accuracyPercentage`, `timeSpentSeconds` | Student exam attempt state machine (`IN_PROGRESS`, `SUBMITTED`, `ABANDONED`). |
| **`MockAnswer`** | `id` (UUID), Unique(`mockAttemptId`, `questionId`) | `selectedOption`, `isMarkedForReview`, `isVisited`, `timeSpentSeconds` | Granular response tracking per question for OMR palette and telemetry. |
| **`MistakeBook`**| `id` (UUID), Unique(`userId`, `questionId`) | `wrongCount`, `consecutiveCorrect`, `isMastered`, `masteredAt` | Personal mistake tracking with spaced-repetition mastery loop. |
| **`Payment`** | `id` (UUID), `receiptNumber` (Unique) | `userId`, `planId`, `gateway`, `amountInr`, `status`, `utrReference` | Dual-gateway payment records (`RAZORPAY`, `CASHFREE`, `MANUAL_UPI`). |
| **`Subscription`**| `id` (UUID) | `userId`, `planId`, `startDate`, `endDate`, `status` | Active premium access pass governing mock exam and analytics entitlements. |
