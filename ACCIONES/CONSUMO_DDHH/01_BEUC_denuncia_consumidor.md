# Complaint to BEUC - The European Consumer Organisation

**Channel:** Email to members@beuc.eu or via national member (FACUA/OCU in Spain)
**Language:** English
**Subject:** Public broadcaster RTVE blocks access to taxpayer-funded content unless users accept third-party advertising surveillance

---

Dear BEUC,

## 1. Summary

I am writing to report a consumer rights issue affecting millions of Spanish citizens. Spain's public broadcaster, **Corporacion RTVE**, blocks access to video content on its streaming platform **RTVE Play** when users have an ad blocker installed in their browser.

RTVE is funded primarily through the Spanish national budget (taxpayer money). Citizens who have already paid for this content through their taxes are denied access unless they agree to:

- Loading advertising served by **Google** (via Google IMA SDK)
- Being tracked by **29 different third-party domains** including US-based companies (Google, Adobe, Conviva)
- Having their browser's network communications **globally intercepted** by a third-party analytics service (Conviva AppTracker)

This amounts to conditioning access to a publicly funded service on the acceptance of commercial surveillance by private companies.

## 2. Technical evidence

A forensic analysis of the RTVE Play video playback page reveals:

### 2.1 Deliberate content blocking

- RTVE uses the commercial video player **THEOplayer 9.4.1**
- THEOplayer offers the option to play content without ads when ads fail to load (`blockContentIfAdError: false`)
- **RTVE has deliberately chosen to block content** when ads cannot load
- RTVE has configured a **fallback to an empty file** (`js2.rtve.es/js/vacio`) that guarantees the ad-blocker detection triggers when the Google IMA SDK is blocked
- The player displays an error message in English (not even localized to Spanish): "Turn off your AdBlocker and try again"

### 2.2 Disproportionate tracking infrastructure

To play a single video, the page contacts **29 different domains**:

| Category | Services | Country |
|----------|----------|---------|
| Advertising | Google IMA SDK, SpringServe | USA |
| Analytics | Adobe Analytics, Chartbeat, Conviva | USA |
| Profiling | Adobe Audience Manager (demdex.net) | USA |
| A/B Testing | Visual Website Optimizer (VWO) | India |
| Identity | SAP Gigya | Germany/USA |
| Consent | OneTrust | USA |
| Tracking pixels | SPXL/Socy | Spain |

### 2.3 Network-level surveillance by Conviva

The Conviva AppTracker v1.4.3 service performs **monkey-patching** of the browser's core network APIs:
- Replaces `XMLHttpRequest.prototype.open/send/setRequestHeader`
- Replaces `window.fetch`
- Replaces the `WebSocket` constructor and `WebSocket.prototype.send`

This means ALL network communications from the user's browser are intercepted and processed by Conviva (a US-based company), not just video-related traffic.

The Conviva code also includes a **complete session replay module** (based on rrweb) capable of recording mouse movements, clicks, scroll, form input changes, and DOM mutations. Activation is controlled remotely by Conviva's servers.

### 2.4 Pay-to-access incentive

The advertising tag includes a parameter `PLAY_PLUS` (0 for free users, 1 for paying subscribers). Paying subscribers of RTVE Play+ receive different (reduced) advertising. Combined with the ad-blocker blocking, this creates a de facto paywall for publicly funded content:

- Free user + no ad blocker = content with heavy advertising and tracking
- Free user + ad blocker = **no access at all**
- Paying subscriber = content with reduced advertising

## 3. Consumer rights implications

### 3.1 Access to publicly funded services

Citizens fund RTVE through general taxation. Denying access to this content based on browser configuration (ad blocker installed) is equivalent to denying access to a public service based on the tools a citizen uses. Ad blockers are legal software used for legitimate purposes (security, privacy, bandwidth saving, accessibility).

### 3.2 Unfair commercial practice

The combination of blocking free access while offering paid access with fewer ads constitutes an unfair commercial practice. It leverages a publicly funded service to drive revenue for a private subscription product (Play+) and for third-party advertising networks (Google).

### 3.3 Lack of transparency

Users are not clearly informed before accessing RTVE Play that:
- 29 domains will track their activity
- Their browser's network communications will be globally intercepted
- Session recording capability exists
- Access is conditional on accepting third-party advertising

### 3.4 Directive (EU) 2020/1828 - Representative actions

This issue affects all Spanish citizens (and EU residents) who use ad-blocking software and attempt to access RTVE Play. It could be suitable for a **representative action** under Directive (EU) 2020/1828 through BEUC member organizations FACUA or OCU in Spain.

## 4. Request

I request that BEUC:

1. Assess whether RTVE's practices constitute an unfair commercial practice under Directive 2005/29/EC
2. Coordinate with Spanish member organizations (FACUA, OCU) to evaluate the case
3. Consider whether a representative action under Directive (EU) 2020/1828 is appropriate
4. Include this case in BEUC's monitoring of digital consumer rights issues

## 5. Evidence

The following technical documentation is available:
- Full technical report analyzing the 157 network requests and 29 domains
- Reverse-engineered ad-blocker detection code from THEOplayer
- HAR file (HTTP Archive) of the complete network traffic
- Browser console logs showing blocked requests

---

[COMPLETAR - Name or pseudonym]
[COMPLETAR - Email]
[DATE]
