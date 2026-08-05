# Document Attribution and Timeline Reconstruction — Technical Notes

## Evidence Sources

I worked from two forensic images:

- Allison Origin's USB drive
- Kelly Copy's computer

Both were hash-verified against the provided values before I touched anything else.

## Integrity Verification

Checked both images with MD5, SHA-256, and SHA-512, and compared the results against the supplied hashes to confirm nothing had changed before analysis started.

## USB Filesystem Analysis

Used Sleuth Kit to inspect the USB image and build a filesystem timeline.

Main tools:

```bash
fls
mactime
icat
```

Found a deleted file named `Treatment Plant Results of Purification.docx` and recovered it with `icat`.

## Document Metadata

Ran ExifTool against the recovered document. It came back with:

- Creator: Alison Origin
- Last modifier: Alison Origin
- Application: Microsoft Word for Mac
- Creation and modification timestamps from July 13, 2015

That metadata is what tied the document back to Allison Origin.

## Plaso Timeline Generation

Processed Kelly Copy's disk image with Plaso.

Main tools:

```bash
log2timeline.py
psort.py
```

Filtered the resulting super timeline down to anything relevant, the document's filename, `.docx` artifacts, Word temp files, shortcut files, registry MRU entries, browser cache records, deleted NTFS records, and `$UsnJrnl` activity.

## Relevant Artifact Types

**Windows Shortcut Files**

Shortcut evidence pointed to `C:\Users\Kelly Copy\Desktop\Grant\Treatment Plant Results of Purification.docx`, created around 11:36:06 AM and accessed again around 11:53:32 AM on July 13, 2015.

**Shell Items**

Shell-item activity tied to the document path showed up around 11:53:34 AM.

**Registry MRU Evidence**

The document showed up in Microsoft Office and Windows recent-document artifacts, including RecentDocs, `.docx` MRU entries, and Office recent-file records.

Around 12:02:08 PM, it appeared as the most recently used `.docx` file. By 12:11:05 PM, it had moved into the broader recent-documents list alongside files like `Proposal.doc`, `Filter.xlsx`, and `Summary.xlsx`.

**Internet Explorer Cache**

IE cache records referenced the same local document path around 12:02 PM, supporting the idea that the file had actually been opened or accessed from Kelly Copy's system.

**NTFS USN Journal**

The `$UsnJrnl` showed deleted-file activity tied to Word documents during the same window, roughly 11:30 AM to 12:11 PM on July 13, 2015.

## Reconstructed Timeline

| Approximate time | Artifact | Interpretation |
|---|---|---|
| 11:36:06 AM | Shortcut created | Document present in Kelly Copy's Grant folder |
| 11:53:32 AM | Shortcut accessed | Additional interaction with the document |
| 11:53:34 AM | Shell item updated | File path accessed through the Windows shell |
| 12:02:08 PM | .docx MRU entry | Document recorded as recently used |
| ~12:02 PM | IE cache reference | Local document path recorded in browser cache |
| 12:11:05 PM | RecentDocs entry | Document remained in the user's recent-file history |
| 11:30 AM–12:11 PM | USN Journal deletion activity | File deletion or temporary-file cleanup occurred |

## Correlation

The USB image had a deleted document attributed to Allison Origin. Kelly Copy's computer had several independent artifacts referencing that same filename and path, shortcut records, shell-item activity, MRU registry entries, RecentDocs entries, browser cache references, and NTFS journal activity.

Taken together, that correlation supports the conclusion that the document was present and repeatedly accessed under Kelly Copy's user account.

## Limitations

- These artifacts show presence and access, not intent. They can't independently prove what the user meant to do.
- Some timestamps may reflect filesystem or application activity rather than something the user directly did.
- I only included exact command syntax where it was preserved in the original coursework or terminal history, rather than reconstructing it after the fact.
