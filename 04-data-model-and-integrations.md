# Data Model And Integrations

## Data model approach

The platform is database-first in its business design.

That means the important business entities are stored in dedicated tables instead of being forced into post types or loosely structured CMS content. This is a strong architectural choice for an operations system because it makes workflows easier to control and query.

## Schema summary at a glance

| Table | Role in the system |
|-------|--------------------|
| `mhr_projects` | project registry, assignment model, and map anchor |
| `mhr_project_submissions` | field work reports and approval state |
| `mhr_monter_profiles` | worker identity and communication preferences |
| `mhr_doc_requirements` | document rules engine |
| `mhr_monter_documents` | primary document records |
| `mhr_monter_documents_multi` | multi-file document records |
| `mhr_documents_archive` | historical document preservation |
| `mhr_attendance` | operational time-tracking records |
| `mhr_attendance_edit_log` | attendance correction trail |
| `mhr_attendance_manual` | manual attendance interventions |
| `mhr_notification_log` | notification tracking and evidence |
| `mhr_regulamin_updates` | terms and policy acknowledgment history |

## Core business tables

The following tables carry the primary operational state of the platform.

## `mhr_projects`

Stores the project catalog and assignment structure.

Important fields include:

- project name,
- client,
- location,
- assigned worker IDs,
- assigned supervisor IDs,
- hourly rate,
- document requirement flag,
- floorplans,
- instruction file,
- map visibility,
- latitude/longitude,
- geocoded address.

This table is the operational backbone of the platform.

It is especially important because it anchors multiple downstream behaviors:

- who can work where,
- who can supervise whom,
- whether document compliance is required,
- what appears on the map,
- and which floorplans or instructions belong to the job.

## `mhr_monter_profiles`

Stores each operational user's profile state.

Important fields include:

- WordPress user mapping,
- operational ID,
- company type,
- encrypted mobile and WhatsApp values,
- notification preferences,
- terms acceptance status.

This table is a bridge between identity, compliance, and communication.

## `mhr_doc_requirements`

Defines which documents are required for each company type.

Important fields include:

- `company_type`,
- `doc_key`,
- `doc_label`,
- `field_type`,
- expiration behavior,
- renewal frequency,
- warning days.

This table is what turns documents into a rules engine instead of a loose upload folder.

## `mhr_monter_documents`

Stores primary document records.

Important fields include:

- owner identifiers,
- document type,
- file URLs,
- text values,
- expiration date,
- status,
- approval state,
- approver and approval date,
- rejection reason,
- first approved tracking.

This table matters because it supports more than storage. It supports document state, approval history, and time-based risk management.

## `mhr_monter_documents_multi`

Supports multi-file document cases such as "other documents" style uploads.

## `mhr_documents_archive`

Preserves historical document versions when newer files replace earlier approved ones.

That is a meaningful governance feature because it avoids losing document history.

## `mhr_attendance`

Stores time records with unusually rich operational context.

Important fields include:

- worker identity snapshot,
- project and supervisor snapshot,
- date,
- clock-in and clock-out,
- total hours,
- entry method,
- edit metadata,
- original values,
- lock flags,
- anomaly flags,
- admin verification,
- notes.

This is not a basic presence table. It is structured for oversight and auditability.

## `mhr_attendance_edit_log`

Tracks field-level edits to attendance records. This is valuable because it gives the system an audit-style trail for corrections.

## `mhr_attendance_manual`

Supports manual attendance adjustments when required.

## `mhr_regulamin_updates`

Stores policy update events and supports forced re-acceptance of terms.

## Data design patterns worth highlighting

- Project assignments are stored as JSON arrays for workers and supervisors.
- Operational records often store snapshot values, not just foreign keys, which protects historical context.
- Profiles and documents are separated cleanly, allowing approval and notification logic to evolve independently.
- Attendance stores anomaly and verification state directly, which is useful for management review.

## Relationship summary

```text
projects -> assignments -> workers and supervisors
projects -> submissions
projects -> attendance
profiles -> document requirements by company type
profiles -> document ownership and notification preferences
documents -> archive history
attendance -> edit log
terms updates -> forced re-acceptance behavior
```

## WordPress-specific signals still used well

Although the business model is custom-table-driven, the app still uses WordPress effectively for:

- authentication,
- roles and capabilities,
- user meta,
- shortcode-driven screen composition,
- AJAX routing,
- media/upload foundations,
- and admin-compatible tooling.

Important user meta and identity signals include:

- `account_status`
- `mhr_default_project`
- `mhr_default_kierownik`
- `worker_id`
- role/capability metadata

## Verified integrations

## Google Maps

The project atlas and map-related project controls rely on a Google Maps API key loaded from `.env-mhr`.

## mPDF

The app includes mPDF and uses it for PDF exports of confirmed work data.

## Notification structure

The platform supports multi-channel notifications including email, SMS, WhatsApp, and in-app popups. Notification preferences are stored per worker profile and a notification log table tracks delivery history. Document expiry warnings trigger through this layer.

