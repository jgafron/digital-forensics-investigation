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
