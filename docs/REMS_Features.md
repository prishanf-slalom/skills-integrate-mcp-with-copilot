# REMS (Resources and Event Management System)

Comprehensive Python/Flask-based toolkit for clubs, student organisations, NGOs, and small communities to automate event handling, certificate distribution, mailing, registrations, and admin workflows.

---
## 1. Architecture Overview
- **Stack:** Python 3.x, Flask, SQLAlchemy, Pydantic, Loguru, Alembic, Jinja2, Bootstrap 4, jQuery.
- **Databases:** Multiple binds (main, forms, mail) – supports MariaDB/MySQL & SQLite.
- **Containerisation:** Dockerfile + entrypoint script (runs Alembic migrations then starts app).
- **Modularity:** Each feature isolated in a Flask Blueprint under `routes/`.
- **Validation:** Pydantic schemas for form & CSV integrity.
- **Templating:** Jinja2 templates under `templates/`; reusable navigation & footer partials.
- **Logging:** Centralised Loguru configuration; f-string logging style.

### Repository Layout (Condensed)
```
src/app/__init__.py      # App factory & blueprint registration
src/app/models.py        # SQLAlchemy models (users, events, certificates, logs)
src/app/schemas.py       # Pydantic validation models
src/app/routes/          # Feature blueprints (auth, forms, mailing, certificates, etc.)
src/app/utils/           # Helpers (auth, email, logger, pagination, sql introspection)
src/app/templates/       # Jinja2 HTML templates
src/app/static/          # CSS, JS, fonts, images
migrations/              # Alembic migration scripts
Dockerfile               # Container build
```

---
## 2. Core Feature Set

### 2.1 Certificate Generation & Distribution (CGDS)
| Aspect | Details |
|--------|---------|
| Purpose | Bulk generate participant certificates from template + CSV |
| Tech | Pillow for image composition; CSV parsing via Pydantic schema |
| Workflow | Admin uploads header template + participant data → system renders all certificates |
| Access | Public searchable portal for participants to retrieve/download |
| Storage | Metadata + generation events tracked in DB |
| Customisation | Supports variable text placement & dynamic fields |

### 2.2 Dynamic Event Registration Form Generator
| Aspect | Details |
|--------|---------|
| Creation | Admin builds forms selecting field set + team size + description (Markdown capable) |
| Field Options | Reg. No, Department, Year, Email, Phone, plus optional custom additions |
| Team Support | Individual or teams (1–5 participants configurable) |
| Persistence | Auto-creates dedicated table per event in `forms` DB bind |
| Access | Generated public URL per event for distribution |
| Data Export | Admin can view + export responses as CSV |
| Validation | Pydantic ensures sanitised identifiers & required fields |

### 2.3 Bulk Mailer System
| Aspect | Details |
|--------|---------|
| Capability | Send HTML email campaigns to uploaded recipient lists |
| Mailing Lists | Upload CSV → persist to mail DB bind |
| Template | Customisable HTML layout per send |
| Batch Handling | Iterates over all recipients; handles failure segregation |
| Use Cases | Event promotion, certificate distribution notices, general announcements |

### 2.4 Response Viewing & Export
| Aspect | Details |
|--------|---------|
| UI | Paginated table of submissions per event |
| Export | One-click CSV download |
| Filtering | Server-side pagination & optional search hooks |
| Compliance | Clean separation of form data per event table |

### 2.5 Database Management Interface
| Aspect | Details |
|--------|---------|
| Scope | CRUD across all bound databases (main/forms/mail) via web GUI |
| Operations | Browse tables, inspect columns, insert/update/delete rows |
| Error Handling | Graceful error pages (e.g., db failure, invalid mutation) |
| Safety | Identifier sanitation and role restrictions (admin only) |

### 2.6 Link Shortener
| Aspect | Details |
|--------|---------|
| Integration | External Short.io API |
| Slugs | Auto-generated or custom user-defined |
| UX | Quick copy button, error display on failure |
| Use | Compact distribution of registration/certificate links |

### 2.7 User & Auth Management
| Aspect | Details |
|--------|---------|
| Roles | Admin vs Member (RBAC checks like `is_admin`) |
| Login Flow | Standard password auth + session management |
| Password Reset | Email-based recovery flow |
| Profile | Avatar / profile details update route |
| Protection | `login_required` decorator wraps protected endpoints |

