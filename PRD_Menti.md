# Live Audience Interaction Platform — PRD

Version: 0.1 (Draft)
Date: 2025-09-01
Owner: Product

## Overview

- **Product**: Live audience interaction platform for polls, Q&A, and quizzes.
- **Goal**: Increase engagement and gather real‑time feedback in meetings, classes, and events.
- **Primary Users**: Presenters, Participants, Moderators, Org Admins.
- **Platforms**: Web (presenter + participant), mobile‑web (PWA); optional native later.
- **Delivery**: MVP in 8–12 weeks; phased enterprise features after.

## Objectives

- **Engagement**: ≥70% participant interaction in active sessions.
- **Speed**: <200 ms median vote propagation; <1 s slide switch.
- **Scale**: 5k concurrent participants per session (MVP), 20k+ later.
- **Reliability**: 99.9% monthly uptime SLO (MVP), 99.95% later.
- **Adoption**: Time‑to‑first‑poll <3 minutes for new presenters.

## Personas

- **Presenter**: Creates sessions, runs activities, views results.
- **Participant**: Joins via code/QR, votes, submits questions.
- **Moderator**: Reviews/approves questions, manages content.
- **Org Admin**: Manages users, SSO, security, billing, analytics.

## Assumptions & Scope Boundaries

- In-scope (MVP): Synchronous sessions with presenter-controlled activities; lightweight moderation; CSV/PNG exports.
- Out-of-scope (MVP): Asynchronous surveys across days, full LMS gradebook sync, SOC 2 audit deliverables, on-prem deployment.
- Constraints: Mobile-web first for participants; presenter runs on desktop web; no native apps in MVP.

## Technical Architecture & Infrastructure

### System Architecture Overview
The platform follows a microservices architecture with the following key components:

**Frontend Layer:**
- **Presenter Dashboard**: React/Next.js application for session creation and management
- **Participant Interface**: Progressive Web App (PWA) optimized for mobile devices
- **Projector View**: Full-screen display optimized for presentations
- **Admin Panel**: Organization management and analytics dashboard

**API Gateway Layer:**
- **Authentication Service**: Handles user authentication, SSO, and session management
- **Session Service**: Manages live sessions, activities, and real-time data
- **Analytics Service**: Processes engagement metrics and generates reports
- **Export Service**: Handles data exports and file generation

**Real-time Layer:**
- **WebSocket Gateway**: Manages real-time connections and message routing
- **Redis Pub/Sub**: Handles message broadcasting and session state
- **Event Stream**: Processes and distributes real-time events

**Data Layer:**
- **PostgreSQL**: Primary database for user data, sessions, and analytics
- **Redis**: Caching and real-time session state
- **S3/Cloud Storage**: File storage for exports and media assets

### Enterprise Technology Alternatives

**Frontend Framework Alternatives:**
- **Primary**: React 18+ with TypeScript
- **Alternatives**: 
  - Vue.js 3 (enterprise-friendly, smaller bundle size)
  - Angular 17+ (enterprise standard, built-in TypeScript)
  - Vanilla JavaScript with Web Components (no external dependencies)
  - Svelte 5 (compiled framework, minimal runtime)

**Build Tools & Bundlers:**
- **Primary**: Webpack 5 or Vite 5
- **Alternatives**:
  - Rollup (ES modules, tree-shaking)
  - Parcel (zero-config, enterprise-friendly)
  - esbuild (Go-based, extremely fast)
  - SWC (Rust-based, Babel alternative)

**State Management:**
- **Primary**: Redux Toolkit or Zustand
- **Alternatives**:
  - Context API + useReducer (built-in React)
  - Jotai (atomic state, lightweight)
  - Valtio (proxy-based, simple API)
  - Custom event-driven state management

**Real-time Communication Alternatives:**
- **Primary**: WebSocket with Socket.io
- **Alternatives**:
  - Server-Sent Events (SSE) - built-in browser support
  - Long Polling with XMLHttpRequest or fetch
  - WebRTC for peer-to-peer communication
  - Custom HTTP streaming with chunked responses

**Database Alternatives:**
- **Primary**: PostgreSQL 15+
- **Alternatives**:
  - **Relational**: MySQL 8.0, SQL Server, Oracle Database
  - **NoSQL**: MongoDB, CouchDB, Apache Cassandra
  - **In-Memory**: Redis, Memcached, Hazelcast
  - **Embedded**: SQLite, H2 Database, Apache Derby

**Caching Alternatives:**
- **Primary**: Redis 7+
- **Alternatives**:
  - **In-Memory**: Node.js Map/WeakMap, custom LRU cache
  - **Database**: PostgreSQL with UNLOGGED tables
  - **File System**: Local file cache with TTL
  - **Distributed**: Hazelcast, Apache Ignite, EhCache

**Authentication Alternatives:**
- **Primary**: JWT with bcrypt
- **Alternatives**:
  - **Session-based**: Express-session with database storage
  - **Token-based**: Custom token system with database validation
  - **OAuth**: Built-in OAuth 2.0 implementation
  - **Enterprise**: SAML 2.0, LDAP integration

**Monitoring & Observability Alternatives:**
- **Primary**: New Relic, DataDog
- **Alternatives**:
  - **Open Source**: Prometheus + Grafana, Jaeger, ELK Stack
  - **Built-in**: Node.js performance hooks, custom metrics
  - **Enterprise**: Splunk, IBM APM, Microsoft Application Insights
  - **Cloud Native**: AWS CloudWatch, Azure Monitor, Google Cloud Monitoring

