# Report to Access Now - Digital Security and Surveillance

**Channel:** Email to info@accessnow.org / Digital Security Helpline: help@accessnow.org
**Language:** English
**Subject:** Spanish public broadcaster deploys corporate surveillance infrastructure that intercepts all browser network traffic

---

Dear Access Now team,

## 1. Summary

I am reporting a case of **corporate surveillance deployed by a public institution**. Spain's national public broadcaster, RTVE, has embedded in its streaming platform (RTVE Play) a third-party analytics service that **intercepts all browser network communications** at the protocol level, includes **session replay capabilities**, and conditions access to publicly funded content on the user's acceptance of this surveillance infrastructure.

This case sits at the intersection of Access Now's work on **corporate accountability**, **government surveillance**, and **digital security**.

## 2. The surveillance infrastructure

### 2.1 Global network interception by Conviva

RTVE Play loads **Conviva AppTracker v1.4.3** (Conviva Inc., San Jose, California). This script performs runtime modification ("monkey-patching") of the browser's three core network APIs:

```
XMLHttpRequest.prototype.open    -> Conviva wrapper
XMLHttpRequest.prototype.send    -> Conviva wrapper
XMLHttpRequest.prototype.setRequestHeader -> Conviva wrapper
window.fetch                     -> Conviva interceptor
WebSocket constructor            -> Conviva replacement
WebSocket.prototype.send         -> Conviva wrapper
```

**Impact:** Every HTTP request, fetch call, and WebSocket message made by the browser while the RTVE Play page is open is routed through Conviva's code. This includes not just video-related traffic but potentially any network activity on the page.

Conviva's configuration includes:
- `networkRequestTrackingConfiguration`: tracking of all HTTP/fetch requests
- `exceptionAutotracking`: capture of all JavaScript exceptions
- `lifecycleAutotracking`: browser lifecycle events
- `clickcc`: user click tracking with broad CSS selectors
- **Remote configuration** downloaded from `rc.conviva.com` and cached in `localStorage`

### 2.2 Session replay capability

The Conviva code includes a **complete session replay engine** based on `rrweb` that can record:
- Mouse movements and positions
- Click events with coordinates
- Scroll behavior
- Form input changes (keystrokes in text fields)
- Full DOM mutations (page content changes)

Recordings are stored in IndexedDB (`conviva-session-record-db`), compressed with gzip (pako), and uploaded to **Google Cloud Storage**.

**Activation is remote:** Conviva's servers can enable session recording for any user segment without any code change on the RTVE website. This means the surveillance capability can be silently expanded at any time.

### 2.3 Scale of tracking

A forensic analysis of a single video page reveals **29 different domains** contacted, including:

- **Google** (advertising, analytics, tag management)
- **Adobe** (analytics, audience profiling via Audience Manager/demdex.net)
- **Conviva** (video telemetry, network interception, session replay)
- **Chartbeat** (real-time behavioral analytics)
- **VWO** (A/B testing, user behavior tracking)
- **SAP Gigya** (identity management)

All US-based services process data on servers outside the EU.

### 2.4 Telemetry without content delivery

The 24 telemetry heartbeats to Conviva (`POST appgw.conviva.com/ctp`) fire **even when the video never plays** (because the player blocks playback when it detects an ad blocker). This means RTVE collects and transmits user surveillance data without providing any service in return.

## 3. Content access conditioned on surveillance

RTVE Play **blocks video playback** when the user has an ad blocker installed. The mechanism:

1. The video player (THEOplayer) attempts to load Google's advertising SDK
2. If the SDK is blocked, THEOplayer classifies this as `AD_BLOCKER_DETECTED` (error code 10001)
3. The player refuses to play the content video
4. RTVE has deliberately configured this behavior (the player supports a fallback-to-content mode, but RTVE chose the blocking mode)

**The result:** Citizens must either accept the full surveillance infrastructure (29 tracking domains, network interception, potential session recording) or be denied access to publicly funded content.

## 4. Why this matters for Access Now's mission

### 4.1 Public institutions deploying corporate surveillance

RTVE is not a private company. It is the Spanish state broadcaster, funded by taxes. A public institution should not deploy surveillance infrastructure that intercepts all browser network communications of its citizens.

### 4.2 No meaningful consent

The consent banner (OneTrust) is itself blocked by privacy-focused browsers and extensions, creating a paradox: the system designed to ask for consent cannot function when the user employs privacy tools. Multiple tracking services load before or independently of consent.

### 4.3 Precedent for public media

If RTVE's model is accepted, it creates a precedent where **any public service can condition access on acceptance of corporate surveillance**. This directly threatens the principle that citizens should be able to access government-funded services without submitting to private tracking.

### 4.4 Chilling effect on privacy tools

Blocking content for ad-blocker users sends a message: using privacy tools has consequences. This creates a chilling effect on the adoption of legitimate security and privacy software.

## 5. Request

I request that Access Now:

1. **Evaluate this case** for inclusion in your corporate accountability research
2. **Consider technical analysis** of the Conviva AppTracker network interception (the JavaScript source code is available for review)
3. **Assess the precedent** this sets for public institutions conditioning service access on surveillance acceptance
4. **Coordinate with European partners** (EDRi network, where a parallel communication is being filed) to amplify the case
5. **Consider including this case** in RightsCon programming or related advocacy

## 6. Evidence available

- Complete technical report (architecture analysis, 157 requests to 29 domains)
- Reverse-engineered ad-blocker detection code showing deliberate blocking
- HAR file (18 MB) with full network capture
- JavaScript source files (44 files) including Conviva AppTracker

All evidence is available for independent verification.

---

[COMPLETAR - Name or pseudonym]
[COMPLETAR - Secure email]
[DATE]

---

**Note on security:** This report can be submitted through Access Now's Digital Security Helpline (help@accessnow.org) which uses encrypted communications.
