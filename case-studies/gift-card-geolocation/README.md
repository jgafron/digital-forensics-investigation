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

1. Reviewed the forensic image's filesystem structure and looked for recoverable artifacts.
2. Ran file-carving tools to recover deleted images based on file signatures.
3. Inspected the recovered files and checked file headers where needed to confirm what I was actually looking at.
4. Pulled metadata, including GPS coordinates, out of the recovered photos using ExifTool.
5. Reviewed the coordinates for relevance and converted them into map-ready location data.
6. Built an interactive Folium map with clustered markers and embedded image previews.

## Key Findings

- Recovered deleted image artifacts from the forensic media
- Identified photos containing embedded geographic coordinates
- Connected recovered images to physical locations relevant to the scenario
- Built an interactive visualization that made the geographic evidence much easier to review

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
