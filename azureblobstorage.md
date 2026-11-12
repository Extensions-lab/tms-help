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

1. Open [TMS Setup](setup.md) and expand **Azure Blob Storage for Attachments**.
2. Turn on **Enabled**.
3. Fill **Account Name**.
4. Fill **Container**.
5. Enter **Account Access Key**.
6. Choose **Check Connection**.
7. Fix any Azure or credential errors before transferring files.

The account and container identify the storage destination for the company's TMS attachments. Once Azure-backed attachments exist, TMS prevents changes to **Account Name** and **Container**, including when **Enabled** is turned off. You can still replace the access key for the same account and container.

Turning off **Enabled** stops new Azure uploads, transfer, and Free Space. Existing Azure-backed attachments continue to read from Azure using their saved storage type; keep the account, container, and a valid key configured. Local attachments continue to use their Business Central copy.

Moving existing attachments to another destination requires a coordinated file migration and verification. Editing the setup fields does not migrate files. This workflow does not include an automatic destination migration.

## Credential security

TMS stores the account access key in encrypted Business Central isolated storage with company scope. TMS accesses the key internally and passes it to the Azure Storage client as `SecretText`. After migration, runtime attachment operations read isolated storage only and do not fall back to the obsolete plaintext field in the TMS Setup table.

- The setup page displays a masked placeholder when a key exists and never loads the saved value back into the page field.
- Entering a new value rotates the saved key. Clearing the field removes it. Both operations require permission to modify **TMS Setup**.
- The storage account name and container are company-specific setup values and are protected from changes while Azure-backed attachments exist.
- Encrypted isolated-storage input is limited to at most 215 plain characters, and special characters can reduce the effective limit. A standard Azure Storage account key fits within the supported size.

The historical credential migration moves the existing table value to encrypted isolated storage and verifies that it can be read back before clearing the plaintext field. If storage cannot retain the credential, that migration stops so the problem can be corrected and the upgrade retried. Later upgrades assume this earlier migration has completed and clear residual plaintext without restoring a missing stored key. If the stored key is missing, configure it again in **TMS Setup**.

Operations users can continue to use TMS attachments without setup-maintenance rights. Dependent AL extensions must use supported attachment business operations; the obsolete public `HasABSAccountAccessKey`, `GetABSAccountAccessKey`, and `SetABSAccountAccessKey` methods remain declared for compatibility but reject calls with an error. If the stored key is missing, an administrator must configure it in **TMS Setup**.

## Transfer existing attachments

1. Open [TMS Setup](setup.md).
2. Choose **Transfer Attachments to ABS**.
3. Wait until the transfer completes.
4. Review a sample attachment from each major document type.
5. Choose **Free Space** only after the transfer has been validated.

## Free Space

**Free Space** checks each transferred attachment that still has a local Business Central copy. It downloads the file from the configured Azure destination and compares its byte length and SHA-256 fingerprint with the local copy. TMS removes the local file reference only when both comparisons match.

If Azure cannot return the file, or the contents differ, TMS keeps the local reference. The result reports counts and identifies each failed attachment by file name, document number, and reason. Correct the storage or file issue before running **Free Space** again. **Check Connection** checks access to the container; it does not verify individual files.

Attachments transferred before these safeguards are checked in the same way during cleanup. Attachments that already have no local copy are skipped because there is no local original to compare.

Free Space also retains its existing cleanup of attachments whose active Forwarding Order or Freight Order no longer exists. That separate cleanup can delete orphan attachment records and their Azure files. The comparison protects local references of retained attachments.

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
- **Transfer Attachments to ABS** uploads existing local TMS attachments to Azure Blob Storage and retains their local references until **Free Space** verifies them.
- **Free Space** removes the local reference only after verifying that Azure returns the same file. Business Central manages subsequent reclamation of Media storage.
- Keep the configured account and container for existing Azure-backed attachments. Turning off ABS does not allow a destination change.
- Do not rotate the storage key without updating TMS Setup.
- The saved key cannot be revealed from TMS Setup. Obtain a replacement from Azure when rotation is required.

## Troubleshooting

| Problem | What to check |
|---|---|
| Check Connection fails | Check account name, container, access key, network policy, and Azure permissions. |
| Attachment does not open | Check whether the file exists in Blob Storage and the setup still uses the correct key. |
| Transfer stops | Review the error message, fix the storage issue, then run transfer again. |
| Account name or container cannot be changed | Azure-backed attachments still use this destination. Keep it configured, or arrange a coordinated file migration and verification. Access-key rotation for the same destination remains available. |
| Free Space reports a verification failure | The reported attachment keeps its local reference. Check Azure read access and restore the matching remote file, then retry. A successful Check Connection alone does not resolve a missing or different file. |
| Extension upgrade reports that the key cannot be migrated | Confirm that the configured value is a valid Azure Storage account key and does not exceed 215 characters, rotate it if necessary, and retry the upgrade. |
| Screenshot exposes a key | Replace the screenshot immediately and rotate the key if it was shared outside the trusted team. |

## Related

- [TMS Setup](setup.md)
- [Attachment Control](attachmentcontrol.md)
- [Forwarding Order](forwardingorder.md)
- [Freight Order](freightorder.md)
