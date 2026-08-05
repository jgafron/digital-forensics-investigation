# Gift Card Geolocation Investigation

## Overview

This investigation focused on recovering deleted image artifacts from a forensic disk image, pulling embedded GPS metadata out of them, and using those coordinates to figure out what locations were actually relevant to the case. I visualized the findings on an interactive Folium map with clustered markers and image previews.

## Scenario

I was given a forensic disk image as part of a simulated investigation involving gift card purchases and location evidence. The goal was to recover deleted files that might matter, dig through their metadata, and see whether the geographic data could tie the recovered images to specific locations connected to the case.

## Objectives

- Preserve and examine the provided forensic image
- Recover deleted or inaccessible image files
- Identify files with embedded geolocation metadata
- Extract and validate latitude/longitude coordinates
- Plot relevant locations on an interactive evidence map
- Document the process and findings

## Tools Used

Autopsy, Foremost, PhotoRec, ExifTool, `xxd`, `dd`, Python, Folium, Linux

## Investigation Process

1. Pulled down the compressed forensic image `fsf.dd.bz2` from the course server and decompressed it to start working with it.

2. Inspected the disk layout to find the NTFS partition inside the image, then mounted it read-only so I wouldn't risk modifying the original evidence.

3. Started with manual carving, going through the image at the byte level and hunting for JPEG headers and footers to find image boundaries by hand before pulling anything out.

4. Ran Foremost for automated carving, which recovered 204 JPEG files and 137 GIF files. Most of it turned out to be noise, unrelated or nonsensical files, so I still had to go through everything manually to find what actually mattered.

5. Used PhotoRec as a second recovery pass, then combined the output with Foremost's results and ran everything through a duplicate finder to clean things up.

6. Pulled metadata out of the recovered JPEGs with ExifTool, GPS coordinates, timestamps, camera and device info, that kind of thing.

7. Split the recovered images into two groups: ones with usable GPS data and ones without. Sixteen images had valid GPS coordinates, timestamps, and device info, all traced back to an iPhone 4S and all captured in or around Oakbrook Center in Illinois.

8. Cross-referenced the storefront and gift-card photos against their timestamps and coordinates to build a picture of where the person was and when.

9. Built an interactive map in Folium, with each marker showing a recovered filename, timestamp, location, and image preview, and added clustering so nearby photos didn't overlap and bury each other.

## Key Findings

- Automated carving with Foremost recovered 204 JPEG files and 137 GIF files from the image.
- Most of that recovered material wasn't useful, it took manual review and metadata analysis to isolate anything relevant.
- 31 recovered images had no usable GPS or device metadata but visually showed gift cards or gift-card displays.
- 16 images had valid GPS coordinates, timestamps, and device metadata, all captured on an iPhone 4S in or around Oakbrook Center, Illinois.
- 12 of those geotagged photos showed storefronts around the mall, including Macy's, Gap, Nordstrom, and Abercrombie.
- 4 GPS-tagged photos specifically showed gift-card racks, captured within about a 15-minute window on June 27, 2015 (roughly 12:32 PM to 12:47 PM), all within a few hundred feet of each other near the mall.
- A separate recovered photo of a Gap storefront carried coordinates that lined up with the same Oakbrook Center location, reinforcing the pattern.
- Between the GPS data, timestamps, device info, and the storefront/gift-card imagery, a consistent location and activity pattern emerged, someone moving through a small section of the mall over a short window of time, stopping at gift-card displays along the way.
- Plotting all 16 geotagged images on the interactive map made that pattern much easier to see than scrolling through metadata as plain text ever would have.

## Evidence Visualization

The recovered photographs contained GPS metadata associated with locations around a shopping mall in Illinois. I plotted those coordinates on an interactive Folium map so the evidence could be reviewed spatially.

Each marker represents the location where a recovered photograph was taken. The clustered layout makes it easier to identify where the images were concentrated and examine the relationship between the photographs and nearby locations.

[View the interactive evidence map](https://jgafron.github.io/digital-forensics-investigation/case-studies/gift-card-geolocation/evidence-map.html)

## Skills Demonstrated

- Forensic image examination
- Deleted file recovery
- File signature analysis and carving
- EXIF metadata extraction
- GPS coordinate interpretation
- Evidence correlation
- Python-based geospatial visualization
- Technical documentation

## Academic Disclaimer

This investigation was completed as part of academic digital forensics coursework. The scenario, identities, and evidence were provided for educational purposes and don't represent a real client or active investigation.
