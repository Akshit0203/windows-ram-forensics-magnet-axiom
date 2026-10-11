# Forensic Investigation Report — Volatile Memory Analysis

| | |
|---|---|
| **Report title** | Live RAM Forensic Analysis of a Windows 11 Host |
| **Case number** | 1 |
| **Case type** | Data exfiltration / IP theft |
| **Examiner** | Akshit |
| **Evidence item** | `forensic.raw` (live physical memory image) |
| **Acquisition date** | 25-May-2025 |
| **Analysis date** | 25-May-2025 |
| **Tools** | Magnet RAM Capture v1.2.0; Magnet AXIOM Process/Examine v5.4.0.26185 |
| **Classification** | Educational / Lab (CHFI preparation) |

---

## 1. Executive summary

A live memory image (~18.75 GB) was acquired from an active Windows 11 x64 workstation and processed at sector level with Magnet AXIOM. Processing completed in **12 minutes 3 seconds** with **zero exceptions**, recovering **7,722 artifacts**. Analysis reconstructed extensive web activity (5,067 web-related artifacts), cloud-storage access, geolocation queries, social-media activity, recovered media, and **two memory-resident malware/phishing URLs**. The exercise demonstrates forensically sound volatile-data acquisition and multi-category artifact reconstruction from a single RAM source.

## 2. Objective

Preserve and analyze volatile memory from a live host to reconstruct user activity and identify in-memory indicators of compromise, following a repeatable, CHFI-aligned methodology.

## 3. Evidence summary

| Attribute | Value |
|---|---|
| Source | Live physical memory, Windows 11 Home Single Language x64 |
| Reported memory | 19,196 MB (~19 GB) |
| Image file | `forensic.raw` (raw/flat, single segment) |
| Image size | ~18.75 GB |
| File system | None detected (raw physical media) |
| Search type | Sector level |

## 4. Methodology

1. **Acquisition** — Magnet RAM Capture imaged 100% of physical memory to `forensic.raw` (segment size: *Don't Split*).
2. **Case setup** — AXIOM Process Case #1, type *Data exfiltration / IP theft*.
3. **Ingest** — Added image as Computer → Windows → Image; AXIOM reported no known file system.
4. **Configuration** — Sector-level search; 221 of 242 computer artifact categories enabled; hashing, OCR, Magnet.AI categorization, and Dynamic App Finder enabled.
5. **Processing** — Completed in 12m 03s, 0 exceptions.
6. **Examination** — Category-by-category triage in AXIOM Examine.
7. **IoC review** — Isolation and documentation of suspicious indicators.

*Full procedural detail: [`/docs`](../docs).*

## 5. Findings

### 5.1 Artifact totals

| Category | Count |
|---|---:|
| Total artifacts | **7,722** |
| Web Related | 5,067 |
| Refined Results | 1,186 |
| Operating System | 1,059 |
| Media | 296 |
| Location & Travel | 101 |
| Custom | 10 |

### 5.2 Key reconstructions

- **Web (5,067):** 2,599 carved WebKit history + 2,371 potential browser activity artifacts; 184 classified URLs incl. Amazon and AWS console sessions; 399 Google searches.
- **Cloud (14):** Google Drive folder/file access and OneDrive endpoints — relevant to exfiltration channels.
- **Geolocation (79):** Google Maps queries with coordinates resolving to New Delhi (110011) addresses.
- **Social (348):** Predominantly LinkedIn job-search and profile activity, keyword `cyber security`.
- **Media (296):** 286 pictures, 7 carved audio, 2 videos, 1 Photoshop file — carved from unallocated sectors, hashed MD5/SHA-1.
- **Other:** 48 device identifiers, 23 locally accessed files/folders, 7 web-chat URLs, 5 people identifiers, 2 documents, 1 cloud-storage artifact.

### 5.3 Indicators of compromise

| # | Defanged indicator | Type |
|---|---|---|
| 1 | `hxxp://rep4.upseek[.]org/?r2=launc1\|hxxp://` | Redirect / suspicious |
| 2 | `hxxp://fewfwe[.]net/` | Suspicious domain |

*Detail and caveats: [05 — Indicators of Compromise](../docs/05-indicators-of-compromise.md).*

## 6. Analysis & interpretation

The volume of carved browser artifacts confirms that RAM retains user activity long after sessions end, with timestamps spanning March–May 2025. Cloud and social evidence provide exfiltration-relevant context, while geolocation data anchors the session to a physical area. Two suspicious URLs present in memory represent potential IoCs warranting enrichment and correlation. Collectively, the evidence would be materially incomplete in a disk-only examination.

## 7. Conclusion

Volatile memory forensics successfully reconstructed a comprehensive picture of on-host user activity and surfaced in-memory IoCs from a single raw image. The methodology was repeatable, documented, and completed without processing exceptions, satisfying the stated objectives.

## 8. Recommendations

- Enrich and correlate the two IoCs across threat-intel and host/network logs.
- In production casework, hash the image at acquisition and maintain a verified working copy.
- Preserve carved media/documents as potential payloads pending IoC confirmation.

## 9. Limitations

- Automated classifications indicate suspicion, not confirmed malice.
- No network capture, disk image, or external threat-intel enrichment was performed (out of lab scope).
- Analysis is bounded to on-host artifacts; no attribution is asserted.

## 10. Exhibits

All 27 evidence screenshots are in [`/assets/screenshots`](../assets/screenshots), numbered in workflow order (01 acquisition → 27 image file).

---

*Prepared for educational/CHFI purposes. Sensitive data redacted; malicious indicators defanged.*
