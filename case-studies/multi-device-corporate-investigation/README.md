# Multi-Device Corporate Investigation

## Overview

This academic digital-forensics investigation examined multiple workstation and USB forensic images connected to M57 Patents.

The investigation began with allegations involving contraband media recovered from a secondhand computer believed to have originated from the company. Analysis expanded across several employee devices and uncovered evidence involving deleted media, USB transfers, company equipment, email activity, browser artifacts, and a possible unauthorized computer sale.

## Scenario

A computer purchased through the secondary market contained prohibited material and evidence that removable media had previously been connected.

The computer was believed to have originated from M57 Patents and to have been assigned to an employee named Jo Smith. Investigators obtained workstation images, USB images, reports, and related company devices for examination.

The investigation sought to determine:

- whether the recovered material could be connected to Jo Smith’s devices,
- whether files had been transferred between a workstation and USB drives,
- how company equipment reached the secondary market,
- and whether other suspicious activity occurred inside the organization.

## Objectives

- Verify the integrity of all supplied evidence files.
- Convert E01 evidence images into raw working images.
- Examine workstation and USB filesystems.
- Recover and correlate deleted media across devices.
- Analyze timestamps and metadata for evidence of file transfer.
- Review email and browser artifacts related to company equipment.
- Reconstruct events surrounding the removal and sale of a company computer.
- Identify additional suspicious activity across employee devices.
- Separate direct evidence from investigative inference.

## Evidence Sources

The investigation included multiple E01 forensic images and supporting reports, including:

- Jo Smith’s workstation
- Jo Smith’s work USB
- Jo Smith’s personal USB
- Terry’s workstation
- Terry’s work USB
- Pat’s workstation
- additional employee and device images
- investigative reports and warrant documentation

## Tools Used

- Autopsy
- libewf
- `ewfexport`
- `sha512sum`
- `mmls`
- `mount`
- ExifTool
- Linux command-line tools
- email artifact analysis
- browser-cache analysis

## Investigation Process

Most artifact examination and cross-device correlation were performed in Autopsy after the evidence images were validated and prepared.

1. Calculated SHA-512 hashes for the provided evidence images and supporting reports.

2. Compared the calculated values against the supplied checksum list to verify evidence integrity.

3. Used `ewfexport` from the libewf toolset to convert E01 images into raw `.dd` working images.

4. Examined the raw images using command-line tools and loaded relevant devices into Autopsy for broader artifact review.

5. Reviewed Jo Smith’s workstation, work USB, and personal USB for deleted and allocated media files.

6. Identified deleted image and video filenames appearing across the workstation and USB devices.

7. Examined timestamps and dot-underscore metadata files to determine when groups of files may have been transferred.

8. Compared file names, creation times, metadata, and storage locations across the workstation and USB images.

9. Expanded the investigation to other employee devices after evidence suggested that a company-owned Dell computer had been removed from Jo’s possession.

10. Reviewed Terry’s email records, deleted-mail folders, browser cache, and related image artifacts.

11. Correlated internal company communications with Craigslist-related email activity and evidence of a proposed $1,000 computer sale.

12. Reconstructed a timeline linking removal of the company computer, creation of a sales listing, communication with a buyer, and later recovery of the computer through the secondary market.

13. Reviewed additional employee-device artifacts for other potentially suspicious activity within the organization.

## Key Findings

- Evidence hashes matched the supplied values for the major forensic images and reports examined.

- Jo Smith’s workstation, work USB, and personal USB contained overlapping filenames associated with deleted image and video media.

- Files named `DSC00003.JPG` through `DSC00084.JPG`, along with several video files, were identified on Jo’s USB devices.

- Corresponding filenames appeared on Jo’s workstation, although some file contents had been deleted and only filesystem traces remained.

- Media creation timestamps ranged from approximately November 11 through November 20, 2009.

- Numerous dot-underscore metadata files were created within a short period on November 20, supporting the possibility of a bulk transfer to removable media.

- Evidence indicated that a Dell computer assigned to Jo was removed from his possession around November 20.

- Four days later, a Craigslist listing advertised a Dell computer for $1,000.

- Email records associated with Terry showed communication about the sale and arrangements with a prospective buyer.

- Terry wrote that he might be “$1000 richer” following an upcoming transaction.

- A Craigslist posting-confirmation email was recovered from Terry’s deleted-mail folder.

- A Craigslist-hosted image of the Dell computer was recovered from Terry’s Chrome browser cache.

- The selective deletion of the posting-confirmation email may indicate an attempt to obscure the sale, although it does not by itself prove deliberate concealment.

- The combined timeline supported further investigation into whether company equipment was sold without authorization.

- The broader review identified suspicious activity involving multiple employees and devices rather than a single isolated workstation.

## Interpretation

The device and USB artifacts supported a relationship between Jo Smith’s workstation and removable media containing related deleted files.

The email, browser, and timeline artifacts supported the conclusion that Terry was involved in arranging the sale of a Dell computer believed to belong to M57 Patents.

The evidence strongly supports unauthorized handling or sale of company property, but individual artifacts should not be treated as standalone proof of intent or criminal responsibility.

## Skills Demonstrated

- Multi-device forensic analysis
- E01 evidence handling
- Hash verification
- Raw-image conversion
- Autopsy artifact review
- Deleted-file analysis
- USB and workstation correlation
- Timestamp reconstruction
- Email artifact analysis
- Browser-cache analysis
- Cross-user evidence correlation
- Corporate incident reconstruction
- Evidence-based limitation statements

## Technical Details

Commands, evidence-image names, hash-verification notes, file-transfer observations, and transaction-timeline artifacts are documented in:

[View technical notes](./technical-notes.md)

## Academic Disclaimer

This investigation was completed as part of academic digital-forensics coursework using a simulated corporate scenario and educational evidence set. The names, organization, devices, and allegations do not represent a real client or active investigation.
