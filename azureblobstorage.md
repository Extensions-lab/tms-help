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

## Credential security

TMS stores the account access key in encrypted Business Central isolated storage with company scope. The key is not retained in the TMS Setup table and is returned to the Azure Storage client as `SecretText`.

- The setup page displays a masked placeholder when a key exists and never loads the saved value back into the page field.
- Entering a new value rotates the saved key. Clearing the field removes it.
- The storage account name and container remain regular company-specific setup values.
- Encrypted isolated-storage input is limited to at most 215 plain characters, and special characters can reduce the effective limit. A standard Azure Storage account key fits within the supported size.

When the extension is upgraded from a version that stored the key in the TMS Setup table, the upgrade moves the existing value to encrypted isolated storage and verifies that it can be read back. Only then is the plaintext table value cleared. If encrypted storage cannot retain the credential, the upgrade stops without erasing the existing value so the problem can be corrected and the upgrade retried.

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
- The saved key cannot be revealed from TMS Setup. Obtain a replacement from Azure when rotation is required.

## Troubleshooting

| Problem | What to check |
|---|---|
| Check Connection fails | Check account name, container, access key, network policy, and Azure permissions. |
| Attachment does not open | Check whether the file exists in Blob Storage and the setup still uses the correct key. |
| Transfer stops | Review the error message, fix the storage issue, then run transfer again. |
| Extension upgrade reports that the key cannot be migrated | Confirm that the configured value is a valid Azure Storage account key and does not exceed 215 characters, rotate it if necessary, and retry the upgrade. |
| Screenshot exposes a key | Replace the screenshot immediately and rotate the key if it was shared outside the trusted team. |

## Related

- [TMS Setup](setup.md)
- [Attachment Control](attachmentcontrol.md)
- [Forwarding Order](forwardingorder.md)
- [Freight Order](freightorder.md)
