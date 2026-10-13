# 📊 Diagram: System Architecture

High-level component layout and interaction channels across TargetTNgrp4 client applications, edge security layers, domain services, and persistence infrastructure.

```mermaid
flowchart TB
    subgraph Clients["Frontend Presentation Applications"]
        STUDENT["🌐 Student Portal Web App<br/>(Next.js 16 App Router)"]
        EXAM_RUNNER["⏱️ Full Mock Exam Runner<br/>(OMR Palette, 180m Timer, Autosave)"]
        ADMIN_PORTAL["🛡️ Admin Management Console<br/>(TETS Question Bank, Approvals)"]
        STUDY_ROOM["👥 Live Aspirant Study Room<br/>(Topic Tests & Community Leaderboard)"]
    end

    subgraph EdgeTier["Edge & Security Layer"]
        CF["Cloudflare Edge<br/>(Global CDN, SSL, DDoS Mitigation)"]
        PROXY["Next.js 16 Proxy Gate<br/>(src/proxy.ts - Route & Session Guard)"]
    end

    subgraph BackendCore["TargetTNgrp4 Core Domain Services"]
        direction TB
        AUTH_STU["Student JWT Session Guard<br/>(targettn_student_session)"]
        AUTH_ADM["Admin DB Session & CSRF Guard<br/>(admin_sessions + origin check)"]
        BLUEPRINT["Blueprint 200Q Sampler Service<br/>(Part A: 100, Part B: 75, Part C: 25)"]
        EVAL_CORE["Exam Evaluation & Scoring Engine<br/>(Zero Answer Leakage, 1.5 marks/Q)"]
        PRACTICE["Topic Practice & Daily Target Engine<br/>(Section > Unit > Topic)"]
        MISTAKE["Mistake Book Spaced Repetition<br/>(Auto wrong logging & mastery)"]
        CSV_ENGINE["PapaParse CSV & Zod Ingestion Pipeline<br/>(Bulk question streaming)"]
        PAYMENT["Dual Payment Service<br/>(Razorpay HMAC Webhooks + Direct UPI)"]
    end

    subgraph Persistence["Storage & Database Infrastructure"]
        DB[("PostgreSQL Database<br/>(Prisma 5.19 with relationJoins)")]
        POOLER["Connection Pooler<br/>(High-throughput pooled connection)"]
    end

    subgraph External["External Gateways"]
        RAZORPAY["Razorpay Payment Gateway<br/>(Order creation & HMAC Webhook)"]
    end

    Clients --> CF
    CF --> PROXY
    PROXY --> BackendCore

    AUTH_STU -.-> EVAL_CORE
    AUTH_ADM -.-> CSV_ENGINE
    
    BLUEPRINT --> POOLER
    EVAL_CORE --> POOLER
    PRACTICE --> POOLER
    MISTAKE --> POOLER
    CSV_ENGINE --> POOLER
    PAYMENT --> POOLER
    POOLER --> DB

    PAYMENT <--> RAZORPAY
```
