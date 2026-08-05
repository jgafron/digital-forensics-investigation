# Gift Card Geolocation Investigation — Technical Notes

## Evidence Source

The investigation worked from a compressed forensic image:

```text
fsf.dd.bz2
```

Pulled from the course server, decompressed, and examined inside a Linux forensic environment.

## Partition and Filesystem Examination

Checked the disk layout with `mmls` and `kpartx` to find the NTFS partition inside the image, then mounted it read-only with `ntfs-3g` so nothing on the source evidence could get accidentally modified.

## Manual JPEG Carving

Went through the image at the byte level with `xxd`, looking for JPEG file signatures:

- Header: `FFD8FFE0`
- Footer: `FFD9`

Converted the relevant offsets from hex to decimal and used `dd` to pull out candidate image segments by hand before moving to automated tools.

## Automated File Carving

Ran Foremost:

```bash
foremost -T -t jpg,gif,pdf -i fsf.dd
```

Recovered:
- 204 JPEG files
- 137 GIF files

Also ran PhotoRec as a second automated pass and combined its output with Foremost's results.

## Duplicate Detection

Used `findimagedupes` to flag and clear out duplicate images from the combined carving output.

## Metadata Extraction

Ran ExifTool against the recovered JPEGs, pulling:

- GPS Latitude
- GPS Longitude
- Date/Time Original
- Camera Make
- Camera Model
- Image dimensions

Sixteen images came back with valid GPS coordinates and device metadata, all associated with an iPhone 4S and locations in or around Oakbrook Center, Illinois.

## Relevant Evidence Summary

**GPS-tagged gift-card photographs**

| Filename | Timestamp | Description |
|---|---|---|
| `f0358224.jpg` | 2015-06-27 12:32:24 | Gift-card rack |
| `f0362384.jpg` | 2015-06-27 12:33:53 | Amazon, iTunes, and Visa gift cards |
| `f0366776.jpg` | 2015-06-27 12:38:50.608 | iTunes gift cards |
| `f0370328.jpg` | 2015-06-27 12:47:23 | Additional gift-card racks |

**Other geotagged photographs**

The rest of the GPS-tagged photos showed storefronts and areas around Oakbrook Center, including Macy's, Gap, Nordstrom, and Abercrombie.

**Files without GPS metadata**

31 additional images had no usable GPS or system metadata but still visually depicted gift cards or gift-card displays.

## Geospatial Visualization

Used Python and Folium to build an interactive HTML map. Each marker shows:

- Recovered filename
- Image timestamp
- GPS location
- Embedded thumbnail

Added marker clustering to keep nearby evidence locations from overlapping.

[View the interactive evidence map](https://jgafron.github.io/digital-forensics-investigation/case-studies/gift-card-geolocation/evidence-map.html)

## Limitations

- A lot of the carved output was unrelated, corrupted, duplicated, or just missing useful metadata.
- Images without GPS data could still be visually tied to the investigation, but couldn't independently pin down a location.
- Exact command syntax for a couple of the manual carving steps wasn't preserved in the original notes, so it's left out here rather than reconstructed after the fact.