### Enterprise Security & Compliance Alternatives

**Encryption Libraries:**
- **Primary**: Node.js crypto module (built-in)
- **Alternatives**:
  - **JavaScript**: CryptoJS, SJCL (Stanford JavaScript Crypto Library)
  - **Web Crypto API**: Built-in browser crypto functions
  - **Custom**: Implementation using native Node.js crypto
  - **Enterprise**: Hardware Security Modules (HSM) integration

**Input Validation & Sanitization:**
- **Primary**: Joi, Yup, or Zod
- **Alternatives**:
  - **Built-in**: Custom validation functions with regex
  - **DOM**: DOMPurify for XSS prevention
  - **Custom**: Input sanitization using native string methods
  - **Enterprise**: OWASP validation libraries

**Rate Limiting Alternatives:**
- **Primary**: Express-rate-limit, Redis-based
- **Alternatives**:
  - **In-Memory**: Custom rate limiting with Map/WeakMap
  - **Database**: Rate limiting using database counters
  - **Built-in**: Custom implementation with timestamps
  - **Enterprise**: API Gateway rate limiting (Kong, AWS API Gateway)

### Deployment & Infrastructure Alternatives

**Container Alternatives:**
- **Primary**: Docker with Kubernetes
- **Alternatives**:
  - **Virtualization**: VMware, Hyper-V, VirtualBox
  - **Bare Metal**: Direct server deployment
  - **Cloud Native**: AWS Lambda, Azure Functions, Google Cloud Functions
  - **Enterprise**: OpenShift, Rancher, VMware Tanzu

**CI/CD Alternatives:**
- **Primary**: GitHub Actions, GitLab CI
- **Alternatives**:
  - **Jenkins**: Self-hosted, enterprise standard
  - **Azure DevOps**: Microsoft enterprise solution
  - **Bamboo**: Atlassian enterprise solution
  - **Custom**: Shell scripts, PowerShell, Python automation

**Load Balancing Alternatives:**
- **Primary**: Nginx, HAProxy
- **Alternatives**:
  - **Built-in**: Node.js cluster module
  - **Reverse Proxy**: Apache httpd, IIS
  - **Cloud Native**: AWS ALB, Azure Load Balancer
  - **Enterprise**: F5 BIG-IP, Citrix NetScaler

### Fallback Solutions for Restricted Environments

**No External Libraries Approach:**
```javascript
// Custom state management without external libraries
class EventEmitter {
    constructor() {
        this.events = {};
    }
    
    on(event, callback) {
        if (!this.events[event]) {
            this.events[event] = [];
        }
        this.events[event].push(callback);
    }
    
    emit(event, data) {
        if (this.events[event]) {
            this.events[event].forEach(callback => callback(data));
        }
    }
}

// Custom caching without Redis
class LRUCache {
    constructor(maxSize = 1000) {
        this.maxSize = maxSize;
        this.cache = new Map();
    }
    
    get(key) {
        if (this.cache.has(key)) {
            const value = this.cache.get(key);
            this.cache.delete(key);
            this.cache.set(key, value);
            return value;
        }
        return null;
    }
    
    set(key, value) {
        if (this.cache.has(key)) {
            this.cache.delete(key);
        } else if (this.cache.size >= this.maxSize) {
            const firstKey = this.cache.keys().next().value;
            this.cache.delete(firstKey);
        }
        this.cache.set(key, value);
    }
}
```

**Database Abstraction Layer:**
```typescript
// Database abstraction for multiple database types
interface DatabaseAdapter {
    connect(): Promise<void>;
    query(sql: string, params?: any[]): Promise<any>;
    close(): Promise<void>;
}

class PostgreSQLAdapter implements DatabaseAdapter {
    // PostgreSQL implementation
}

class MySQLAdapter implements DatabaseAdapter {
    // MySQL implementation
}

class SQLiteAdapter implements DatabaseAdapter {
    // SQLite implementation
}

class DatabaseFactory {
    static createAdapter(type: string): DatabaseAdapter {
        switch (type) {
            case 'postgresql': return new PostgreSQLAdapter();
            case 'mysql': return new MySQLAdapter();
            case 'sqlite': return new SQLiteAdapter();
            default: throw new Error(`Unsupported database: ${type}`);
        }
    }
}
```

