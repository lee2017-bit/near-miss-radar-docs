# Near-Miss Radar: setup for judges

Everything below works on a fresh Jira Cloud site with no robot, no sensors and no extra configuration. Total time: about 3 minutes.

## Requirements

- A Jira Cloud site where you are a site admin (a free developer site is fine).
- Any Jira project where you are a project admin. A new "Kanban" software space works.
- Optional: Jira Service Management (for worker reports from the customer portal) and Rovo (for the Safety Brief agent). The core flow works without either.

## 1. Install (1 minute)

1. Open the installation link from the Devpost submission.
2. Choose your site, select **Jira**, and approve the permissions:
   - `read:jira-work`, `write:jira-work`: create and update hotspot work items and comments.
   - `storage:app`: store close-call events and hotspots in Forge SQL.
   - `read:servicedesk-request`, `read/write:requesttype:jira-service-management`: only used if you enable portal reporting.

The app runs entirely on Atlassian (no egress, no external backend).

## 2. Load the demo site (30 seconds)

1. Open your project. In the space's top tabs, open **Near-Miss Radar** (it may be under **More**).
2. Go to **Setup** and press **Install demo site**. Wait about 20 seconds.

This loads a fictional assembly and distribution site with 45 days of close calls and creates two earlier hotspot work items in your project:
- **A1 Receiving dock**: a corrective action 20 days ago (traffic lights) that worked.
- **C2 Canteen walkway crossing**: a corrective action 18 days ago (repainted crossing) that did not.

## 3. See the main flow (1 minute)

1. Press **Run hotspot scan** (top right). You should see:
   - **New hotspot B7**: a new work item for the blind corner where machines logged ~15 close calls and people reported 1.
   - **A1**: verified effective, work item closed with the before/after numbers in a comment.
   - **C2**: not effective, work item reopened.
2. Press **Simulate shift** to stream live close calls onto the floor plan (they pulse as they arrive).
3. Open **Hotspots**, then click the new **B7** work item. The **Near-miss evidence** panel on the right shows machine vs people counts, the 45-day trend and worker notes. Record a corrective action there to start its 14-day check.

## 4. Optional: Rovo Safety Brief

On a site with Rovo, press **Ask Safety Brief** on the radar page, or open Rovo chat and pick the **Near-Miss Safety Brief** agent. Try "Prepare Monday's toolbox talk from the last 30 days". The agent quotes only numbers returned by the app's actions.

The same read-only tools are exposed through the Atlassian MCP server (`rovo:mcp`, server name "Near-Miss Radar"); the open skill file (published with the source code) describes them for third-party agents.

## 5. Optional: worker reports from the JSM portal

1. In **Setup → Worker reports (JSM portal)**, pick a service project and press **Create "Report a near miss"**.
2. Open the request type link shown there (or a zone poster link such as `...?zone=B7`). The form shows a tap-to-locate floor plan, also for customers and anonymous users if your portal allows them.
3. Submit. The request gets a thank-you comment and the report appears on the radar as a person-reported close call.

## 6. Optional: send a machine event

**Setup → Connect machines** shows the signed ingest URL, the secret and a `curl` example. The event format is documented in [EVENT_SCHEMA.md](EVENT_SCHEMA.md).

## Notes

- Scans run hourly; right after **Install demo site** the hourly scan pauses for 2 hours so the first scan you press is the one you see.
- **Install demo site** replaces existing Near-Miss Radar data (it never deletes Jira work items).
- The app does not control machines and is not a functional safety system.
