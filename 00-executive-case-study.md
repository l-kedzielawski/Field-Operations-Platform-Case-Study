# Executive Case Study

## In one paragraph

As a solo developer, I turned a fragmented field operation into a governed internal platform used by a 200+ person workforce across Poland and Germany. What had been handled through chat messages, spreadsheets, phone calls, inbox attachments, and manual follow-up became one working system for field reporting, document compliance, document-state monitoring, expiry tracking, attendance review, approvals, security-conscious file handling, and operational visibility.

## The situation before the build

The business did not have one clean operational workflow. Work updates came in through chats and calls. Worker documents were spread across inboxes, attachments, local folders, and ad hoc message threads. Finding a single required document could take 3 to 5 people and become an urgent coordination task in itself.

There was no reliable central view of which records were pending review, approved, rejected, expiring soon, or already expired. That created both operational friction and security risk, because sensitive documents were harder to control, monitor, and retrieve safely. Attendance depended too much on memory and later cleanup. Managers could get the information eventually, but only after chasing it.

### That meant four real problems:

- reporting was inconsistent,
- approvals were slow,
- document retrieval was inefficient, sensitive records were harder to control, and compliance issues were often discovered too late because document status and expiry were hard to monitor,
- and leadership did not have a live picture of what was happening.

## What I built

I used WordPress as the application shell and built a custom operations platform on top of it.

That choice was deliberate. The office team already knew the CMS, so using WordPress made the system easier for them to adopt and manage day to day. It preserved familiar admin patterns and reduced training overhead, while still giving enough flexibility to build custom workflows and data structures.

The result was not a content site with a few forms added in. It was a working internal product with its own data model, role-based workflows, approval logic, document-state management, expiry tracking, compliance gating, attendance records, notifications, exports, secure file delivery, and map-based oversight.

At a high level, the platform connects four systems:

1. work supervision and field submissions,
2. document governance, status monitoring, and expiry tracking,
3. attendance and hours review,
4. operational atlas and management visibility.

## Why this project matters

The strongest part of the project is not the stack and not the number of features.

It is that the software changed how the business operates.

Instead of hoping the right person sees the right message at the right time, the workflow now carries the work forward. Reports move into review. Documents move through clear states such as pending, approved, rejected, expiring, and expired. Sensitive records are retrieved through more controlled access patterns instead of loose sharing. Critical actions can be blocked when required records are not valid. Attendance is tied to real projects. Managers and office staff can monitor operational, compliance, and review status without piecing it together manually.

The security story matters here because document disorder was not only inefficient. It also made sensitive records harder to control. By centralizing documents, tracking their status, and restricting access through governed workflows, the platform improved both day-to-day execution and operational trust.

## Results

| Area | Before | After |
|------|--------|-------|
| Report approval cycle | 48 hours | 12 hours |
| Expired document incidents | 6 per month | 0 per month |
| Payroll correction rate | 18% | under 5% |
| Coordination overhead | high manual effort | lower by about 20 hours per week |


## Screenshots

![Operational atlas](screenshots/01-operational-atlas.png)
Map-based project and workforce visibility.

![Field submission form](screenshots/03-field-submission-form.png)
Structured field report submission tied to project and supervisor.

![Pending approvals](screenshots/05-approval-dashboard.png)
Submissions queued for review with status tracking.

![Approved records](screenshots/06-approved-dashboard.png)
Approved submissions with PDF export for payroll and compliance.

![Worker roster](screenshots/07-monter-list.png)
Worker list with compliance status and assignment context.

