# Solicitud de apoyo a EDRi - European Digital Rights

**Destinatario:** EDRi - European Digital Rights
**Sede:** Rue Belliard 12, 1040 Bruselas, Belgica
**Contacto:** info@edri.org / https://edri.org/take-action/
**Idioma:** Ingles
**Coste:** Gratuito

---

## Request for support and coordination

### Dear EDRi team,

I am writing to bring to your attention a documented case involving the Spanish public broadcaster RTVE that touches on several core issues within EDRi's mission: the right to use ad blockers, mass surveillance through public service websites, and the dependency of European public institutions on US tech infrastructure.

### 1. Summary of the case

RTVE Play, the video-on-demand platform of Spanish public television (funded by the State Budget, equivalent to the BBC iPlayer or ARD Mediathek), blocks video playback for citizens who use ad blockers in their browsers.

A forensic technical analysis of the page reveals:

- **Deliberate content blocking:** RTVE has configured the THEOplayer video player to prevent content playback when Google's advertising system (IMA SDK) fails to load. The player offers the option to play content without ads, but RTVE explicitly chose to block access instead.
- **Massive tracking infrastructure:** Loading a single video page generates 157 HTTP requests to 29 different domains, including multiple US-based profiling and tracking services (Google, Adobe, Conviva, Chartbeat, VWO).
- **Global browser surveillance:** The Conviva telemetry service monkey-patches XMLHttpRequest, fetch, and WebSocket to intercept all network communications from the user's browser -- far beyond what is needed for video analytics.
- **Session replay capability:** Conviva includes a full session recording module (based on rrweb) that can capture mouse movements, clicks, scroll, form inputs, and DOM mutations. Activation is controlled remotely.
- **Broken consent mechanism:** The OneTrust consent banner is blocked by common privacy extensions, and multiple tracking services load before or independently of user consent.
- **Pay-to-access incentive:** Paid subscribers (Play+) receive different ad treatment, creating an economic incentive to pay for content that should be universally free as a public service.

### 2. Why this matters for digital rights in Europe

#### 2.1 Right to use ad blockers
This case represents a direct assault on the right of citizens to use ad blockers -- a right that has been increasingly recognised as a legitimate tool for self-defence against tracking, malware, and unwanted content. When a public broadcaster punishes citizens for exercising this right by denying access to publicly funded content, it sets a dangerous precedent.

#### 2.2 Public service media and digital sovereignty
European public broadcasters are supposed to be the counterweight to commercial platforms. If they outsource their digital infrastructure to Google (IMA SDK, Tag Manager), Adobe (Audience Manager), and Conviva (telemetry), they become dependent on US tech companies and subject their citizens' data to US jurisdiction.

#### 2.3 Pattern across Europe
This is unlikely to be unique to RTVE. Many European public broadcasters use similar advertising and analytics stacks. A coordinated investigation could reveal systemic issues across the EU.

### 3. Actions already taken at national level

The following formal actions have been prepared and are being filed in Spain:

- Complaint to RTVE Viewer's Ombudsman (Corporacion RTVE) - Prepared
- Freedom of information request (RTVE via Transparency Portal) - Prepared
- GDPR complaint (Spanish DPA / AEPD) - Prepared
- Audiovisual regulation complaint (CNMC) - Prepared
- Parliamentary questions (Spanish Parliament) - Prepared
- GDPR complaint to noyb (Vienna) - Prepared
- Cross-border complaint (EDPB, Brussels) - Prepared
- International report (EFF, San Francisco) - Prepared
- EU-level complaint (DG CONNECT, Brussels) - Prepared

### 4. What we request from EDRi

1. **Amplification:** Help raise awareness of this case across EDRi's network of 40+ member organisations, particularly those working on the right to use ad blockers, public service media accountability, GDPR enforcement, and digital sovereignty.

2. **Coordination with Spanish members:** Connect this case with EDRi members in Spain (e.g., Xnet, Pangea) who may want to join or support the national-level actions.

3. **European advocacy:** Consider using this case as evidence in EDRi's advocacy work on the ePrivacy Regulation (tracking without consent on public service websites), Digital Services Act implementation (transparency of public service platforms), and digital sovereignty and public digital infrastructure.

4. **Media strategy:** Coordinate European media coverage to maximise impact and encourage other citizens across Europe to examine their own public broadcasters.

### 5. Evidence package

We can provide the complete technical evidence package:

1. Full technical report with loading chain analysis
2. Deobfuscated source code of the ad blocker detection mechanism
3. HAR network traffic capture (157 requests, 29 domains)
4. Browser console logs
5. 44 downloaded JavaScript source files as primary evidence
6. All formal complaint documents (in Spanish)

### 6. Contact

- Name: [COMPLETAR]
- Email: [COMPLETAR]
- Country: Spain
- Organisation (if any): [COMPLETAR / Citizen initiative]
- Date: [FECHA]

---

*This case demonstrates that the fight for digital rights is not just about regulating tech giants -- it is also about ensuring that our own public institutions respect citizens' digital autonomy.*
