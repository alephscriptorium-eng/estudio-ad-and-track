# Report to the Electronic Frontier Foundation (EFF)

**Recipient:** Electronic Frontier Foundation
**Headquarters:** 815 Eddy Street, San Francisco, CA 94109, USA
**Contact:** info@eff.org / https://www.eff.org/about/contact
**Language:** English
**Cost:** Free

---

## Spain's Public Broadcaster Blocks Content for Ad Blocker Users While Conducting Mass Surveillance

### Executive Summary

RTVE, Spain's public television broadcaster (funded by the State Budget, equivalent to PBS but with mandatory universal access obligations), blocks video playback on its streaming platform RTVE Play for any user running an ad blocker. A forensic technical analysis reveals that the platform simultaneously conducts mass surveillance of its users through 29 third-party domains -- including US-based Google, Adobe, Conviva, and Chartbeat -- while a telemetry service intercepts all browser network communications and includes the capability for full session replay recording.

This case sits at the intersection of several issues central to EFF's mission: the right to use ad blockers, browser-level surveillance by websites, and the dependency of public institutions on US surveillance-capable infrastructure.

---

### 1. The Facts

#### 1.1 Content blocking

RTVE uses the commercial video player THEOplayer 9.4.1 with the Google IMA SDK advertising integration. The system works as follows:

1. Before playing any content, the player attempts to load Google's advertising system.
2. If the ad system is unavailable (e.g., because the user has an ad blocker), the player blocks content playback entirely.
3. The player displays the message "Turn off your AdBlocker and try again" -- notably in English, not Spanish, on a Spanish public service website.
4. THEOplayer offers a configuration option to play content without ads when advertising fails. RTVE explicitly chose the blocking option.
5. RTVE also configured a deliberate empty fallback file (js2.rtve.es/js/vacio) that ensures playback never proceeds when ads fail.

This is not a technical limitation -- it is a deliberate policy choice to deny citizens access to publicly funded content unless they accept advertising served by Google.

#### 1.2 Global network interception by Conviva

The Conviva AppTracker v1.4.3 library, loaded via Google Tag Manager, performs a systematic interception of all browser network APIs through monkey-patching:

- XMLHttpRequest.prototype.open: replaced by Conviva interceptor
- XMLHttpRequest.prototype.send: replaced by Conviva interceptor
- XMLHttpRequest.prototype.setRequestHeader: replaced by Conviva interceptor
- window.fetch: replaced by Conviva interceptor
- WebSocket constructor: replaced by Conviva interceptor
- WebSocket.prototype.send: replaced by Conviva interceptor

This means that every network request made by the browser while the RTVE Play page is open -- including requests to other domains and services -- is potentially monitored by Conviva and transmitted to US servers (appgw.conviva.com).

This is not a standard analytics technique. It is a browser-level man-in-the-middle interception applied silently to every visitor of a public service website.

#### 1.3 Full session replay capability

Conviva AppTracker includes a complete session recording module based on the open-source rrweb library. This module can capture:

- All mouse movements
- Every click
- Scroll events
- Form field changes (including text inputs)
- All DOM mutations (every change to the page)

The recordings are stored locally in IndexedDB (conviva-session-record-db), compressed with gzip, and uploaded to Google Cloud Storage. Activation is controlled remotely via configuration downloaded from rc.conviva.com, meaning the session recording can be enabled without any code change to the website and without the user's knowledge.

#### 1.4 Scale of tracking

To play a single video on Spain's public broadcaster, the user's browser contacts 29 different domains and makes 157 HTTP requests. These include:

- Google (IMA SDK, Tag Manager, Analytics) -- advertising, tag management
- Adobe (Audience Manager, VisitorAPI) -- visitor identification, audience profiling
- Conviva (AppTracker) -- telemetry, network interception, session replay
- Chartbeat -- real-time behaviour analytics
- VWO / Wingify -- A/B testing, user behaviour
- OneTrust -- consent management (which itself is blocked by privacy extensions, creating a consent paradox)

#### 1.5 Telemetry without content delivery

