# Project Roadmap: Enhancing Activities API

This roadmap translates the REMS comparison into actionable epics & features for the Mergington High School Activities API.

## Legend
| Label | Meaning |
|-------|---------|
| `epic` | Large initiative grouping features |
| `feature` | User-facing capability |
| `task` | Implementation or support work |

## Phase 1 – Foundations
### Epic: Persistence & Data Layer
- Feature: Introduce SQLAlchemy models (Activity, Student, Enrollment)
- Feature: Add Alembic migrations & DB config via env vars
- Feature: Replace in-memory dict with DB repository abstraction

### Epic: Authentication & Roles
- Feature: User accounts (students, admins)
- Feature: JWT or session-based auth
- Feature: Role checks for admin endpoints

## Phase 2 – Activity Management Expansion
### Epic: Dynamic Activities & Registration
- Feature: Admin CRUD for activities (create/update/delete)
- Feature: Participant capacity enforcement at DB level
- Feature: CSV export of registrations

### Epic: Team Registrations (Optional)
- Feature: Team entity (name, members)
- Feature: Endpoint: create team & register to activity

## Phase 3 – Communication & Certificates
### Epic: Certificate Generation
- Feature: Upload template + CSV -> generate PDFs/images
- Feature: Public certificate retrieval endpoint

### Epic: Bulk Email
- Feature: Persist mailing lists
- Feature: Send bulk HTML emails for announcements

## Phase 4 – Observability & Admin UX
### Epic: Logging & Audit
- Feature: Structured logging (request + domain events)
- Feature: Audit table storing actions

### Epic: Admin Dashboard
- Feature: Stats endpoint (activities count, registrations, capacity usage)
- Feature: Broadcast alert (store + expose via endpoint)

## Phase 5 – Enhancements
### Epic: Link Shortening
- Feature: Integrate external API for short activity links

### Epic: UI & Theming
- Feature: Basic frontend (React or server-rendered) for browsing
- Feature: Dark mode toggle

### Epic: Security Hardening
- Feature: Rate limiting
- Feature: Input sanitization & validation layer

## Backlog / Future Considerations
- Analytics endpoints (trends of signups per week)
- Role granularity (advisor, coach)
- Internationalization (i18n) for UI strings
- Queue-based email sending (Celery / RQ)

## Suggested Order of Execution
1. Persistence
2. Auth & Roles
3. Dynamic Activities
4. Logging & Audit
5. Certificates
6. Bulk Email
7. Dashboard
8. Export / Teams
9. Security & Theming
10. Optional integrations (Shortener, Analytics)

## Success Metrics
| Area | Metric |
|------|--------|
| Reliability | <200ms median GET /activities |
| Adoption | % of activities created via admin UI vs code seed |
| Engagement | Avg registrations per activity |
| Quality | Test coverage > 70% |

## Contribution Guidelines (Preview)
- Each feature gets a GitHub issue using the Feature template.
- Link tasks to parent feature via issue references.
- Epics tracked by updating child issue checklist.

_Last updated: 2025-12-02_
