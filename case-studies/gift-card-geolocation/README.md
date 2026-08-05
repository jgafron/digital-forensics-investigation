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

1. Started by going through the forensic media to see what image artifacts were actually recoverable, and whether any deleted photos were still sitting in unallocated space.

2. Carved JPEG images out based on file signatures rather than trusting the original filesystem structure, since the files I cared about were deleted. The recovery tool assigned them generic filenames like `f0334504.jpg` and `f0340088.jpg`.

3. Went through the recovered files and checked a few of the questionable ones at the byte level to confirm file headers and boundaries, and to see if there was any more image data worth pulling out.

4. Ran ExifTool against the recovered JPEGs to see what metadata survived. The fields I cared about most were:
   - Original date and time
   - GPS latitude
   - GPS longitude
   - Camera/device info, when it was there

5. Filtered down to the photos that actually had usable GPS coordinates, then converted those into decimal lat/long values I could actually map.

6. Organized everything I had, filename, timestamp, latitude, longitude, into a single working set of evidence.

7. Built an interactive map in Python using Folium. Each recovered photo got its own marker showing:
   - The recovered filename
   - The photo's timestamp
   - Its GPS location
   - A preview of the image itself

8. Added clustering to the markers, since several photos were taken close enough together that individual pins would've overlapped and made the map harder to read.

9. Exported the finished map as a standalone HTML file so it could be opened and reviewed in any browser.

## Key Findings

- Several of the recovered JPEGs still had embedded GPS metadata intact.
- The coordinates placed the photos around a shopping mall in Oakbrook, Illinois, not just one single point.
- The locations formed a loose cluster around the mall and the surrounding properties, rather than all sitting on top of each other.
- Between the timestamps and the coordinates, I could get a rough sense of the order the photos were taken in and how the person moved between locations.
- Plotting everything spatially made the relationships between photos obvious in a way a plain metadata list never would have.
- The final map kept each photo's filename, timestamp, and preview tied directly to its marker, so nothing lost context once it was on the map.
- Altogether, it was a decent demonstration of how deleted file recovery, metadata analysis, and geospatial visualization can come together to reconstruct where and roughly when something happened.

The map includes entries like `f0334504.jpg` and `f0340088.jpg`, both carrying June 27, 2015 timestamps and distinct coordinates around the same Illinois location.

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
