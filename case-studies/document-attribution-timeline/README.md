
# Document Attribution and Timeline Reconstruction

## Overview

This investigation looked at two disk images to figure out where a document called `Treatment Plant Results of Purification.docx` actually came from, who accessed it, and when it got deleted. I pulled together filesystem metadata, document metadata, NTFS journal activity, Windows shortcut files, registry artifacts, and Internet Explorer cache records to piece together whether the document started with one user and later showed up on someone else's system.

## Scenario

A university suspected that material created by Allison Origin had been used without authorization by Kelly Copy in connection with a grant proposal.

I was given two forensic images:

- a disk image of Allison Origin's USB drive
- a disk image of Kelly Copy's computer

The goal was to identify who actually created the document, recover any deleted copies, and reconstruct what happened with the document on Kelly's system.

## Objectives

- Verify the integrity of both forensic images
- Recover the deleted document from the USB image
- Identify the document's creator and modification metadata
- Build filesystem and system-wide forensic timelines
- Identify access, shortcut, registry, browser, and deletion artifacts
- Correlate evidence from both devices into a chronological sequence
- Determine whether the document was accessed from Kelly Copy's system

## Tools Used

Sleuth Kit, `fls`, `icat`, `mactime`, Plaso, `log2timeline.py`, `psort.py`, ExifTool, `grep`, MD5/SHA-256/SHA-512 hashing tools, Linux

## Investigation Process

1. Started with two compressed forensic images, one for Allison Origin's USB drive, one for Kelly Copy's computer.

2. Hashed both images with MD5, SHA-256, and SHA-512 and checked the results against the provided values to confirm nothing had been altered.

3. Used Sleuth Kit to examine the USB image and build a filesystem timeline with `fls` and `mactime`.

4. Found a deleted file named `Treatment Plant Results of Purification.docx` sitting in the timeline.

5. Recovered the deleted document using `icat`.

6. Ran ExifTool against the recovered document. The metadata pointed to `Alison Origin` as both the creator and last person to modify it, created in Microsoft Word for Mac.

7. Built a full Plaso super timeline from Kelly Copy's disk image with `log2timeline.py`, then processed it with `psort.py`.

8. Filtered that timeline down to anything touching the document, `.docx` filenames, Word temp file patterns, deleted NTFS records, shortcut files, registry activity, and browser history.

9. Went through the NTFS `$UsnJrnl` entries and found deleted document traces sitting between roughly 11:30 AM and 12:11 PM CDT on July 13, 2015.

10. Checked Windows `RecentDocs` and MRU registry artifacts, which showed repeated access to the document under Kelly Copy's account.

11. Pulled shortcut and shell-item records tied to `C:\Users\Kelly Copy\Desktop\Grant\Treatment Plant Results of Purification.docx`.

12. Looked through Internet Explorer cache records and found local-file references to that same document path.

13. Put everything together, evidence from both images, to reconstruct the full creation, access, and deletion sequence.

## Key Findings

- Recovered a deleted copy of `Treatment Plant Results of Purification.docx` from Allison Origin's USB image.
- ExifTool metadata identified `Alison Origin` as the document's creator and last modifier, created in Microsoft Word for Mac, with timestamps from July 13, 2015.
- Artifacts on Kelly Copy's computer showed repeated interaction with the same document between roughly 11:36 AM and 12:11 PM CDT.
- 11:36:06 AM: a Windows shortcut for the document was created in Kelly Copy's desktop `Grant` folder.
- 11:53:32 AM: that shortcut was accessed again.
- 11:53:34 AM: the associated shell item got updated.
- 12:02:08 PM: the document showed up as the most recently used `.docx` file in an MRU registry key.
- 12:11:05 PM: it appeared in the broader `RecentDocs` list, alongside files like `Proposal.doc`, `Filter.xlsx`, and `Summary.xlsx`.
- Internet Explorer cache records referenced the document's local path around 12:02 PM.
- NTFS journal artifacts showed deleted-file activity consistent with Word document interaction during that same window.
- Put together, the evidence shows a document created by Allison Origin was present and repeatedly accessed under Kelly Copy's account before related deletion activity followed.

## Skills Demonstrated

- Evidence integrity verification
- Deleted file recovery
- Filesystem timeline creation
- Plaso super-timeline analysis
- NTFS USN Journal interpretation
- Registry MRU and RecentDocs analysis
- Windows shortcut analysis
- Browser cache analysis
- Cross-device evidence correlation
- Technical timeline reconstruction

## Technical Details

More on the exact commands, artifact paths, timestamps, and timeline notes is in the supporting technical notes.

[View technical notes](./technical-notes.md)

## Academic Disclaimer

This investigation was completed as part of academic digital forensics coursework. The scenario, identities, and evidence were provided for educational purposes and don't represent a real client or active investigation.
