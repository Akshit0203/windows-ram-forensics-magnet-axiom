# 01 — Case Overview

## Scenario

A simulated **post-breach / insider-threat** investigation on a live Windows 11 workstation. The investigative premise is a **data exfiltration / IP theft** case in which the responder must preserve volatile system state *before* shutdown, because the most valuable evidence — active browser sessions, decrypted content, cached media, and network artifacts — exists only in RAM and is lost on power-off.

## Why volatile memory first?

In the **order of volatility** (RFC 3227), memory ranks above disk. RAM contains:

- Active and recently closed **browser sessions** and history not yet flushed to disk
- **Decrypted** data that is encrypted at rest
- **Cached media** (images, thumbnails) and document fragments
- **Network artifacts** — URLs, redirect chains, cloud endpoints
- Remnants of **multiple user contexts** sharing the same physical host

Capturing memory first preserves this transient evidence for post-mortem analysis.

## Scope

| In scope | Out of scope |
|---|---|
| Acquisition of live physical memory | Full disk imaging |
| Sector-level carving of the raw image | Network packet capture |
| Reconstruction of web / cloud / geo / social activity | Live malware detonation |
| Recovery of in-memory media & documents | Mobile / cloud evidence sources |
| Identification of in-memory IoCs | Attribution beyond on-host artifacts |

## Case parameters

| Field | Value |
|---|---|
| Case number | 1 |
| Case type | Data exfiltration / IP theft |
| Examiner | Akshit |
| Scan description | RAM Forensic |
| Scan date | 25-May-2025 |
| Evidence item | `forensic.raw` |
| Platform | Computer (Windows) |

## Assumptions & constraints

- The host was **powered on and unlocked** at acquisition time (required for live capture).
- The memory image presents **no recognizable file system**, which is expected for a raw RAM dump — hence a **sector-level** search strategy.
- The examiner has **lawful authority** over the host (author-owned lab system).

## Legal & ethical framing

All activity was performed on author-owned hardware for CHFI skills development. Sensitive values in all exhibits are redacted; malicious indicators are defanged. See the [Disclaimer](../README.md#-disclaimer).