### 2.8 Activity Logging System
| Aspect | Details |
|--------|---------|
| Logged Entities | User actions (creation, mail send, form generation, certificate runs) |
| View | Paginated activity log screen |
| Purpose | Auditing & traceability |

### 2.9 Member Dashboard & Alerts
| Aspect | Details |
|--------|---------|
| Metrics | Event count, user metrics, quick-start info |
| Alert Broadcasting | Admin chooses severity (info/success/warning); non-admin defaults to info |
| Delivery | Immediate UI broadcast; username attribution captured |

### 2.10 Dark Mode Support
| Aspect | Details |
|--------|---------|
| Toggle | Checkbox-backed theme switcher (CSS class adjustments) |
| Persistence | Client-side (likely cookie/local storage pattern) |
| Accessibility | Reduces eye strain for night usage |

---
## 3. Supporting Utilities & Modules
| Module | Function |
|--------|----------|
| `utils/auth.py` | Authentication decorators & helpers |
| `utils/email.py` | Email assembly + sending utilities |
| `utils/helpers.py` | Admin checks, identifier sanitation, general helpers |
| `utils/pagination.py` | Generic pagination abstraction |
| `utils/sql.py` | Cross-bind introspection (list tables/columns) |
| `utils/logger.py` | Loguru setup – consistent structured logging |

---
## 4. Security & Reliability Considerations
- **Input Sanitisation:** Event table names scrubbed to prevent injection (`sanitize_identifier`).
- **ORM Safety:** SQLAlchemy parameter binding reduces raw query risk.
- **Role Enforcement:** Admin-only pages guarded by privilege check prior to mutation actions.
- **Error Pages:** Custom templates for 404 / DB errors provide graceful degradation.
- **Session Security:** Flask `SECRET_KEY` (must be rotated in production).

---
## 5. Deployment & Ops
| Element | Notes |
|---------|-------|
| Docker | `python:3.13-slim` base; installs deps; runs entrypoint script |
| Environment | `.env` loaded via `dotenv` for DB URIs |
| Migrations | Alembic auto-run before app start |
| Logs | Structured Loguru output; helps container log collection |

---
## 6. UX & Frontend Details
- **Framework:** Bootstrap 4 + Font Awesome Icons.
- **Interactive JS:** jQuery for small enhancements (auto-hiding alerts, clipboard copy, dynamic selects).
- **Responsive Behaviour:** User-agent detection logic in navigation/dashboard for layout tweaks.
- **Reusable Partials:** Navigation & footer included across pages.

---
## 7. Typical Workflow Examples
### Event Setup & Registration
1. Admin opens Form Generator → chooses fields & team size.
2. System creates dedicated table & exposes registration URL.
3. Participants submit form; data stored row-wise.
4. Admin views responses & exports CSV if needed.

### Certificate Distribution
1. Admin uploads participant CSV + template assets.
2. REMS generates certificate images (Pillow).
3. Public certificate portal allows retrieval by participant details.

### Bulk Mailing
1. Admin uploads/update mailing CSV list.
2. Composes HTML email in Bulk Mailer.
3. Sends campaign; logs activity & tracks send status.

### Link Shortening
1. Admin/member enters long URL.
2. Chooses custom slug or accepts generated.
3. Receives short link, copies, distributes.

---
## 8. Strengths & Usage Scenarios
| Scenario | Benefit |
|----------|---------|
| Semester-long club operations | Centralised forms, certificates & communication |
| Hackathons / competitions | Rapid team registration & bulk result mailing |
| Certification workshops | Automated certificate generation & retrieval |
| Outreach campaigns | Managed mailing lists & template-driven emails |

---
## 9. Improvement Opportunities (Future Roadmap Ideas)
| Area | Potential Enhancement |
|------|-----------------------|
| Access Control | Granular role tiers (e.g., event managers) |
| Mailing | Queued/asynchronous sending with retry/back-off |
| Analytics | Dashboard charts for registration trends & mail engagement |
| Certificates | WYSIWYG template editor & drag/drop positioning |
| Auditing | Exportable audit log with filters |
| Internationalisation | Multilingual template + locale-aware date formatting |

---
## 10. Quick Reference Summary
| Feature | Status |
|---------|--------|
| Form Generation | Stable |
| Certificate System | Stable |
| Bulk Mailer | Stable |
| DB Manager | Admin-only stable |
| Link Shortener | External API reliant |
| Logging | Active & paginated |
| Dark Mode | UX enhancement |
| Role System | Basic (Admin/Member) |

---
_Last updated: 2025-12-02_
