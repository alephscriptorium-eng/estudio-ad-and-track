# Report to Privacy International - Corporate Surveillance and Data Exploitation

**Channel:** Email to info@privacyinternational.org or via tip form at privacyinternational.org
**Language:** English
**Subject:** Spanish state broadcaster RTVE deploys invisible corporate surveillance: network-level interception by Conviva Inc. affecting millions of users

---

Dear Privacy International,

## 1. Why this is relevant to your work

This report documents a case of **invisible corporate surveillance** deployed by a **public institution** that directly aligns with Privacy International's campaigns on:

- **Corporate surveillance:** A third-party analytics company (Conviva Inc.) intercepts all browser network communications at the API level
- **Data exploitation:** User behavioral data is extracted, compressed, and sent to US servers even when no service is delivered
- **Hidden data ecosystem:** 29 third-party domains are contacted to play a single video, creating an invisible web of corporate data flows
- **Coerced consent:** Users must accept the entire surveillance stack or be denied access to publicly funded content

## 2. The case

**RTVE** (Corporacion de Radio y Television Espanola) is Spain's national public broadcaster, funded primarily through the national budget. Its streaming platform, **RTVE Play**, serves millions of users.

A forensic technical analysis of a video playback page on RTVE Play reveals two interconnected problems:

### Problem 1: Invisible surveillance infrastructure

To play a single video, the user's browser contacts **29 different domains** operated by 12+ companies across 4 countries. The user is not meaningfully informed of this.

The most concerning component is **Conviva AppTracker v1.4.3** (Conviva Inc., San Jose, CA), which:

**a) Replaces the browser's core network APIs:**
- `XMLHttpRequest.prototype.open/send/setRequestHeader` -> Conviva wrappers
- `window.fetch` -> Conviva interceptor
- `WebSocket` constructor + `WebSocket.prototype.send` -> Conviva replacements

This is not passive observation. Conviva **replaces** the browser's built-in network functions with its own implementations. Every network request the page makes passes through Conviva's code first.

**b) Includes full session replay capability:**
- Records mouse movements, clicks, scroll, form inputs, DOM changes
- Uses the `rrweb` library
- Stores recordings in IndexedDB, compresses with gzip, uploads to Google Cloud Storage
- **Activation is controlled remotely** by Conviva's servers (not by RTVE)

**c) Operates independently of consent:**
- Conviva is loaded via Google Tag Manager (managed by marketing, not engineering)
- The consent mechanism (OneTrust) is itself blocked by privacy-focused browsers
- Conviva telemetry fires even when the user has not consumed any content

**d) Maintains persistent tracking:**
- Stores configuration in `localStorage`
- Downloads remote config from `rc.conviva.com`
- Sends 24+ compressed heartbeat POST requests to `appgw.conviva.com/ctp` per page load

### Problem 2: Coerced acceptance of surveillance

RTVE has configured its video player to **block content playback** when it detects an ad blocker. The technical mechanism:

1. Player attempts to load Google IMA SDK (advertising)
2. If blocked, player classifies failure as `ADBLOCK` (error code 10001)
3. Player refuses to play the content video
4. User sees: "Turn off your AdBlocker and try again"

The player software (THEOplayer) supports a mode where content plays without ads when ads fail. RTVE has **deliberately chosen the blocking mode** and configured a fallback to an empty file (`js2.rtve.es/js/vacio`) that guarantees the detection triggers.

**The coercion:** Users who value their privacy (and therefore use ad/tracker blockers) are denied access to publicly funded content. To watch the video, they must disable their privacy tools and accept the full surveillance infrastructure.

## 3. The hidden data ecosystem

### 3.1 Data flows map

```
User's browser
    |
    +---> RTVE servers (content, API, static files)
    |
    +---> Google (IMA SDK, Tag Manager, Cast)
    |         |
    |         +---> Google ad servers (VAST/VMAP ads)
    |         +---> Google Analytics (via GTM)
    |
    +---> Conviva Inc. (appgw.conviva.com)
    |         |
    |         +---> Network interception data
    |         +---> Session replay recordings -> Google Cloud Storage
    |         +---> Remote config <- rc.conviva.com
    |
    +---> Adobe Inc.
    |         |
    |         +---> Adobe Analytics (sadbmetrics.rtve.es)
    |         +---> Adobe Audience Manager (dpm.demdex.net) -- profiling
    |         +---> Adobe Target (mboxedge37.tt.omtrdc.net)
    |         +---> Adobe Visitor ID (cross-site tracking)
    |
    +---> SAP Gigya (identity, login)
    +---> Chartbeat Inc. (real-time analytics)
    +---> Wingify/VWO (A/B testing)
    +---> OneTrust (consent -- ironic)
    +---> SPXL/Socy (tracking pixel)
```

### 3.2 What makes this case special

- **Public institution:** This is not a private company making business decisions. It is the state broadcaster spending public money to deploy private surveillance.
- **No opt-out:** Users cannot access the content without the surveillance. There is no "lite" version, no opt-out, no alternative.
- **Invisible:** None of this is visible to the user. The surveillance operates entirely in JavaScript, invisible to the naked eye.
- **Disproportionate:** 29 domains and 157 HTTP requests for one video. The surveillance infrastructure is larger than the content delivery infrastructure.
- **US data flows:** At least 5 US-based companies receive data (Google, Adobe, Conviva, Chartbeat, OneTrust), with no clear assessment of Schrems II compliance.

## 4. Request

I request that Privacy International:

1. **Investigate the Conviva AppTracker** network interception technique. This pattern (monkey-patching browser network APIs) may be deployed across many websites beyond RTVE. A broader investigation could reveal the scale of this invisible surveillance.

2. **Assess the coercion model:** The "block content unless surveillance is accepted" pattern is spreading across media websites. Documenting and challenging this pattern early could prevent it from becoming normalized.

3. **Include this case** in your corporate surveillance research and advocacy.

4. **Coordinate with European organizations** already notified (EDRi, noyb, Access Now, EFF) to build a multi-organizational response.

## 5. Evidence

All technical evidence is available:
- Deobfuscated source code of the detection and blocking mechanism
- Full network capture (HAR file, 18 MB, 157 requests)
- 44 JavaScript source files from the page
- Two detailed technical reports

---

[COMPLETAR - Name or pseudonym]
[COMPLETAR - Email]
[DATE]
