# Multi-Device Corporate Investigation — Technical Notes

## Evidence Sources

Worked from multiple forensic images and supporting reports from the simulated M57 Patents case.

Primary evidence:

- `jo-2009-12-11-002.E01`
- `jo-work-usb-2009-12-11.E01`
- `jo-favorites-usb-2009-12-11.E01`
- `terry-2009-12-11-002.E01`
- `terry-work-usb-2009-12-11.E01`
- `charlie-2009-12-11.E01`
- `charlie-work-usb-2009-12-11.E01`
- `pat-2009-12-11.E01`
- detective reports
- affidavit and warrant documentation

## Evidence Integrity

The evidence was transferred using SCP.

Calculated SHA-512 hashes for the forensic images and supporting documents:

```bash
sha512sum <filename>
```

The calculated values matched the supplied checksums for the major evidence files and reports.

One exception: the checksum calculated for `checksums.txt` itself didn't match the value listed inside that file. The underlying evidence images and reports still matched their supplied values, so this didn't affect the integrity of the actual evidence.

## E01 Conversion

Jo Smith's workstation and USB images came in Expert Witness Format.

Converted them to raw format with `ewfexport` from the libewf toolset:

```bash
ewfexport -t <output-name> -f raw <input-image.E01>
```

That produced raw working images for Jo's workstation, work USB, and personal/Favorites USB.

Briefly reviewed the raw images with tools like `mmls` before mounting them for closer examination.

## Autopsy Workflow

Autopsy was the primary platform for this investigation.

Added multiple evidence images as data sources so I could compare artifacts across users, workstations, USB drives, allocated files, deleted files, unallocated space, registry records, email stores, and browser artifacts.

Relevant Autopsy areas I worked through: Deleted Files, File Views, Data Artifacts, Registry, Recent Documents, Web History, Web Downloads, Browser Cache, Run Programs, Recycle Bin, Notable Items, Email Messages, and Unallocated Space.

## Jo Smith Device Correlation

Jo's workstation, work USB, and Favorites USB all had overlapping media filenames:

- `DSC00003.JPG` through `DSC00084.JPG`
- `Cat.mov`
- `Cat.MOV`
- `KittyMontage.mov`
- `MontereyKitty.m4v`
- `MontereyKittyHQ.m4v`
- `TiggerTheCat.m4v`

The USB images had recoverable media, and matching filenames showed up on Jo's workstation too, even where the actual file contents had already been deleted. That supported the idea that related files had existed on both the workstation and the removable media at some point.

## Media Metadata

Images in the HighQuality folder were tied to a Sony HDR-SR10 camera.

Original creation dates ran from roughly November 5 to November 11, 2009. A lot of the files shared access dates of November 24, 2009, which suggests they were copied to or accessed on the USB around that date.

Video files had creation timestamps around November 20, 2009, with similar access dates.

A cluster of dot-underscore metadata files was created in a short window, which points toward a possible bulk transfer.

## Registry Evidence

Registry analysis in Autopsy showed shell-folder activity under Jo's user profile, specifically at:
