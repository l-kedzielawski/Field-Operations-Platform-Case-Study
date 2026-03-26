# User Roles and Workflows

## Why this file matters

This platform is built around operational roles. Different users see different tools, make different decisions, and carry different responsibilities inside the workflow.

## Primary user groups

### Workers / technicians (`monter`)

These users do the field work.

They typically:

- accept terms,
- complete their profile,
- upload required documents,
- clock in and out,
- submit realizations,
- and review the status of their own submissions.

### Supervisors (`kierownik`)

These users oversee project execution.

They typically:

- review field submissions,
- comment on work,
- approve or reject submissions,
- appear as selectable supervisors for attendance and submissions,
- and are tied to projects through assignment data.

### Admin / management roles

Admin-like roles include variants such as `admin` or `manager` 

These users typically:

- approve or deactivate accounts,
- manage projects,
- review and approve documents,
- oversee attendance,
- view broader analytics and atlas visibility,
- and reset terms acceptance after policy updates.

## Simple access matrix

| Capability | Worker | Supervisor | Admin / management |
|------------|--------|------------|--------------------|
| Submit work reports | own only | no | no |
| Review submissions | no | assigned projects | all projects |
| Upload documents | own only | no | no |
| Approve documents | no | no | all users |
| Clock in and out | own only | no | no |
| Review attendance | no | limited operational visibility | full visibility |
| Manage projects | no | limited project context | yes |
| Manage accounts | no | no | yes |
| Access admin controls | no | limited workflow views | full access |
| View operational atlas | limited relevance | assigned-project visibility | full command view |

## Role model signals

The system also uses operational login conventions such as:

- `M...` for workers,
- `K...` for supervisors,
- `A...` and `X...` for other account types.

That naming convention is more than cosmetic. It helps the application distinguish operational actor types in queries and workflows.

## Core workflow 1: onboarding and access control

1. A user account is created.
2. The app expects a worker-style or supervisor-style operational identity.
3. The user must accept terms.
4. A profile row is created in `mhr_monter_profiles` if missing.
5. Admin can approve or deactivate the account.

Why this matters: access is governed as part of operational onboarding, not treated like simple website registration.

## Core workflow 2: document setup and compliance

1. The user opens the document center.
2. The system ensures a profile exists.
3. The user selects company type, especially `PL` or `DE`.
4. The system loads the correct required document set.
5. The user uploads files or enters text values where appropriate.
6. Documents enter an approval state such as `pending`, `approved`, or `rejected`.
7. Expiration logic and warning logic affect status over time.

This is a key business workflow because the document layer directly affects whether workers can continue operating in the app.

## Core workflow 3: project setup and assignment

1. A manager creates or updates a project.
2. The project stores client, location, assigned workers, assigned supervisors, hourly rate, and commentary.
3. Optional floorplans and instruction files are attached.
4. The `require_documents` flag determines whether document compliance becomes a hard gate for submissions.
5. Geocoding and map visibility settings support the atlas.

This makes the project record the anchor for multiple downstream workflows.

## Core workflow 4: work submission and approval

1. A worker opens the submission form.
2. The app checks whether the worker is assigned to a document-required project.
3. If required documents are missing, expired, rejected, or still pending approval, submission is blocked.
4. If the worker is allowed to proceed, they select a project.
5. The app loads project-specific supervisor choices.
6. The worker adds submission data, photos, and floorplan or location evidence.
7. The submission enters a pending state such as `DO POTWIERDZENIA` `to be approved`.
8. A supervisor or manager reviews it in the approval dashboard.
9. The reviewer can add comments, approve, reject, or otherwise resolve the item.

This was the main starting workflow in the whole platform because it combines assignment logic, compliance logic, field evidence capture, and managerial review.

## Core workflow 5: attendance

1. The user opens the attendance widget.
2. The system loads assigned projects and supervisors.
3. The user clocks in against a project and supervisor.
4. Default selections are remembered through user meta.
5. The user clocks out later.
6. The record stores hours, timestamps, and status.
7. Anomaly flags can mark suspicious entries such as very long durations or missing checkout.
8. Admin-side views and logs support oversight and correction.

## Core workflow 6: operational atlas

1. Projects are geocoded and optionally made visible on the map.
2. The atlas loads project data, location, assignments, and related operational details.
3. Management can use the map as a command view rather than browsing separate admin pages.

