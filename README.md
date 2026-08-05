# Digital Forensics Investigations

A collection of academic digital-forensics case studies covering disk image analysis, deleted-file recovery, timeline reconstruction, browser and registry artifacts, geolocation, network log correlation, and multi-device investigations.

## About This Repository

These are selected investigations from my digital forensics coursework. Each one includes a short summary, the methodology and tools I used, key findings, and a technical notes file with the actual commands and artifact details, plus a visualization where one made sense.

The scenarios, identities, and evidence were all provided for coursework, not real cases.

## Case Studies

### [Gift Card Geolocation Investigation](./case-studies/gift-card-geolocation/)

Recovered deleted image artifacts, pulled GPS metadata out of them with ExifTool, and plotted sixteen geotagged photos around Oakbrook Center on an interactive Folium map.

**Focus:** File carving, EXIF analysis, geolocation, Python, Folium

[View the interactive evidence map](https://jgafron.github.io/digital-forensics-investigation/case-studies/gift-card-geolocation/evidence-map.html)

---

### [Document Attribution and Timeline Reconstruction](./case-studies/document-attribution-timeline/)

Recovered a deleted Word document and correlated metadata, shortcut files, RecentDocs, MRU entries, browser cache, and NTFS journal activity across two devices to figure out who created it and who accessed it.

**Focus:** Sleuth Kit, Plaso, timeline analysis, registry artifacts, deleted-file recovery

---

### [Workstation Policy Investigation](./case-studies/workstation-policy-investigation/)

Investigated an employee workstation for acceptable-use policy violations using browser history, cookies, form history, OOXML metadata, PDF artifacts, and photo geolocation data.

**Focus:** Firefox SQLite analysis, EXIF metadata, OOXML, forensic image handling

---

### [Network Log Analysis](./case-studies/network-log-analysis/)

Correlated RADIUS and DHCP logs to identify devices, MAC addresses, IP assignments, access points, and suspicious use of a missing MacBook Pro.

**Focus:** RADIUS, DHCP, `grep`, `awk`, device and authentication correlation

---

### [Multi-Device Corporate Investigation](./case-studies/multi-device-corporate-investigation/)

Used Autopsy as the primary platform to examine multiple workstation and USB images, correlating deleted media, registry evidence, email, browser cache, suspicious software, and an apparent unauthorized sale of company equipment.

**Focus:** Autopsy, E01 evidence, USB correlation, email and browser artifacts, multi-device analysis

## Tools and Technologies

Autopsy, Sleuth Kit, Plaso / log2timeline, libewf / `ewfexport`, ExifTool, Foremost, PhotoRec, SQLite, Folium, Python, `dc3dd`, `grep`, `awk`, Linux

## Repository Structure

```text
case-studies/
├── gift-card-geolocation/
├── document-attribution-timeline/
├── workstation-policy-investigation/
├── network-log-analysis/
└── multi-device-corporate-investigation/

original-coursework/
reports/
assets/
```

## Skills Demonstrated

- Forensic evidence validation
- Disk image and E01 analysis
- Deleted file recovery
- Filesystem and super-timeline reconstruction
- Browser, registry, and email artifact analysis
- EXIF and geospatial analysis
- RADIUS and DHCP log correlation
- USB and workstation correlation
- Cross-device investigation
- Technical reporting
- Evidence-based conclusions and limitations

## Academic Disclaimer

These investigations were completed as part of academic digital forensics coursework. The scenarios, organizations, identities, and evidence were provided for educational purposes and don't represent real clients or active investigations.
