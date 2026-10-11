# 05 — Indicators of Compromise (IoCs)

During analysis, AXIOM Examine classified **2 artifacts** under **Refined Results → Malware/Phishing URLs**. Both were resident in volatile browser memory with no guaranteed disk footprint, illustrating the value of memory forensics for IoC discovery.

> ⚠️ **All indicators below are defanged.** Do **not** navigate to, resolve, or interact with them. They are documented for forensic and defensive purposes only.

![AXIOM Examine Malware/Phishing URLs filter showing two memory-resident suspicious URLs](../assets/screenshots/25-malware-phishing-urls.png)
*Source exhibit — two URLs classified under Refined Results → Malware/Phishing URLs, recovered from volatile browser memory.*

## Indicator table

| # | Defanged indicator | Type | Original artifact | Observation |
|---|---|---|---|---|
| 1 | `hxxp://rep4.upseek[.]org/?r2=launc1\|hxxp://` | Redirect / suspicious URL | Potential Browser Activity (Physical Sector 1350822) | Stacked/chained URL containing an embedded redirect (`?r2=launc1`) followed by a second `http://` — a pattern consistent with redirect chains and ad/malware redirectors |
| 2 | `hxxp://fewfwe[.]net/` | Suspicious / low-reputation domain | Potential Browser Activity | Short, random-looking domain with no legitimate service context, present in volatile browser activity |

## Why these matter

- **Redirect chains** (`rep4.upseek[.]org/?r2=...`) are commonly used to shuttle victims through intermediary hosts toward phishing or malware-delivery endpoints while evading simple blocklists.
- **Low-reputation throwaway domains** (`fewfwe[.]net`) are typical of staging, redirection, or short-lived campaign infrastructure.
- Their presence **in memory** means they were loaded or referenced during the active session — valuable even if the corresponding disk cache was cleared.

## Analytical caveats (honest assessment)

- Classification is **AXIOM's automated categorization**; it indicates *suspicion*, not confirmed compromise.
- Memory presence alone does **not** prove execution or successful compromise — a URL can be present via an ad frame, redirect, or incidental reference.
- Confirmation would require **threat-intel enrichment** (reputation lookups on VirusTotal / URLhaus / etc.), correlation with disk and network evidence, and timeline analysis. Those steps are **out of scope** for this lab but are the correct next actions in real casework.

## Recommended follow-up actions

1. **Enrich** both indicators against threat-intel platforms (VirusTotal, URLhaus, AbuseIPDB) in a sandboxed/isolated environment.
2. **Correlate** indicator timestamps with the browser-activity timeline (docs 04) to establish the entry vector.
3. **Hunt** for the domains/patterns across proxy, DNS, and firewall logs for the same host and others.
4. **Hash and preserve** any associated carved media/documents as potential payloads.
5. **Block** the indicators at the egress/DNS layer as a precautionary containment step.

## Defensive summary

This exercise shows that **volatile memory is a first-class IoC source**. A disk-only or log-only investigation could miss redirect/phishing URLs that exist primarily in RAM. Capturing memory early — and carving it at sector level — expands IoC visibility in insider-threat and post-breach scenarios.

> ⬅️ Back to [Findings & Analysis](04-findings-and-analysis.md) · [Forensic Report](../reports/forensic-report.md)