**Real-time Communication Fallbacks:**
```typescript
// Fallback communication strategy
class CommunicationManager {
    private websocket: WebSocket | null = null;
    private eventSource: EventSource | null = null;
    private pollingInterval: NodeJS.Timeout | null = null;
    
    connect(sessionId: string) {
        try {
            // Try WebSocket first
            this.websocket = new WebSocket(`wss://api.example.com/ws/${sessionId}`);
            this.setupWebSocket();
        } catch (error) {
            try {
                // Fallback to Server-Sent Events
                this.eventSource = new EventSource(`/api/events/${sessionId}`);
                this.setupEventSource();
            } catch (error) {
                // Fallback to long polling
                this.setupPolling(sessionId);
            }
        }
    }
    
    private setupPolling(sessionId: string) {
        this.pollingInterval = setInterval(async () => {
            try {
                const response = await fetch(`/api/poll/${sessionId}`);
                const data = await response.json();
                this.handleMessage(data);
            } catch (error) {
                console.error('Polling failed:', error);
            }
        }, 2000); // Poll every 2 seconds
    }
}
```

### Enterprise Integration Alternatives

**SSO Integration:**
- **SAML 2.0**: Built-in implementation or enterprise SSO providers
- **OAuth 2.0**: Custom implementation with enterprise identity providers
- **LDAP/Active Directory**: Direct integration without external libraries
- **Custom**: Enterprise-specific authentication protocols

**File Storage Alternatives:**
- **Primary**: AWS S3, Azure Blob Storage
- **Alternatives**:
  - **Local Storage**: File system with backup strategies
  - **Network Storage**: NFS, SMB, enterprise storage solutions
  - **Database**: BLOB storage in database
  - **Enterprise**: SharePoint, OneDrive, Box integration

**Email Service Alternatives:**
- **Primary**: SendGrid, Mailgun
- **Alternatives**:
  - **SMTP**: Direct SMTP server integration
  - **Exchange**: Microsoft Exchange integration
  - **Custom**: Enterprise email gateway integration
  - **Built-in**: Node.js nodemailer with enterprise SMTP

### Performance Optimization Alternatives

**Image Processing:**
- **Primary**: Sharp, Jimp
- **Alternatives**:
  - **Browser**: Canvas API for client-side processing
  - **Server**: ImageMagick command-line tools
  - **Custom**: Native image processing algorithms
  - **Enterprise**: Enterprise image processing services

**PDF Generation:**
- **Primary**: Puppeteer, jsPDF
- **Alternatives**:
  - **Server**: wkhtmltopdf, PhantomJS
  - **Browser**: Canvas-based PDF generation
  - **Custom**: HTML to PDF conversion
  - **Enterprise**: Enterprise PDF services

**Data Export Alternatives:**
- **Primary**: ExcelJS, csv-writer
- **Alternatives**:
  - **Built-in**: Custom CSV/JSON generation
  - **Server**: LibreOffice, OpenOffice integration
  - **Browser**: Client-side file generation
  - **Enterprise**: Enterprise reporting tools

### Compliance & Security Alternatives

**Data Anonymization:**
- **Primary**: Custom implementation using crypto libraries
- **Alternatives**:
  - **Built-in**: Node.js crypto module
  - **Custom**: Hash-based anonymization
  - **Enterprise**: Enterprise data masking tools
  - **Database**: Database-level anonymization

**Audit Logging:**
- **Primary**: Custom audit system
- **Alternatives**:
  - **Database**: Audit tables with triggers
  - **File System**: Structured log files
  - **Custom**: Event-driven audit system
  - **Enterprise**: Enterprise logging solutions

**Backup & Recovery:**
- **Primary**: Custom backup scripts
- **Alternatives**:
  - **Database**: Native database backup tools
  - **File System**: rsync, robocopy, custom scripts
  - **Cloud**: Enterprise cloud backup services
  - **Enterprise**: Enterprise backup solutions

## Functional Requirements

Note: Each item includes an ID and priority [MVP | P2 | P3]. Acceptance tests expand on high-level criteria below.

### Sessions & Access Control

- FR-001 [MVP]: Create, edit, duplicate, archive, and delete sessions; cloning retains activities and settings.
- FR-002 [MVP]: Session states: Draft, Live, Paused, Ended, Archived; transitions allowed: Draft→Live, Live↔Paused, Live→Ended, Ended→Archived.
- FR-003 [MVP]: Join via 6–8 char code, link, or QR; vanity link support [P2].
- FR-004 [MVP]: Access modes per session: Public (open link), Code-only, Passcode, Org-restricted [P2]; optional CAPTCHA [P2].
- FR-005 [MVP]: Presenter lock/unlock participation per activity; lock entire session; hard stop ends submissions.
- FR-006 [P2]: Multi-presenter support: add/remove co-presenters and moderators with live control handover.
- FR-007 [MVP]: Device limit: one active vote device per participant; rejoin restores identity within session.
- FR-008 [P2]: Schedule sessions (start time reminders); presenter pre-warm lobby and waiting screen.

### Activities & Question Types

- FR-020 [MVP]: Multiple Choice (single/multiple select), 2–10 options; optional images per option [P2].
- FR-021 [MVP]: Word Cloud with dedupe, stemming (en), stop-words list; max 3 words per submission configurable.
- FR-022 [MVP]: Open Text (short/long); character limits; optional approval required.
- FR-023 [MVP]: Rating/Scale (1–5 or 1–10); configurable labels.
- FR-024 [P2]: Quiz mode with correct answers, timers, scoring, leaderboards; streaks and fastest-answer tie-breaker [P3].
- FR-025 [P2]: Advanced: Ranking, Matrix (Likert), NPS, Image choice, Slider, Multiple text entries.
- FR-026 [MVP]: Activity settings: time limit, max responses per participant, show/hide results, allow change of answer.
- FR-027 [MVP]: Content library: save activities as templates; import/export activity sets (JSON/CSV import) [P2].

### Presentation & Display

- FR-040 [MVP]: Presenter view with controls: start/stop activity, next/prev, show/hide results, show instructions, timer.
- FR-041 [MVP]: Projector view: full-screen, auto-fit, high-contrast theme; watermark toggle by plan.
- FR-042 [MVP]: Charts: bars, donuts [P2], animated updates; display absolute counts and percentages.
- FR-043 [MVP]: Word cloud layout with collision handling; profanity masking; color themes.
- FR-044 [P2]: On-slide reactions/emojis; confetti for milestones; sound cues.
- FR-045 [P2]: Remote control via mobile presenter panel; keyboard shortcuts cheat-sheet.

### Participation & Identity

- FR-060 [MVP]: Join as Anonymous or Named; name length validation; optional required name/role.
- FR-061 [MVP]: Reconnect handling: preserve submissions after network drop; prevent duplicate counting.
- FR-062 [MVP]: Language selection per participant; UI strings localized (EN MVP; others P2).
- FR-063 [P2]: Org directory lookup for authenticated users; avatar display; custom profile fields.

### Q&A & Moderation

- FR-080 [MVP]: Participants submit questions; upvote/downvote [MVP upvote only]; sort by Top/Recent.
- FR-081 [MVP]: Presenter/moderator can approve, hide, pin, archive; undo last action.
- FR-082 [MVP]: Profanity and flagged-term auto-hold; spam throttling (rate-per-user).
- FR-083 [P2]: Merge/mark-duplicate questions; edit for clarity with edit history [P3].
- FR-084 [MVP]: Display selected question on projector; track displayed duration.

### Templates, Libraries & Imports

- FR-100 [MVP]: Starter templates for classes, all-hands, workshops.
- FR-101 [P2]: Import from CSV (MCQ, ratings) with mapping UI; export/import JSON for sessions.
- FR-102 [P2]: Organization shared library with browse, search, permissions.

### Collaboration, Roles & Permissions

- FR-120 [MVP]: Roles: Org Admin, Member, Guest; Session roles: Owner, Co-presenter [P2], Moderator.
- FR-121 [MVP]: Permissions matrix: who can create, present, moderate, export, delete (see matrix below).
- FR-122 [P2]: Team spaces with shared sessions; role-based access per space.

### Branding & Theming

- FR-140 [MVP]: Org-level logo/colors; session theme selection (light/dark/high-contrast).
- FR-141 [P2]: Custom fonts, CSS variables per org; per-session background images with accessibility checks.
- FR-142 [P2]: Custom subdomain; remove vendor branding (Enterprise).

### Integrations & Embeds

- FR-160 [P2]: Google Slides/PowerPoint add-ins: insert live activity, control playback.
- FR-161 [P2]: Zoom/Teams/Webex apps: run activities within meeting side panel.
- FR-162 [MVP]: Embeddable widget via iframe/script; embed tokens scoped to session.
- FR-163 [P2]: Slack/Teams bots to share join link and post results.
- FR-164 [P2]: LMS (LTI 1.3) launch and roster fetch; basic gradeback for quizzes [P3].

### Reporting, Exports & Data Management

- FR-180 [MVP]: Exports: CSV per activity/session; PNG snapshots; include timestamps and (optional) participant name.
- FR-181 [P2]: PDF session report with charts; PPT export of slides.
- FR-182 [MVP]: Delete session permanently with 7-day soft-delete window [P2]; audit event logged.
- FR-183 [MVP]: Data retention settings per org (30–365 days); auto-expire and notify owner.
- FR-184 [P2]: Org analytics dashboard (engagement, participation, usage by team).
- FR-185 [MVP]: GDPR tools: export user data, anonymize/delete on request (admin UI).

### Notifications

- FR-200 [P2]: Email summary to presenter after session (participation counts, top Qs).
- FR-201 [P2]: Real-time mod notifications (flagged terms, spike alerts) via web and email.

### Failure Modes & Offline

- FR-220 [MVP]: Realtime fallback to SSE; banner indicates degraded mode; submissions queued and retried.
- FR-221 [MVP]: Presenter safe mode: freeze results, allow advancing slides without updates.

## Roles & Permissions Matrix (Summary)

- Org Admin: manage org settings, billing, users, data retention, audit exports.
- Member: create/present sessions, invite moderators, export own sessions.
- Guest: present if assigned, moderate if invited, no org settings access.
- Session Owner: full control; Co-presenter: start/stop, show/hide, edit activities [P2]; Moderator: approve/hide Q&A, block users.

### Permissions Table

| Capability | Org Admin | Member | Guest | Session Owner | Co-presenter (P2) | Moderator |
|---|---:|---:|---:|---:|---:|---:|
| Create session | ✓ | ✓ | ✗ | ✓ | ✓ | ✗ |
| Edit session settings | ✓ | ✓ | ✗ | ✓ | ✓ | ✗ |
| Present (start/stop session) | ✓ | ✓ | ✓ (if assigned) | ✓ | ✓ | ✗ |
| Manage activities (add/edit/reorder) | ✓ | ✓ | ✗ | ✓ | ✓ | ✗ |
| Lock/unlock participation | ✓ | ✓ | ✗ | ✓ | ✓ | ✗ |
| Moderate Q&A (approve/hide/pin) | ✓ | ✓ | ✗ | ✓ | ✓ | ✓ |
| Invite co-presenters/moderators | ✓ | ✓ | ✗ | ✓ | ✗ | ✗ |
| Export data (CSV/PNG) | ✓ | ✓ (own) | ✗ | ✓ | ✓ | ✗ |
| Manage org settings & billing | ✓ | ✗ | ✗ | ✗ | ✗ | ✗ |
| Delete/archive session | ✓ | ✓ (own) | ✗ | ✓ | ✗ | ✗ |

## Session Lifecycle & Behavior

- States: Draft (editable), Live (collecting), Paused (no submissions, projector banner), Ended (read-only results), Archived (hidden from lists).
- Live controls: lock/unlock activity, reset activity (with confirm), reveal correct answers [P2].
- Waiting room: optional lobby with instructions and countdown; reveal first activity on start.
- Post-session: thank-you screen with summary and link to feedback [P2].

## APIs & Webhooks (High-Level)

- REST: sessions (CRUD, duplicate, archive), activities (CRUD, reorder), responses (list/export), qna (list/moderate).
- Auth: OAuth2 for API tokens; session-scoped embed tokens.
- Webhooks: session.started, session.ended, activity.started, activity.ended, response.created, question.created, question.moderated, export.ready.
- Rate limits per org and per session to protect realtime pipeline.

## Traceability Matrix (FR → Acceptance Criteria)

Note: MVP focus. Each FR maps to concise acceptance checks; detailed test cases go in QA test plans.

| FR ID | Acceptance Criteria (summary) |
|---|---|
| FR-001 | Create/edit/duplicate/archive/delete session via UI and API; duplicate retains activities/settings; archived sessions hidden from default list; permission checks enforced. |
| FR-002 | Session shows valid state; allowed transitions work; UI reflects Paused banner; Ended prevents new submissions; Archived read-only. |
| FR-003 | Join works via link/code/QR; invalid code error; code format 6–8 alphanum; QR encodes join URL; join in ≤3 steps on mobile/desktop. |
| FR-004 | Access modes: Public, Code-only, Passcode; entering wrong passcode blocks access; Org-restricted out-of-scope for MVP (graceful message). |
| FR-005 | Presenter can lock/unlock activity and whole session; locked state blocks new submissions; unlocking resumes without data loss. |
| FR-007 | One active device per participant enforced; rejoin after refresh preserves identity; second device prompts to switch. |
| FR-020 | MCQ create with 2–10 options; single/multi-select; votes counted once per participant; results chart updates <200 ms median. |
| FR-021 | Word cloud dedupes case/tense; stop-words applied; limit per participant enforced; profanity masked per list. |
| FR-022 | Open text respects character limit; optional approval gate works; approved items appear in projector view. |
| FR-023 | Rating/Scale configurable range (1–5/1–10); labels rendered; aggregation correct (avg, count). |
| FR-026 | Time limit starts/stops correctly; max responses per participant enforced; allow-change toggle respected; show/hide results toggle works. |
| FR-027 | Save activity to personal library; apply template creates identical activity; org library out-of-scope (P2). |
| FR-040 | Presenter controls: start/stop, next/prev, show/hide results, show instructions, timer; keyboard shortcuts for next/prev. |
| FR-041 | Projector full-screen and high-contrast themes; safe area rendering at 1080p and 4k; watermark toggle by plan. |
| FR-042 | Bar chart renders counts and percentages; donut chart flagged as P2; animations do not drop frames at 5k votes. |
| FR-043 | Word cloud layout avoids overlaps; colors follow theme; profanity masking visible. |
| FR-060 | Join as Anonymous or Named; name required toggle respected; validation on length/emoji; name visible to presenter if enabled. |
| FR-061 | Temporary network loss shows reconnecting; on reconnect no duplicate submissions; progress preserved. |
| FR-062 | Participant can select language (EN MVP); UI strings localized; persists across reload within session. |
| FR-080 | Submit question; upvote increments; sort by Top/Recent; duplicate upvotes prevented per participant. |
| FR-081 | Approve/hide/pin/archive actions update views in <500 ms; undo reverts last moderation action. |
| FR-082 | Flagged words auto-hold; spam throttling caps submissions per minute; mod queue counter visible. |
| FR-084 | Selected question displays on projector; duration tracked; unpin restores list view. |
| FR-100 | Create session from template; template library shows predefined sets; applying template preserves ordering. |
| FR-120 | Roles available: Owner, Moderator; role assignment UI; permissions enforced per table. |
| FR-121 | Permissions matrix enforced in API and UI; unauthorized actions blocked; audit event logged. |
| FR-140 | Org logo/colors configurable; theme applied to projector and participant views; contrast passes WCAG AA for text. |
| FR-162 | Embeddable widget renders activity; embed token scoped to session; same-origin and X-Frame-Options configured appropriately. |
| FR-180 | CSV export includes timestamps, participant (if named), question text, counts; PNG snapshot matches projector view. |
| FR-182 | Delete moves to soft-delete if enabled (P2); MVP delete immediate with confirm; audit event logged. |
| FR-183 | Data retention setting configurable; auto-expire job removes data after threshold; email notice sent to owner (configurable). |
| FR-185 | Admin can export or anonymize a user’s data; processing completes and logs audit trail. |
| FR-220 | WebSocket fallback to SSE on failure; banner indicates degraded mode; submissions queued and retried. |
| FR-221 | Presenter safe mode freezes results while allowing navigation; participants see read-only banner. |

## 90-Day Delivery Plan & Backlog

Assumptions: 6–8 engineers (3 FE, 3 BE, 1–2 full-stack), 1 designer, 1 QA; weekly releases; feature flags for risky items; target 5k concurrent participants per session by Day 75.

### Milestones & Workstreams

- Weeks 0–2: Foundations (Infra + Core CRUD)
  - Auth, orgs, users; sessions CRUD (FR-001), states (FR-002), join (FR-003), access modes (FR-004 basics), device limit (FR-007).
  - Data model, Postgres/Redis; WebSocket gateway + SSE fallback scaffolding (FR-220).
  - Presenter/participant skeleton UIs; QR/link generation.

- Weeks 3–6: Activities + Presentation MVP
  - MCQ (FR-020), Word Cloud (FR-021), Open Text (FR-022), Rating (FR-023), activity settings (FR-026).
  - Presenter controls (FR-040), Projector view (FR-041), charts (FR-042), word cloud display (FR-043).
  - Templates basic (FR-027), branding basics (FR-140), lock/unlock (FR-005).
  - Q&A core: submit/upvote/sort (FR-080), moderation controls (FR-081), anti-spam (FR-082), display (FR-084).

- Weeks 7–10: Hardening + Data + Embeds
  - Exports CSV/PNG (FR-180); retention (FR-183); GDPR tools (FR-185); delete behavior (FR-182).
  - Embeddable widget + tokens (FR-162).
  - Participation: identity, reconnection (FR-060/061), localization EN (FR-062).
  - Accessibility pass (WCAG AA essentials), observability, rate limits.

- Weeks 11–12: Scale, QA, Beta Readiness
  - Load tests to 5k; perf tuning (p95/p99); failover tests (FR-220/221).
  - Security review, threat model, DPA template; support runbook; documentation.
  - Beta cohort onboarding; collect feedback; bug triage and stabilization.

### Deliverables by Day 90

- Live MVP covering all [MVP] FRs in Traceability Matrix.
- Beta with 10–20 customers; 99.9% SLO; exports operational; basic branding.

### Backlog (Prioritized P2 → P3)

- P2-High
  - FR-006 Co-presenters & live handover.
  - FR-044 Reactions/confetti; FR-045 remote control.
  - FR-101 CSV import; FR-102 org library.
  - FR-181 PDF/PPT exports.
  - FR-184 Org analytics dashboards.
  - FR-200/201 Notifications (post-session summaries; mod alerts).

- P2-Normal
  - FR-008 Scheduling & lobby.
  - FR-024 Quiz mode and leaderboards.
  - FR-141/142 Advanced theming and white-label.
  - FR-160/161 Slides & meeting apps; FR-163 Slack/Teams bots.
  - FR-164 LMS (LTI 1.3) launch.
  - FR-083 Merge/mark-duplicate Qs.

- P3 (Future Enhancements)
  - Advanced quiz features (streaks, fastest tie-breakers), image choice, sliders.
  - Edits with history for Q&A, AI dedupe/summarization; data residency regions.

### Risks & Mitigations (Execution)

- Realtime scale spikes → Shard by session, backpressure, autoscale; pre-warm for known events.
- Abuse/spam → Rate limits, risk-based CAPTCHA, moderation tools.
- Adoption friction → Templates, onboarding, embed widget; quickstart guides.

## Service Level Objectives (SLOs) & SLIs

- Vote propagation latency: P50 ≤ 150 ms, P95 ≤ 300 ms, P99 ≤ 600 ms.
- Slide switch latency (presenter→projector): P50 ≤ 300 ms, P95 ≤ 800 ms.
- Q&A moderation action latency: P95 ≤ 500 ms.
- Reconnect success: ≥ 98% within 30 seconds after network disruption.
- Error rate: < 0.5% of realtime messages dropped/failed (per session).
- Availability: 99.9% monthly SLO (MVP) with 43.8 min/month error budget.
- Projector rendering: ≥ 55 FPS on reference hardware (1080p); no jank > 16 ms for > 99% frames during bursts.
- Backend performance: P95 API latency ≤ 250 ms (read) / 400 ms (write); P99 ≤ 800 ms.
- Error budget policy: If monthly error budget is exhausted, freeze risky launches, prioritize reliability work until burn rate normalizes.

## Disaster Recovery & Business Continuity (DR/BCP)

- RPO ≤ 5 minutes (Postgres WAL + streaming replicas; Redis AOF with min fsync every second).
- RTO ≤ 30 minutes for primary region failure; Phase 2 cross-region warm standby with automated failover.
- Backups: Nightly full DB snapshots; 15‑minute incremental; 30‑day retention; backups encrypted (AES‑256) with key rotation.
- Restore drills: Quarterly full restore rehearsal; document RTO/RPO achieved; remediate gaps.
- Storage durability: S3 exports stored with versioning + lifecycle policies; server-side encryption enabled.
- Dependency failure modes: Degrade to read-only projector when Redis/pub-sub unavailable; queue submissions locally up to 60 seconds.
- Runbook: Step-by-step failover + rollback; DNS/traffic switching; comms templates for customers.

## Security & Privacy Program

- Authentication & sessions: OAuth2/OIDC for APIs; short‑lived access tokens (≤ 60 min) with refresh; session cookies `HttpOnly`, `SameSite=Lax`; CSRF tokens for form/REST.
- Authorization: Least privilege; role/permission checks at API and UI; deny by default; audit on privilege grants.
- Data protection: TLS 1.2+ in transit; AES‑256 at rest; field‑level encryption candidates: participant names/emails; KMS-managed keys; rotation ≥ yearly.
- Secret management: Centralized vault; no secrets in code; rotation playbooks and quarterly audits.
- Abuse prevention: 64‑bit entropy join codes; HMAC’d join tokens bound to session; per‑IP/agent rate limits; anomaly CAPTCHA; replay protection.
- Input safety: Profanity lists per locale; XSS/HTML sanitization for text; image uploads type/size limits; content security policy (CSP) strict.
- Vulnerability management: SAST/Dependency scanning on CI; monthly 3rd‑party scan; patch critical within 7 days, high within 30.
- Privacy & compliance: GDPR/CCPA; DPA + subprocessor list; Records of Processing; DPIA for data flows; DSR SLA ≤ 30 days; cookie consent (ePrivacy) and preferences.
- Sector posture: Not HIPAA; COPPA/FERPA reviewed—education usage guidance with parental consent requirements.
- Audit logging: Login events, session access, moderation actions, exports, data changes; 1‑year retention; tamper‑evident hashing.

## API & Webhook Standards

- Specs: OpenAPI 3.1 for REST; AsyncAPI for realtime events; published in developer portal.
- Versioning: URI prefix (e.g., `/v1`); 12‑month deprecation policy; changelog with migration guidance.
- Auth: OAuth2 client credentials and user tokens; scopes per resource; rate limits by org and token.
- Pagination: Opaque cursors (`next`, `prev`); max page size; stable sort.
- Idempotency: `Idempotency-Key` required for submissions/mutations; at‑least‑once processing with safe dedupe.
- Errors: Standard envelope `{code, message, details, traceId}`; use RFC7807 `problem+json`.
- Timeouts/retries: Server timeouts ≤ 30s; clients retry idempotent 5xx with exp backoff and jitter.
- Webhooks: HMAC‑SHA256 signing; retries up to 24h with backoff; 2xx considered success; user-configurable endpoints with delivery logs; replay prevention via timestamps and nonces; dead‑letter queue after final failure.

## Billing & Entitlements

- Plans: Free, Pro, Enterprise with explicit feature flags and limits (participants/session, sessions/org, exports, branding, co‑presenters, embeds, APIs).
- Enforcement: UI gating + server-side checks; graceful degradation and upsell messaging; 429/403 with hint headers on limit breaches.
- Commerce flows: Trials (14/30 days), proration on upgrades; downgrade at renewal with warnings; seat management with overage handling; cancellation with grace period and data retention choice.
- Payments & tax: Stripe (cards/invoices), tax/VAT collection, receipts, dunning for failed payments; invoicing for Enterprise.
- Entitlements API: Read-only endpoint for client to determine capabilities; cached with short TTL; event `entitlement.updated` on changes.
- Data model: Plan, seats, add‑ons, limits; per‑org usage counters with reset cadence.

## Observability & Operations

- Metrics: SLIs (latency P50/P95/P99, error rate, reconnect success, dropped messages), infra (CPU/mem, GC, queue depth), business (active sessions, participants, votes/second).
- Tracing: Distributed tracing with trace IDs propagated over WebSocket (via connection metadata) and HTTP; sampling strategy documented.
- Logging: Structured JSON; PII redaction; correlation IDs; retention 30–90 days depending on tier.
- Dashboards & alerts: Thresholds aligned with SLOs; paging for SLO burn rate, error spikes, queue saturation; non‑paging for trends.
- Deployments: CI/CD with automated tests; feature flags; blue/green or canary; 1‑click rollback; infra as code; config via environment with secrets from vault.
- Rate limiting: Per‑org/session/user/IP caps; `429` with `Retry-After`; headers `X-RateLimit-Limit/Remaining/Reset`.
- Incident management & status page: Sev definitions (SEV1–SEV4), on‑call rotation, comms templates, status page with uptime and incident history; postmortems within 5 business days for SEV1/2.

## Testing & Load Strategy

- Test pyramid: unit, integration (API + DB), contract (WS), e2e (presenter/participant flows), accessibility tests.
- Load targets: Spike 2k rps per session for 60 seconds; sustained 500 rps for 30 minutes; 5k concurrent participants with ≤ 0.5% error rate.
- Chaos drills: WebSocket disconnect/reconnect every 5 minutes; Redis failover; DB read replica promotion test; network latency injection.
- Soak tests: 2‑hour runs with rolling reconnects; memory leak detection.
- Accessibility audits: Axe automated + manual with NVDA/JAWS/VoiceOver; keyboard-only runs.
- Environments: Dev/staging/prod parity; seed data sets; synthetic canary sessions in prod for continuous verification.

## Accessibility Criteria & Audit Plan

- WCAG 2.1 AA: Contrast ratios, focus indicators, error messaging, form labels.
- ARIA live regions: Announce chart updates and timer changes without stealing focus.
- Keyboard: All presenter controls and participant actions operable by keyboard; visible focus order; shortcuts documented.
- Screen readers: Meaningful headings, labels, alt text; skip links; projector conveys information non‑color‑dependently.
- Motion & preferences: Reduced motion mode honors OS setting; de‑animate charts.
- Localization: Language attributes and per‑locale typography; RTL support plan; profanity/stopword lists per locale.
- Audit cadence: Pre‑beta full audit; regression checks each release impacting UI components.


## MVP Scope

- **Session Setup**: Create/edit/delete sessions; share via link/QR; access code.
- **Activities**: Multiple choice, word cloud, open text, rating (1–5/1–10).
- **Q&A**: Submit questions, upvote, presenter pin/hide; basic moderation.
- **Presentation Mode**: Live results, timers, themes; projector‑friendly view.
- **Participation**: Anon or named; one device‑one vote; optional re‑vote lock.
- **Export**: CSV export per session; PNG snapshot of results.
- **Branding**: Basic logo + colors at org level.
- **Realtime**: WebSockets with SSE fallback; optimistic UI for votes.
- **Onboarding**: Templates for common use cases; quickstart tour.

## Post‑MVP (Phase 2)

- **Quizzes**: Correct answers, points, leaderboards.
- **Slides Integration**: PowerPoint/Google Slides add‑in, slide control.
- **Advanced Activities**: Ranking, matrix, NPS, image choice, scales.
- **Moderation**: Profanity filter, spam throttling, flagged terms, bulk actions.
- **Analytics**: Session/Org dashboards, engagement funnels, exports.
- **APIs**: REST + webhooks; Zapier/Make; Slack/Teams bots.
- **Enterprise**: SSO (SAML/OIDC), SCIM, data residency, audit logs.
- **Accessibility**: WCAG 2.1 AA certification; full keyboard/screen reader.
- **AI Aids**: Question dedupe, summarization, suggested themes/questions.

## User Flows

- **Presenter Flow**: Create session → Add activities → Start session → Switch activities → Moderate Q&A → Export results.
- **Participant Flow**: Open link/QR → Enter name/anon → Submit votes/answers → React/ask questions → View results.
- **Moderator Flow**: Join as mod → Approve/deny/merge questions → Pin/archive → Block/report users.

## Acceptance Criteria (MVP)

- **Join**: Participant joins in ≤3 steps via link/QR; works on iOS/Android/desktop.
- **Poll**: Create MCQ with 2–10 options; ≥5k concurrent votes; update chart in <200 ms median.
- **Word Cloud**: Deduplicate words; safe‑list/ban‑list; limit per participant.
- **Q&A**: Submit/query in <500 ms; upvote; presenter can show/hide/pin live.
- **Moderation**: Toggle pre‑approval; auto‑block flagged words; undo last action.
- **Export**: CSV includes timestamps, user (if named), question text, counts.

## Non‑Functional Requirements

- **Performance**: P95 interaction <500 ms; cold start <2 s on presenter view.
- **Availability**: Graceful degradation to read‑only when realtime fails.
- **Security**: TLS 1.2+; AES‑256 at rest; rate limiting; per‑session isolation.
- **Privacy**: PII minimization; data retention policy configurable (30–365 days).
- **Accessibility**: Contrast, focus states, ARIA roles, captions for timers.

## Security & Compliance

- **Auth**: Email/password + magic link; SSO (Phase 2).
- **Roles**: Org Admin, Member, Guest; session‑level Presenter/Moderator.
- **Compliance**: GDPR and CCPA (MVP), SOC 2 Type II (later); DPA template.
- **Audit**: Login events, session access, moderation actions (Phase 2).

## Integrations

- **Slides**: PowerPoint add‑in, Google Slides add‑on (Phase 2).
- **Meetings**: Zoom/Teams/Webex apps; share‑screen optimized (Phase 2).
- **Comms/Automation**: Slack/Teams notifications; Zapier/webhooks for exports.
- **LMS**: LTI 1.3 for Canvas/Moodle/Blackboard (later).

## Data Model (High‑Level)

- **Organization**: id, name, branding, plan, settings.
- **User**: id, email, role, org_id, name, SSO mapping.
- **Session**: id, org_id, code, title, status, settings, created_by.
- **Activity**: id, session_id, type, config, order.
- **Response**: id, activity_id, participant_id/hash, payload, ts.
- **Question**: id, session_id, text, votes, status, ts.

## Architecture (High‑Level)

- **Frontend**: React/Next.js; PWA; SSR for presenter; CDN caching.
- **Realtime**: WebSocket gateway; Redis pub/sub; backpressure protection.
- **Backend**: Node/Go services; REST + WebSocket; rate‑limited endpoints.
- **Storage**: Postgres for core data; Redis for sessions; S3 for exports.
- **Observability**: Structured logs, metrics (latency, fan‑out), tracing, alerts.

## Admin & Billing

- **Plans**: Free (limits on participants/features), Pro, Enterprise.
- **Controls**: Domain capture, seat management, limits per plan.
- **Branding**: Custom themes, subdomain; remove vendor logo (Enterprise).
- **Billing**: Stripe with invoicing; usage‑based overages (optional).

## Internationalization

- **Languages**: EN MVP; add ES/FR/DE/JP after.
- **Timezones**: Per user; timestamps localized.
- **Content**: RTL support later.

## Browser/Mobile Support

- **Desktop**: Latest Chrome/Edge/Firefox/Safari; fallback to SSE.
- **Mobile**: iOS 15+, Android 9+; PWA install prompt; offline page.

## Success Metrics

- **Activation**: % new presenters who run a live poll in 7 days.
- **Engagement**: Median participants per session; interactions per participant.
- **Performance**: P95 realtime latency; error rate; reconnect rate.
- **Retention**: 4‑week presenter retention; session repeat rate.
- **Quality**: CSAT ≥4.5/5; support tickets per 1k sessions.

## Risks & Mitigations

- **Scale Spikes**: Use autoscaling + per‑session shard; pre‑warm for large events.
- **Abuse/Spam**: Rate limits, CAPTCHA risk‑based, moderation tools.
- **Network Variability**: Retry + offline banners; SSE fallback; local queueing.
- **Adoption Friction**: Templates, import from CSV, Slides add‑ons.

## Rollout Plan

- **Alpha**: Internal dogfood with classrooms/meetings (≤200 participants).
- **Beta**: Selected customers; add SSO, exports; collect feedback.
- **GA**: Pricing, billing, support runbook, status page.

## Open Questions

- **Scale target**: Typical vs peak participants per session?
- **Compliance**: Must‑have for GA (GDPR only vs SOC2)? Data residency needs?
- **Integrations**: Which are top priority: Slides, Zoom/Teams, LMS, Slack?
- **Branding**: White‑label needs? Custom domains?
- **Monetization**: Seat‑based, MAU‑based, or event‑based pricing?
