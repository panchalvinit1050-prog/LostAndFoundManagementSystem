
## 1. Overview of the application

**Lost & Found Management System** is a full‑stack web application that helps users **report lost items**, **report found items**, and **match** them to increase recovery success. It provides a controlled workflow so that users can **securely claim** recovered belongings and administrators can **moderate** and manage reports.

### 1.1 Goals

- Provide a **single place** to register lost/found reports with consistent metadata (category, location, time, description, images).
- Support **search and matching** between lost and found items (automatic + manual review).
- Enable a **secure claim process** to reduce fraud.
- Give admins the tools to **moderate, verify, and close** cases.

### 1.2 Primary users (roles)

- **Guest / Visitor**: browse public items (optional, depends on configuration).
- **Registered User**: create and manage lost/found posts; initiate claims.
- **Admin / Moderator**: review reports, approve/deny claims, manage categories/users.

---

## 1) Functional and Non‑functional requirements

### 1.3 Functional requirements

#### A. Authentication & user management
- Users can **register, login, logout**.
- Passwords are stored securely (hashed).
- Role-based access control:
  - Users can manage their own reports.
  - Admins can manage all reports and moderate content.

#### B. Lost item reporting
- Users can create a **Lost Item** report with:
  - title / item name
  - category
  - description
  - last seen date/time
  - last seen location
  - optional images
  - contact preferences (if supported)
- Users can edit or delete their own lost item report (subject to status rules).

#### C. Found item reporting
- Users can create a **Found Item** report with:
  - title / item name
  - category
  - description
  - found date/time
  - found location
  - optional images
- Users can edit or delete their own found item report (subject to status rules).

#### D. Search and filtering
- Users can browse and search lost/found listings.
- Filters may include:
  - category
  - date range
  - location (text-based; optionally geo-based)
  - status (open/claimed/closed)
- Sort options (e.g., newest first).

#### E. Matching (lost ↔ found)
- System should generate **suggested matches** based on shared attributes (e.g., category, keywords, location proximity, time proximity).
- Users can view suggested matches for their report.
- Admin can manually link/unlink matches (if implemented).

#### F. Claim workflow (secure recovery)
- A user can initiate a **claim request** for a found item.
- Claim request includes **verification details** (e.g., proof of ownership questions, unique identifiers, description details not public).
- Owner/admin can **accept or reject** the claim.
- System records claim status history for auditability.

#### G. Notifications (optional but recommended)
- Users receive notifications for:
  - match suggestions
  - claim requests
  - claim approvals/denials
  - report status changes
- Notification channels may include in-app, email (if configured).

#### H. Administration & moderation
- Admin can:
  - view all reports and claims
  - change status of reports (open/closed/resolved)
  - manage categories (add/edit/disable)
  - manage users (lock/disable, role changes)
  - remove inappropriate content/images

#### I. Audit and logs (recommended)
- Store important actions (report created/updated, claim initiated, status changed) for debugging and compliance.

---

### 1.4 Non-functional requirements

#### A. Security
- Use **JWT/session-based authentication** with proper expiration and refresh strategy (as implemented).
- Enforce **RBAC** at API level (not only UI).
- Input validation on backend for all endpoints.
- Protect against common web threats:
  - SQL injection (via ORM/parameterized queries)
  - XSS (sanitize user-generated content if rendered)
  - CSRF (if using cookies)
- Secure file upload handling (type/size validation) if images are supported.

#### B. Performance
- List/search endpoints should respond quickly under expected load.
- Use pagination for listing endpoints.
- Index frequently queried fields (category, status, created_at, location fields).

#### C. Availability & reliability
- Application should handle failures gracefully:
  - clear error messages
  - retries/timeouts for external dependencies (if any)
- Use structured logging for troubleshooting.

#### D. Scalability
- System should scale horizontally:
  - stateless backend services (preferred)
  - externalized session/token management as needed
- Database design should support growing numbers of reports and claims.

#### E. Maintainability
- Clean separation of layers (Controller → Service → Repository).
- Consistent API conventions, validation, and error format.
- Automated tests (unit + integration) for core flows:
  - create report
  - search/match
  - claim approval flow

#### F. Usability & accessibility
- UI should provide:
  - clear navigation between Lost / Found / Matches / Claims
  - form validation and helpful hints
- Follow basic accessibility practices (labels, keyboard navigation).

#### G. Observability
- Logs with correlation/request IDs (recommended).
- Health endpoint for backend (recommended).
- Metrics/tracing (optional, but useful in production).

## User Flow Diagram
![User Flow](./diagrams/DomainContext.svg)

## Database ER Diagram
![ER Diagram](./diagrams/er.drawio.svg)

## OpenAPI Spec
- [openapi.yaml](./openapi/openapi.yaml)
