---
icon: shield-check
---

# Data Safety

At SharpSync we take the safety of your data very seriously.

While we would all like to pretend that penetrations never happen, no one is truly safe and the risk of IP exposure is real due to phishing attacks or compromised user machines.&#x20;

That's why we are transparent about our safety mechanisms and the steps we take to ensure your data is protected at all times. We employ industry-leading encryption protocols to safeguard your data both in transit and at rest. Our systems undergo regular security audits, and we work with trusted third-party experts to identify and address potential vulnerabilities. Additionally, we implement multi-factor authentication (MFA) and continuous monitoring to prevent unauthorized access.

We also have strict access control measures in place, ensuring only authorized personnel can interact with sensitive information. &#x20;

### Per Organization Access

Each organization in SharpSync is unique.&#x20;

When logging in to SharpSync, you only see your organization's data. No other organization's data is visible, so it's not possible for you to see another organization's BOM details, or vice versa.

### Per User Access

Each user in SharpSync is unique.&#x20;

When logging in to SharpSync, you only see your login's BOM data. No other user's data is visible, so it's not possible for you to see another organization's BOM details, or vice versa.

The exception to this is when another user (in the same organization) shares a Bill of Material with you explicitly using the 'Share' button.

### Role Based Access Controls

We provide access to setup and administration via role based access controls. The roles are:

| Role name     | Description                                                                 |
| ------------- | --------------------------------------------------------------------------- |
| Administrator | Can edit data sources, property mappings and BOMs, Billing, Users and more. |
| Editor        | Can edit, transfer & view BOMs                                              |
| Viewer        | Can transfer & view BOMs                                                    |

&#x20;An editor and viewer cannot view the setup connection details for a `DataSource`. This means sharing a BOM with a colleague does not give that person visibility on how you connect to your sources.&#x20;

This is great for inviting other users to your SharpSync instance and sharing BOM information with them without exposing your authentication secrets. Each user's session is authenticated individually with a source and kept separate from other users.

See more about permissions at [application-permissions.md](../user-management/application-permissions.md "mention") and [Broken link](broken-reference "mention")

### Pre-empting Data Leaks

To pre-empt any potential data leakage, the following mechanism are in place:

#### Automatic deletion of unused or stale data

To prevent stale data from hanging around, we've implemented a 30 day stale data policy.

* **Bill of Material** (BOM) data is only kept around for a maximum of 30 days.&#x20;
* Thumbnails
* Metadata
* Hierarchical data
* File derivatives
* Backup copies of derivatives
* Organizational data is kept up to&#x20;

#### Conditions for triggering

The removal of stale data condition is triggered when:

* There is no activity in your account OR
* You have not interacted with an individual Bill of Materials in 30 days.

When this condition is triggered, you will receive an optional email notifying you of the removal of the data. This does _not_ delete the data at the source (i.e. CAD, PLM or ERP source), it simply removes _your_ data from _our_ servers.

### Reduced logging

At SharpSync we use diagnostic logging to troubleshoot our software. This is what all great software companies do. We take this a step further by taking the following preventative measures:

* We don't store any:
  * User names,&#x20;
  * Identifying names&#x20;
  * Company names
  * Account references.
* We only store our application's internal ids for these objects
* We automatically remove (delete) any log entries older than 7 days. This future proofs the logs by simply not making it available for a mining exfiltration attack.

### Preventing Credentials Leakage

You user authentication credentials are stored in a [vault](https://www.vaultproject.io/). SharpSync staff does not have access to it unless _you provide them_ with admin access to your instance.

If for any reason there are any problems with the server, the vault is automatically locked

At SharpSync, we aim to give you the peace of mind that your data is in safe hands. Your trust is paramount, and we are committed to maintaining the highest level of security and transparency.

If there are any obligatory certifications that _you_ have to adhere to and we can implement, please reach out to us for further comment.

We support OAuth2.0 authentication flow for sources (if supported at the source), meaning that user credentials are not stored at SharpSync. Should this become the target of an attack, simply revoke the OAuth client id and secret at your source.
