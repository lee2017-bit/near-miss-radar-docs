# Near-Miss Radar privacy policy

Last updated: 2026-11-15

Near-Miss Radar is a Forge app for Jira built by an independent developer. It runs entirely on Atlassian's Forge platform ("Runs on Atlassian"): it has no external servers and sends no data outside Atlassian.

## What the app stores

Inside your Atlassian site's Forge storage (Forge SQL and the Forge key-value store), the app stores:

- **Close-call events**: time, floor-plan position and zone, source (machine or person), event type, severity, device ID (for example `AMR-01` or `FL-12`), measured distance, and for worker reports an optional short note.
- **Hotspots**: zone, counts, rates, the Jira work item key, the recorded corrective action and the effectiveness verdict.
- **Settings**: the Jira project and service project used, and the signing secret for machine feeds (stored as a Forge secret).

The app does **not** store Atlassian account IDs, names, email addresses, images, video, location of people, or health and injury data. Worker reports can be anonymous; the report form asks people not to include names. Corrective actions and comments the app writes are stored in Jira itself, under your site's existing data controls.

## How data is used

Only to show the floor plan, detect hotspots, create and update hotspot work items in your Jira site, verify corrective actions, and answer questions through the app's Rovo agent and MCP tools within your site.

## Sharing

The app does not sell or share data and does not send it to third parties. Rovo and Atlassian MCP features run inside Atlassian's platform under Atlassian's own terms.

## Retention and deletion

Data stays in your site's Forge storage until an administrator reinstalls the demo site (which replaces it) or uninstalls the app, after which Forge storage is deleted according to Atlassian's hosted storage data lifecycle.

## Contact

Questions or requests: open an issue at https://github.com/lee2017-bit/near-miss-radar-docs/issues
