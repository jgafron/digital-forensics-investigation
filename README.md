# Digital Forensics Investigations

A collection of academic forensics case studies I put together during my computer science coursework: disk analysis, artifact recovery, timeline reconstruction, geolocation, and network log correlation.

## About This Repository

These are selected investigations from coursework, each one focused on a different piece of the forensic puzzle. Some involve digging through filesystem artifacts and deleted files, others lean on browser history, registry data, or metadata, and a couple pull network logs and multiple devices together to build a fuller picture. All scenarios and evidence were provided for coursework, not real cases.

## Case Studies

**Gift Card Geolocation Investigation**
Recovered image artifacts, pulled geolocation metadata out of them, and built an interactive map to visualize where the evidence pointed.

**Document Attribution and Timeline Reconstruction**
Recovered a deleted document and pieced together filesystem, MRU, and browser artifacts to reconstruct what the user actually did and when.

**Workstation Policy Investigation**
Went through browser history, document metadata, and filesystem evidence on a workstation image to investigate a possible policy violation.

**Network Log Analysis**
Cross-referenced DHCP, RADIUS, MAC/IP addresses, and wireless access point data to track down device activity across a network.

**Multi-Device Corporate Investigation**
Worked across several forensic images, correlating workstation, USB, deleted file, and secondary market evidence to connect activity across multiple devices.

## Tools and Technologies

Autopsy, Sleuth Kit, Plaso / log2timeline, libewf, ExifTool, dc3dd, Foremost, PhotoRec, SQLite, Folium, Linux

## Repository Structure

```text
case-studies/       polished investigation writeups
reports/             selected final reports
assets/              images, maps, diagrams
original-coursework/ original academic submissions
```
