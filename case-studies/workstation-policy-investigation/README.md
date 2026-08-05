# Workstation Policy Investigation

## Overview

This investigation looked at an employee workstation to check for possible violations of the company's acceptable-use policy, and to see whether the digital evidence actually backed up the employee's reported business travel.

The analysis pulled together forensic image verification, read-only mounting, EXIF metadata analysis, OOXML document inspection, and Firefox SQLite artifact analysis.

## Scenario

An employee was suspected of visiting unauthorized music-streaming and music-publication sites during work hours. Separately, the employer wanted to know whether artifacts on the workstation could confirm company-funded conference travel the employee had reported.

Two questions drove the investigation:

- Was there evidence of prohibited music-related activity on the workstation?
- Did the image metadata actually support the employee's reported travel dates and locations?

## Objectives

- Verify the forensic image's integrity
- Preserve the evidence through read-only examination
- Analyze image and PDF metadata
- Examine OOXML document contents and metadata
- Review Firefox browsing history, cookies, and form history
- Compare photo timestamps and coordinates against the provided travel records
- Separate what the evidence actually supported from what it couldn't establish

## Tools Used

`sha256sum`, `dc3dd`, `mount`, `ntfs-3g`, ExifTool, `zip`, `xmllint`, SQLite, Linux

## Investigation Process

1. Hashed the received evidence with SHA-256 and checked it against the provided value before starting anything.

2. Created a forensic working image with `dc3dd`, which logged hashes and activity during acquisition.

3. Mounted the working image loopback, read-only, and no-execution, to keep from accidentally modifying the evidence.

4. Went through 18 photographs recovered from the employee's Pictures directory using ExifTool.

5. Pulled whatever metadata was available: capture date and time, GPS latitude/longitude, camera or device model, and software/file metadata.

6. Compared the photo dates and coordinates against the company's conference schedule.

7. Looked at an Eventbrite PDF ticket and reviewed its creation metadata.

8. Found `MyFavoriteBands.docx`, pulled apart its OOXML contents, and went through `document.xml`, `core.xml`, and `app.xml`.

9. Identified music-related artist names in the document and pulled creator, application, and timestamp metadata.

10. Went into the employee's Firefox profile using SQLite.

11. Queried `places.sqlite` for URLs and visit timestamps, `cookies.sqlite` for domain-related cookie counts, and `formhistory.sqlite` for stored searches, usernames, and form entries.

12. Pulled all of it together, browser, document, and metadata evidence, and lined it up against the allegations under investigation.

## Key Findings

- Firefox history showed access to Spotify, Pitchfork, and Stereogum between roughly 3:04 PM and 3:29 PM on June 30, 2015.
- Cookie records tied to the Firefox profile included 42 for `spotify.com`, 16 for `pitchfork.com`, and 8 for `stereogum.com`.
- Firefox form history included music-related search terms and stored entries tied to the employee's username and company email.
- `MyFavoriteBands.docx` listed artist names including The Weeknd, Crystal Castles, Arcade Fire, and Japandroids.
- OOXML metadata attributed the document to the employee and showed it was created in Microsoft Word for Mac.
- Metadata from several photos lined up with dates and locations in the employer's conference records.
- Other photos were taken at times or places that didn't match the supplied conference schedule at all.
- The photo metadata could confirm where and when certain pictures were taken, but it couldn't independently prove the employee attended those conferences or that the travel was company-funded.
- Between the browser activity and the document artifacts, the combined evidence supported the conclusion that music-related activity had occurred on the workstation.

## Skills Demonstrated

- Forensic image verification
- Read-only evidence handling
- EXIF and GPS metadata analysis
- OOXML document examination
- Firefox SQLite analysis
- SQL querying
- Browser history reconstruction
- Cross-artifact correlation
- Evidence-based limitation statements
- Expert-witness-style reporting

## Technical Details

Commands, SQL queries, artifact paths, metadata examples, and travel-correlation notes are documented in the supporting technical notes.

[View technical notes](./technical-notes.md)

## Academic Disclaimer

This investigation was completed as part of academic digital forensics coursework. The organization, employee, allegations, identities, and evidence were provided for educational purposes and don't represent a real client or active investigation.
