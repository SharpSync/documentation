---
icon: arrow-right-from-bracket
---

# Submitting a BOM for update

When a BOM is submitted in SharpSync, it pushes the information to the ERP.

There are 2 methods for communicating with PDM:

1. Pulling the information from SharpSync
2. Pushing the information to PDM



### 1. Pulling the Meta Data from SharpSync

This method is _not_ the preferred method because it relies on the user to pull the information from SharpSync.  PDM's API has severe limitations in updating file data cards however, so this method is more reliable than method #2 (Pushing the meta data to PDM).

The pull data from SharpSync, submit the BOM in SharpSync.

In PDM, select the file you want to pull the BOM for. Right Click > Pull BOM...

### 2. Pushing the Meta Data to PDM

This is the preferred method, but due to instability in the implementation of the PDM API it is not reliable and, therefore, not supported at this time. A future service pack to PDM (2025 is current at the time of writing) may make the implementation more stable and let us focus on pushing the information to the files.
