# System Architecture

## Architectural style

The platform uses WordPress as the application shell, but the domain model is largely custom.

This is important: the app does not mainly rely on posts, pages, and standard CMS patterns to represent the business. Instead, it uses dedicated database tables and purpose-built PHP modules to model projects, documents, attendance, and submissions directly.

That gives the system much more control over workflow rules and operational data integrity.

## Main architecture layers

### 1. Presentation layer

The front-end experience is delivered through custom theme PHP files, page templates, shortcodes, and jQuery/AJAX interactions.

The app exposes custom screens such as:

- manager dashboard,
- approval dashboard,
- project submission form,
- document center,
- attendance clock,
- admin panel,
- and the project atlas.

Most of these are not generic WordPress pages. They are app screens rendered inside WordPress.

### 2. Application layer

Core business logic lives in custom PHP modules inside `wp-content/themes/oceanwp`.

Key modules include:

- `wp-content/themes/oceanwp/functions.php`
- `wp-content/themes/oceanwp/page-manager-dashboard.php`
- `wp-content/themes/oceanwp/mhr-project-submission.php`
- `wp-content/themes/oceanwp/approval-dashboard.php`
- `wp-content/themes/oceanwp/mhr-document-center.php`
- `wp-content/themes/oceanwp/mhr-document-block-submission.php`
- `wp-content/themes/oceanwp/mhr-document-notification.php`
- `wp-content/themes/oceanwp/mhr-terms-acceptance.php`
- `wp-content/themes/oceanwp/mhr-attendance-shortcode.php`
- `wp-content/themes/oceanwp/mhr-attendance-helpers.php`
- `wp-content/themes/oceanwp/mhr-projects-map.php`
- `wp-content/themes/oceanwp/admin-panel.php`
- `wp-content/themes/oceanwp/mhr-pdf-export.php`

### 3. Data layer

The app is heavily table-driven.

Major business data sits in custom MySQL tables such as:

- `mhr_projects`
- `mhr_monter_profiles`
- `mhr_doc_requirements`
- `mhr_monter_documents`
- `mhr_monter_documents_multi`
- `mhr_documents_archive`
- `mhr_attendance`
- `mhr_attendance_edit_log`
- `mhr_attendance_manual`
- `mhr_notification_log`
- `mhr_regulamin_updates`


## Shortcode and screen map

The app uses WordPress shortcodes as delivery points for application screens.

| Shortcode | Primary purpose |
|-----------|-----------------|
| `[project_submission]` | Worker-facing work submission form |
| `[approval_dashboard]` | Supervisor and manager review queue |
| `[mhr_document_center]` | Worker document management workspace |
| `[mhr_document_accept]` | Admin-side document approval area |
| `[mhr_notification_popup]` | In-app compliance and status popup |
| `[mhr_notification_static]` | Inline notification banner |
| `[mhr_clock]` | Clock in/out widget |
| `[mhr_attendance]` | Attendance review interface |
| `[mhr_projects_map]` | Atlas and map-based operations screen |

### 4. Interaction layer

The platform relies heavily on AJAX through `admin-ajax.php` for business actions such as:

- retrieving floorplans,
- saving submission markers,
- approving or rejecting work,
- uploading and approving documents,
- secure file downloads,
- clock-in and clock-out actions,
- terms acceptance,
- atlas data retrieval,
- and contact/profile updates.

This lets the app behave more like a live operations tool than a page-refresh-driven site.

### 5. Integration layer

Verified integrations and platform dependencies include:

- Google Maps API for the atlas and geocoding features,
- mPDF for export generation,
- WordPress roles, auth, and admin facilities,
- Ultimate Member-related account status patterns,
- environment variables loaded from `.env-mhr`.

## Request and action pattern

Most state-changing actions follow the same shape:

1. a user opens a shortcode-driven or template-driven screen,
2. the screen loads scoped business data,
3. a jQuery/AJAX action posts to `admin-ajax.php`,
4. the backend applies nonce, auth, and role checks,
5. the app sanitizes inputs and writes to custom tables,
6. the result appears immediately in downstream views.

That pattern is what gives the product its app-like feel inside WordPress.

## System coupling: why it is strong

The app is most effective at the connection points between modules.

Examples:

- Projects store assigned workers and supervisors.
- Attendance uses those assignments to capture who is working where.
- The document system tracks whether workers are compliant.
- The submission system can block work reporting if required documents are missing.
- The atlas combines project location, activity, and workforce visibility.


## Key technical files and what they do

### `wp-content/themes/oceanwp/functions.php`

Acts as the custom application bootstrap. It loads environment values, registers AJAX hooks and shortcodes, wires custom modules together, and contains utility logic such as encryption and secure downloads.

### `wp-content/themes/oceanwp/page-manager-dashboard.php`

Handles project CRUD, worker/supervisor assignment, floorplan management, instruction uploads, status control, and the `require_documents` business toggle.

### `wp-content/themes/oceanwp/mhr-project-submission.php`

Provides the work-report submission interface. It checks whether the worker is allowed to submit, loads assigned projects, fetches project-specific supervisors, and records field evidence.

### `wp-content/themes/oceanwp/approval-dashboard.php`

Provides the supervisor/manager review queue for pending submissions with role-scoped visibility.

### `wp-content/themes/oceanwp/mhr-document-center.php`

Implements the compliance document hub: profile bootstrap, company type selection, upload workflow, approval pipeline, secure downloads, and notification preferences.

### `wp-content/themes/oceanwp/mhr-attendance-shortcode.php`

Exposes the clock widget used by workers and supervisors for attendance tracking.

### `wp-content/themes/oceanwp/mhr-projects-map.php`

Renders the map-based operational atlas with project visualization and project-side drilldown.