The 24 Conviva heartbeat POST requests to conviva.com/ctp are sent even when the video never plays -- because the player blocks playback after detecting the ad blocker. This means RTVE collects and transmits telemetry about the user without ever delivering any content in return.

---

### 2. Why This Matters for EFF

#### 2.1 The right to use ad blockers

EFF has long defended the right to use ad blockers as a legitimate tool for privacy, security, and user autonomy. This case represents a public institution -- not a private company -- punishing citizens for exercising this right. If a state-funded broadcaster can deny access to public content based on browser configuration, it establishes a precedent that could extend to other public services.

#### 2.2 Browser-level surveillance

Conviva's monkey-patching of XHR, fetch, and WebSocket at the prototype level is an unusually aggressive surveillance technique. It goes beyond standard tracking by establishing a programmatic capability to monitor all network traffic from the page. This technique deserves wider scrutiny from the security and privacy research community.

#### 2.3 US infrastructure dependency

The practical consequence is that a European public broadcaster's content delivery depends on the availability and goodwill of US companies:
- Google (Mountain View): If Google's IMA SDK servers are unreachable, Spanish public television doesn't play.
- Conviva (San Jose): All viewing telemetry goes to US servers.
- Adobe (San Jose): User profiling goes to US servers.

This represents a form of digital colonialism where European public services cannot function without US corporate infrastructure.

#### 2.4 Broken consent model

The entire consent mechanism (managed by OneTrust, also a US company) is blocked by the same privacy tools that trigger the content block. Citizens using privacy-protective tools are caught in a paradox: they can't consent because the consent mechanism is blocked, but they also can't access content because the advertising is blocked. Meanwhile, tracking proceeds regardless.

---

### 3. Technical Evidence

The following evidence package is available:

1. Full technical report: Analysis of the complete loading chain, inventory of all 29 third-party domains, detailed Conviva AppTracker analysis including decompiled interception code.
2. Ad blocker detection analysis: Deobfuscated THEOplayer source code showing the detection mechanism and deliberate blocking configuration.
3. HAR capture: Complete network traffic capture of a single video page load (157 requests, 29 domains). Machine-readable, independently verifiable.
4. Browser console logs: Error logs showing blocked requests and the ad blocker detection message.
5. Source code archive: 44 JavaScript files downloaded directly from the page as primary verifiable evidence.

All evidence has been preserved with timestamps and can be independently reproduced by visiting any video page on rtve.es/play with an ad blocker enabled.

---

### 4. Actions Taken

This case is being pursued through multiple channels simultaneously:

- National (Spain): Complaint to RTVE Viewer's Ombudsman
- National (Spain): Freedom of information request to RTVE
- National (Spain): GDPR complaint to Spanish DPA (AEPD)
- National (Spain): Audiovisual regulation complaint to CNMC
- National (Spain): Parliamentary questions to Spanish Parliament
- European: GDPR complaint to noyb (Vienna)
- European: Cross-border cooperation request to EDPB (Brussels)
- European: Support and coordination request to EDRi (Brussels)
- European: EU regulation complaint to DG CONNECT (Brussels)
- International: This report to EFF (San Francisco)

---

### 5. What We Ask of EFF

1. **Public awareness:** Consider covering this case on EFF's blog or social media. A European public broadcaster blocking content for ad blocker users while conducting mass surveillance has high public interest value.

2. **Technical analysis:** If EFF's technologists have interest, the Conviva monkey-patching / session replay technique deserves independent security analysis. The technique of intercepting all network APIs at the prototype level from a third-party analytics script is concerning beyond this specific case.

3. **Pressure on US providers:** Google, Adobe, and Conviva are US companies subject to US jurisdiction. EFF is uniquely positioned to question these companies about their role in enabling content blocking and mass surveillance on public service websites.

4. **Ad blocker rights advocacy:** This case provides concrete evidence for the argument that the right to use ad blockers must be protected, particularly when public services are involved.

---

### Contact

- Name: [COMPLETAR]
- Email: [COMPLETAR]
- Country: Spain
- Date: [DATE]

---

*When a public broadcaster treats its citizens as adversaries for protecting their own privacy, something has gone fundamentally wrong with the relationship between public services and the public they serve.*
