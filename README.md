# Employee Access & Onboarding — ServiceNow Administration Lab

**A self-directed, non-production ServiceNow project exploring employee onboarding requests, access approvals, fulfillment, and operational troubleshooting.**

**Project status:** In progress · **Platform:** ServiceNow Personal Developer Instance (PDI) · **Repository:** ServiceNow-generated application exports and selected configuration records

## Project overview

An employee's first day can require a laptop, application permissions, VPN access, and additional review for privileged accounts. This lab models how an IT service team could gather those requirements consistently, route sensitive access for approval, coordinate fulfillment, and communicate request status.

I developed this project to practice translating service requirements into ServiceNow configuration, managing change across a Personal Developer Instance, documenting technical decisions, and diagnosing platform problems. It is **personal lab work, not a production deployment or paid enterprise administration experience**.

## What I worked on

- **Application structure:** Created the `Employee Access & Onboarding` scoped application with application-specific user and administrator roles.
- **Request intake:** Configured an `Employee Onboarding & Access Request` Service Catalog item in the PDI, with ten request variables and supporting fictional test identities/groups.
- **Workflow design:** Created an `Employee Onboarding & Access Fulfillment` Flow Designer definition that references request variables, approval decisions, fulfillment tasks, and record updates. The repository contains a published flow export; that does **not** establish that every end-to-end scenario passed testing.
- **Notification configuration:** Created a request-completion email notification record. Email delivery in a complete end-to-end scenario is **not yet documented as verified**.
- **Change handling:** Connected the scoped application to GitHub source control and used ServiceNow Update Set/XML export methods to preserve additional configuration outside this repository.
- **Incident investigation:** Troubleshot serious PDI slowness using Transaction Logs and `stats.do` diagnostics, then backed up and migrated work to a replacement PDI and checked restored catalog functionality.

### Workflow concept

1. A user submits an onboarding/access request and selects the resources needed.
2. ServiceNow reads request variables and determines which fulfillment paths apply.
3. Requests involving privileged access require appropriate security review.
4. Assigned groups complete the relevant fulfillment tasks.
5. The request is closed and a completion message is generated when the process is satisfied.

**Important:** This is the *intended workflow*. Individual flow components and configuration records are present, but full approval, rejection, task, notification, and failure-path behavior is still under development/validation. This README does not claim the process is production-ready.

## Evidence in this repository

The repository is synchronized from ServiceNow Studio, so most tracked application files are **generated XML configuration records**, not handwritten application source code. The artifacts below are directly accessible:

| Artifact | What it demonstrates |
| --- | --- |
| [Scoped application definition](./da0ba5a42f478710021314807fa4e370/sys_app_da0ba5a42f478710021314807fa4e370.xml) | Application identity, scope, and metadata |
| [Application user role](./da0ba5a42f478710021314807fa4e370/update/sys_user_role_331ba9a42f478710021314807fa4e3bc.xml) | Role configuration for application users |
| [Application admin role](./da0ba5a42f478710021314807fa4e370/update/sys_user_role_7d1b69a42f478710021314807fa4e3aa.xml) | Separate administrator role |
| [Flow Designer export](./da0ba5a42f478710021314807fa4e370/update/sys_hub_flow_77ba648283c74310c74d52e0deaad36c.xml) | A published flow definition referencing catalog variables, approval and fulfillment actions |
| [Completion email record](./da0ba5a42f478710021314807fa4e370/update/sysevent_email_action_f82c7cd88317c350c74d52e0deaad3a0.xml) | Notification configuration for request completion |
| [ServiceNow source-control properties](./sn_source_control.properties) | Location of the application export directory |

**What is not in this GitHub snapshot:** A standalone runnable web application; the complete Service Catalog configuration; fictional user/group records; independent end-to-end test results; or a finished collection of requirements, architecture decision records, and screenshots. Some PDI/global records were managed separately through Update Sets and XML backups. The presence of a flow XML export is not proof that every part of the intended workflow has been tested successfully.

## Technical investigation: PDI performance and recovery

One of the more valuable parts of this project was handling an environment issue rather than only configuring features.

**Problem:** The ServiceNow Personal Developer Instance developed severe latency during development.

**Investigation:** I reviewed Transaction Logs and platform diagnostics (`stats.do`), including request latency, servlet-memory indicators, semaphore queues, database connections, scheduler state, and host-load information. This helped distinguish possible application-configuration faults from broader PDI/shared-environment contention. I also tried a controlled cache flush while investigating.

**Recovery:** I preserved the source-controlled scoped application and separately exported dependent configuration through XML/Update Set methods before moving the work to a replacement PDI.

**Validation:** After the migration, I restored dependent records, confirmed the request catalog could be used again, and observed responsiveness returning to normal in manual checks. A sanitized, reproducible metrics comparison and formal test log have not yet been published.

This exercise provided practice in **evidence-led troubleshooting, backups, migration planning, dependency awareness, and checking the result after a change**.

## Skills demonstrated

- ServiceNow scoped application configuration, application roles, and Service Catalog concepts
- Flow Designer structure, request-variable mapping, conditional approval and fulfillment *design*
- Git-backed ServiceNow application source control, Update Sets, and XML export/recovery
- Platform diagnostics, incident investigation, change documentation, and validation
- Translating requirements into configuration decisions and explicit test cases

These skills describe hands-on **lab activities**, not claims of certified ServiceNow administration or production support experience.

## Current status and next steps

| Area | Status |
| --- | --- |
| Scoped app definition and two role records | **Present in repository** |
| Employee access request catalog item and test records | **Configured in PDI; not fully represented in this GitHub export** |
| Flow Designer definition | **Published export present; end-to-end behavior not independently verified** |
| Completion notification definition | **Configuration exported; delivery not independently verified** |
| Conditional privileged-access approval and fulfillment | **Further testing/refinement needed** |
| Repeatable screenshots, architecture diagram, and test evidence | **Not yet published** |

**Next development/documentation tasks**

1. Run and record representative standard, privileged, approved, rejected, and incomplete-request scenarios.
2. Validate task assignment, completion logic, status transitions, notification behavior, and error handling.
3. Publish a sanitized process diagram, test matrix, configuration screenshots, and short configuration/change notes.
4. Confirm the repository and non-source-controlled configuration can be restored on a fresh PDI.
5. Keep this README synchronized with features that have actually been tested.

## About the repository

This is a **portfolio and learning project by Morgan Koenig**. The exported files were generated by ServiceNow's source-control integration; they are meant to document a PDI application rather than serve as a turnkey install. ServiceNow-generated files inside the application directory should not be manually edited without understanding ServiceNow's source-control/checksum requirements.

The project uses fictional test data and is not associated with an employer's production environment. No live production data, access credentials, or secrets should be committed here.

---

*For hiring reviewers: start with the project overview, inspect the application/role/flow exports above, and see the troubleshooting case study for an example of how I approach an unfamiliar technical problem. I can discuss which features are working, which are in progress, and how I would verify them in a technical interview.*
