# Security And Governance

## Why this area matters

Many internal tools handle sensitive workforce and compliance data poorly. This app stands out because security and governance are built into the workflow design, especially around documents, identity data, attendance history, and access-controlled files.

That matters because the document area is not just a convenience feature. It holds records that can affect whether someone is allowed to work, submit evidence, stay assigned to a project, or pass internal review. In practice, document disorder was both an operational problem and a data-protection problem.

## Document controls

The document system is not an open upload bucket.

It includes:

- mandatory document definitions,
- approval and rejection states,
- missing, expiring, and expired states,
- expiration tracking,
- rejection reasons,
- historical archiving,
- company-type-specific requirements,
- and secure download handlers.

That means documents are treated as controlled business records rather than loose attachments.

This is one of the clearest places where the platform improved the real operation. Before the system, finding one urgent document could involve several people searching messages, inboxes, and local folders. After the system, office staff could monitor document state centrally and retrieve the right record through a controlled workflow.

## Document-state monitoring

The platform does not just store files. It makes document status visible and actionable.

Office and admin users can see whether a required record is:

- pending review,
- approved,
- rejected,
- expiring soon,
- expired,
- or missing entirely.

That visibility matters because it turns compliance from a reactive cleanup exercise into an active operating process.

## Submission gating

One of the most important governance controls in the platform is this:

workers can be blocked from submitting work if they are assigned to a project that requires compliant documents and their required records are missing, expired, rejected, or still pending approval.

This is a powerful example of compliance enforcement through product design.

It also has a security dimension: the application does not rely only on reminders or manual oversight. It actively prevents downstream actions when the underlying records are not in a valid state.

## Terms and policy governance

The terms acceptance module does more than show a one-time checkbox.

It supports:

- mandatory acceptance for non-admin operational users,
- database-backed acceptance timestamps,
- admin-triggered global resets,
- storage of policy update history in `mhr_regulamin_updates`.

That turns policy acknowledgment into a governed system event.

## Access control patterns

The platform uses multiple layers of access control:

- logged-in checks,
- role checks,
- scope restrictions for supervisors,
- admin-like permission groups,
- secure AJAX endpoints with nonce verification,
- controlled secure-file delivery instead of naive public linking.

This is especially important because the app mixes operational data, identity data, compliance files, and attendance history.

The practical point is simple: not every logged-in user should be able to see every worker document, every attendance change, or every review workflow. The system is stronger because access is shaped around role and responsibility, not just basic authentication.

## Data protection signals

The reviewed code shows encryption helpers based on `AES-256-CBC` for sensitive contact fields such as mobile and WhatsApp values.

That is a strong design choice for a WordPress-based internal app and signals deliberate treatment of personal data.

## File handling hardening

The application also shows signs of defensive upload and file handling, including checks around file type validation and secure serving behavior.

Combined with document approval workflows, this reduces the risk of the document area becoming a weak point.

That matters because sensitive workforce files are exactly the kind of area where an internal system can become risky if uploads, downloads, or review permissions are too loose.

## Attendance governance

Attendance is also treated with more rigor than usual.

The schema and code support:

- edit counts,
- original value preservation,
- change logging,
- lock flags,
- anomaly flags,
- admin verification,
- manual correction records.

This gives the business a path to review suspicious or corrected time entries rather than trusting unchecked edits.

## Auditability and historical trace

One of the strongest governance signals across the platform is that important changes do not simply disappear into the latest saved state.

The reviewed design shows a pattern of keeping historical visibility around:

- document replacement and archive history,
- attendance edits and original values,
- policy acceptance timestamps,
- review decisions and rejection reasons,
- and status changes that affect downstream work.

That is important because internal trust depends on being able to answer not only what the current state is, but how it got there.

## Governance summary

The most important part is not any one security feature. It is the way governance is built into daily business flows:

- who can access what,
- which documents are visible, valid, expiring, or blocked,
- what must be approved,
- what blocks downstream actions,
- what changes are logged,
- and what remains historically visible.
