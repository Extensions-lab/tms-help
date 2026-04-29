---
title: "Azure Blob Storage for Attachments"
description: "Configure Azure Blob Storage for TMS attachments and move existing files from Business Central storage."
---

# Azure Blob Storage for Attachments

Use **Azure Blob Storage** when your company wants TMS attachments stored outside the Business Central database.

This can reduce database storage pressure when users attach many files to Forwarding Orders, Freight Orders, agreements, carriers, vehicles, and drivers.

## Before you start

Make sure that:

- an Azure storage account exists,
- a blob container exists,
- your administrator has the storage account name, container name, and access key,
- users understand that credentials are sensitive and must not be shown in screenshots.

## Configure storage

1. Open [TMS Setup](setup.md).
2. Turn on **ABS Storage Enabled**.
3. Fill **ABS Account Name**.
4. Fill **ABS Container**.
5. Enter **ABS Account Access Key**.
6. Choose **Check Connection**.
7. Fix any Azure or credential errors before transferring files.

## Transfer existing attachments

1. Open [TMS Setup](setup.md).
2. Choose **Transfer Attachments to ABS**.
3. Wait until the transfer completes.
4. Review a sample attachment from each major document type.
5. Choose **Free Space** only after the transfer has been validated.

## What can be transferred

| Record area | Attachment support |
|---|---|
| **Forwarding Orders** | Active document attachments. |
| **Posted Forwarding Orders** | Posted history attachments. |
| **Freight Orders** | Active execution document attachments. |
| **Posted Freight Orders** | Posted execution history attachments. |
| **Forwarding Agreements** | Customer agreement attachments. |
| **Freight Agreements** | Carrier agreement attachments. |
| **Carriers, Vehicles, Drivers** | Master data attachments. |

## Good to know

- **Check Connection** validates that Business Central can reach the configured container.
- **Transfer Attachments to ABS** moves TMS attachment content to Azure Blob Storage.
- **Free Space** removes the imported file payload from Business Central after transfer.
- Do not rotate the storage key without updating TMS Setup.

## Troubleshooting

| Problem | What to check |
|---|---|
| Check Connection fails | Check account name, container, access key, network policy, and Azure permissions. |
| Attachment does not open | Check whether the file exists in Blob Storage and the setup still uses the correct key. |
| Transfer stops | Review the error message, fix the storage issue, then run transfer again. |
| Screenshot exposes a key | Replace the screenshot immediately and rotate the key if it was shared outside the trusted team. |

## Related

- [TMS Setup](setup.md)
- [Attachment Control](attachmentcontrol.md)
- [Forwarding Order](forwardingorder.md)
- [Freight Order](freightorder.md)
