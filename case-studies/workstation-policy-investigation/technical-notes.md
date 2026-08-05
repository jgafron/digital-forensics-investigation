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

Same treatment as before, structure untouched, prose rewritten in first person. One extra thing I cleaned up: your original had a few stray :contentReference[oaicite:...] tags scattered through it, those look like leftover citation artifacts from a copy-paste (possibly from another AI tool's output), not something you'd want visible in your actual README, so I removed them.

Next, fill technical-notes.md for the Workstation Policy Investigation. Use this: # Workstation Policy Investigation — Technical Notes ## Evidence Source The investigation examined a forensic disk image of Kevin Tunes’s assigned workstation. The original compressed image was verified using SHA-

PASTED

markdown
# Workstation Policy Investigation — Technical Notes

## Evidence Source

Examined a forensic disk image of Kevin Tunes's assigned workstation.

Verified the original compressed image with SHA-256:

```bash
sha256sum del.dd.bz2
```

Calculated hash:

baef6587eace24809ead77a5b3eb6af3f99470efa0c825a46c966430d96d3d0d


## Forensic Working Copy

Created a working image with `dc3dd`:

```bash
dc3dd if=del.dd hof=out.dd verb=on hash=sha256 hlog=out.hashlog log=log rec=off
```

Input and output SHA-256 hashes matched:

2774034abc0ac9c0049f80c5a9cc9ec4b970480c16a584eab3df14d3a5ef72a0


That confirmed the working copy matched the source image exactly.

## Read-Only Mounting

Mounted the working image with:

```bash
sudo mount -t ntfs-3g -o loop,ro,noexec out.dd ~/out
```

- `loop` — mounts the image file as a virtual block device
- `ro` — mounts it read-only
- `noexec` — blocks anything on the mounted image from executing

Together, these kept the evidence from being modified or accidentally run.

## Photograph Metadata Analysis

Found 18 photographs in `Documents and Settings/Kevin Tunes/Pictures`.

Used ExifTool to pull creation timestamps, GPS coordinates, camera/device model, and other embedded metadata.

Devices showing up across the image set: Apple iPhone 4, Apple iPhone 4S, Canon PowerShot SD1100 IS, and NORITSU KOKI photo-processing equipment.

A few examples:

| File | Date | Location | Device |
|---|---|---|---|
| 01.jpg | March 18, 2011 | Memphis, Tennessee | iPhone 4 |
| 04.jpg | October 6, 2011 | St. Louis, Missouri | iPhone 4 |
| 11.jpg | July 15, 2012 | Chicago, Illinois | iPhone 4S |
| 12.jpg | July 14, 2012 | Chicago, Illinois | iPhone 4S |
| 18.jpg | July 19, 2014 | Chicago, Illinois | iPhone 4S |

A number of other photos either had no GPS coordinates or didn't line up with the supplied conference schedule.

## PDF Metadata

Found a PDF ticket at `Documents and Settings/Kevin Tunes/Documents/11291307605-327242879-ticket.pdf`.

ExifTool identified:

- Creator: Eventbrite
- Creation date: 2014-09-23 10:03:15-07:00

This confirms an Eventbrite-generated ticket existed, but on its own it doesn't prove the employee actually attended the event.

## OOXML Document Analysis

Recovered a document named `MyFavoriteBands.docx` from the user's Documents directory.

Since `.docx` files are just OOXML ZIP containers, I extracted it and went through the internal XML, mainly `word/document.xml`, `docProps/core.xml`, and `docProps/app.xml`.

The document body listed artist names: The Weeknd, Crystal Castles, Arcade Fire, Japandroids, Real Estate.

Metadata in `core.xml` and `app.xml` identified Kevin Tunes as the creator, built in Microsoft Word for Mac.

## Firefox History Analysis

The Firefox profile had three SQLite databases worth digging into: `places.sqlite`, `cookies.sqlite`, and `formhistory.sqlite`.

**Browsing History**

Joined `moz_places` and `moz_historyvisits`:

```sql
SELECT
    datetime(
        moz_historyvisits.visit_date / 1000000,
        'unixepoch',
        'localtime'
    ),
    moz_places.url
FROM moz_places, moz_historyvisits
WHERE moz_places.id = moz_historyvisits.place_id;
```

Results showed visits between roughly 3:04 PM and 3:29 PM on June 30, 2015 to Spotify, Pitchfork, and Stereogum. Other domains showed up in the same window too, but these were the ones relevant to the acceptable-use question.

**Cookie Analysis**

Queried the cookie database:

```sql
SELECT
    baseDomain,
    COUNT(*) AS cookie_count
FROM moz_cookies
GROUP BY baseDomain
ORDER BY cookie_count DESC
LIMIT 10;
```

| Domain | Cookie records |
|---|---|
| spotify.com | 42 |
| pitchfork.com | 16 |
| stereogum.com | 8 |
| bandcamp.com | 7 |

Worth noting: cookie counts show stored browser artifacts tied to those domains, they're not the same thing as an exact visit count.

**Form History**

Queried the form history database:

```sql
SELECT
    fieldname,
    value,
    timesUsed,
    datetime(firstUsed / 1000000, 'unixepoch') AS firstUsed,
    datetime(lastUsed / 1000000, 'unixepoch') AS lastUsed
FROM moz_formhistory
ORDER BY lastUsed DESC;
```

Relevant entries:

username|KevinTunes1111|1|2015-06-30 15:21:01|2015-06-30 15:21:01
searchbar-history|spotify|2|2015-06-30 15:14:38|2015-06-30 15:15:50
txtFistName|Kevin|1|2015-06-30 15:15:33|2015-06-30 15:15:33


Also found an associated company email address in the Firefox artifacts.

## Evidence Correlation

The strongest policy-related findings came from several independent artifact types lining up together: Firefox browsing history, cookie records, form history searches, and the `MyFavoriteBands.docx` document with its metadata. Together, they supported the conclusion that music-related sites and content were accessed from the workstation.

The travel-related evidence was weaker. Photo metadata established dates, devices, and locations for individual images, but couldn't independently prove conference attendance, participation in conference activities, or whether the employer paid for the travel.

## Limitations

- Cookie counts aren't the same as confirmed page-visit counts.
- A local artifact can show activity happened on a device without proving who was physically behind it.
- EXIF coordinates and timestamps can support location and time, but not conference attendance or funding.
- Some photos had no GPS coordinates at all.
- Metadata is best read alongside other evidence, not treated as standalone proof on its own.
