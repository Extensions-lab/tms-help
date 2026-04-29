---
title: "Documentation Standards"
description: "Authoring rules for the TMS for Logistics Service Providers GitHub Pages documentation site."
---

# Documentation Standards

Use these standards when adding or changing TMS documentation.

The goal is simple: a user should know what the page is for, when to use it, what to do next, and what to check when something does not work.

## Page template

Every user article should use this structure unless there is a clear reason not to:

```markdown
---
title: "Page title"
description: "One short sentence that explains the user value."
---

# Page title

Short purpose statement.

## Before you start
## How to work in this page
## Fields that matter most
## Good to know
## Troubleshooting
## Related
```

## Writing rules

- Write for Business Central users, not developers.
- Use short sentences.
- Prefer active voice.
- Start with the user task.
- Use tables for fields, actions, statuses, and troubleshooting.
- Link related pages at the end.
- Use exact Business Central page, action, and field names when they matter.
- Keep marketing language out of task articles.

## Product boundary

This documentation covers **TMS for Logistics Service Providers** only.

Keep this site focused on the LSP workflow. Do not link to unrelated product areas.

It is okay to use lowercase **shipper** when it means the party that releases cargo on a Forwarding Order.

## GitHub Pages rules

- Add YAML front matter to every published Markdown page.
- Use relative links such as `forwardingorder.md`.
- Store screenshots under `docs/resources/<topic>/`.
- Use lowercase filenames with hyphens or existing topic names.
- Do not link to missing screenshots.
- Keep `screenshot-registry.md` updated when a screenshot is needed.

## Screenshot rules

- Capture the full Business Central page when the layout matters.
- Crop only when the user needs to focus on one field group or action.
- Hide customer, vendor, address, and personal data.
- Use consistent demo data.
- Prefer one strong screenshot over several weak screenshots.

## Review checklist

Before publishing, check that:

- all local links work,
- all screenshot links exist,
- no page documents unrelated product areas,
- every page has front matter,
- the home page links to the new article,
- the article has a troubleshooting section when users can get blocked.

## Context-Sensitive Help Backlog

When AL pages are updated, add or review `ContextSensitiveHelpPage` mappings for these documentation pages:

- `freightagreement.html`
- `freightordertype.html`
- `purchaseassignment.html`
- `stageentries.html`
- `azureblobstorage.html`
- `orderwizards.html`
- `first-forwarding-job.html`
