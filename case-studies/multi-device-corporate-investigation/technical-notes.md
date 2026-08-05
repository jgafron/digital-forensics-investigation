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

Software\Microsoft\Windows\ShellNoRoam\BagMRU


Artifacts showed access to a directory named `HighQuality`, matching a directory on the USB that contained the recovered media.

Registry evidence also referenced `E:\Pics\Hidden`, suggesting removable media had been mounted and browsed, even though that directory wasn't visible in the current filesystem view anymore.

## Deleted and Unallocated Evidence

Searched Jo's workstation unallocated space and found filenames matching files recovered from the USB, including part of the `DSC00025.JPG` through `DSC00046.JPG` range.

The deleted records didn't have complete timestamps, but the matching names still supported the idea that these files had previously existed on the workstation.

## Terry and the Computer Sale

Autopsy analysis of Terry Johnson's workstation turned up a timeline tied to the sale of a Dell desktop through Craigslist.

**Internal company activity**

On November 18, Jo reached out to Terry for help with a slow workstation. On November 20, Terry said he'd replace Jo's computer and take the old machine to his own desk to diagnose it. Company leadership later instructed that the old computer be properly wiped.

**Craigslist artifacts**

Found Craigslist posting-confirmation emails, emails with a buyer named Aaron Greene, photos of a Dell desktop, Internet Explorer Craigslist history, Chrome posting-editor cache, Craigslist-hosted images, and recent-document shortcuts.

A posting confirmation titled "Dell Computer For Sale" was received on November 24, 2009. Later email correspondence showed Terry discussing a $1,000 transaction and arranging pickup with the buyer.

## Deleted Email Evidence

Recovered the Craigslist posting-confirmation email from Terry's Deleted Items folder. That selective deletion could point toward an effort to hide the posting, though deletion alone doesn't prove intent. Other buyer correspondence was still sitting in the inbox, untouched.

## Browser Cache Evidence

Recovered a Craigslist-hosted Dell image from Chrome cache:

http://images.craigslist.org/...


That cached image, along with posting-editor artifacts, supported Terry's direct interaction with the live sale listing.

## Keylogger Evidence

Found several artifacts tied to keylogging software on Terry's systems:

- `XPADVANCEDKEYLOGGER.EXE-291AEECE.pf`
- `keylogger.zip.lnk`
- `xpadvancedkeylogger.exe`

The executable also showed up on Terry's work USB. Relevant Autopsy areas: Run Programs, Recent Documents, Deleted Files, Recycle Bin, Web Downloads, Notable Items.

A recovered screenshot showed the XP Advanced Key Logger interface open on a computer with the company logo as wallpaper. Together, these artifacts supported both possession and likely execution of keylogging software.

## Additional Suspicious File

Autopsy flagged `42.zip` in Terry's personal directory and Web Downloads artifacts. Since `42.zip` is a commonly recognized ZIP bomb, I treated it as a suspicious file worth further review.

## Financial Misconduct Artifacts

Email analysis turned up:

- company approval for a hard-drive purchase
- a personal receipt showing a $100 cost
- a submitted company receipt showing $300
- an email where Terry mentioned making a quick $200
- a discussion about using that money for poker

The matching amounts and close timing supported the possibility of a falsified reimbursement.

## Charlie's Data Exfiltration

Examination of Charlie's workstation and work USB turned up emails offering confidential research to an external competitor, image attachments tied to concealed data, a ZIP archive used in an apparent extortion attempt, password instructions, references to steganography, deleted email fragments, and matching files on Charlie's USB.

Relevant files: `astronaut.jpg`, `microscope.jpg`, `01.zip`.

Together, the email and USB artifacts supported unauthorized disclosure of company information and an attempted extortion.

## Cross-Device Correlation

This investigation really depended on comparing artifacts across sources rather than treating each image in isolation. That included matching filenames between Jo's workstation and USBs, registry references to USB folder names, Terry's emails/browser cache/photos/shortcuts, keylogger artifacts on both Terry's workstation and USB, Charlie's email attachments matched against his USB files, and internal company emails explaining custody of the Dell computer.

## Interpretation

The strongest, best-supported conclusions:

- Jo's workstation and USBs contained related media artifacts.
- Terry took custody of Jo's Dell computer and later arranged its sale through Craigslist.
- Terry possessed and likely executed keylogging software.
- Terry may have submitted an inflated reimbursement request.
- Charlie transmitted confidential material externally and used concealed files in an extortion attempt.

Some conclusions are still inferential rather than proven outright:

- A deleted email may suggest concealment, but it doesn't prove intent on its own.
- Device artifacts don't independently identify who was physically at the keyboard at every moment.
- Matching filenames support correlation, but they don't alone establish original authorship.

## Skills Demonstrated

- Autopsy multi-image case management
- E01 evidence handling
- SHA-512 validation
- Registry analysis
- Deleted file recovery
- Unallocated space review
- USB correlation
- Email analysis
- Browser cache analysis
- Prefetch and execution artifact analysis
- Suspicious file identification
- Cross-user and cross-device timeline reconstruction
- Evidence-based limitation statements

## Academic Disclaimer

This investigation used a simulated educational evidence set. The identities, organization, allegations, and artifacts don't represent a real client or active investigation.
