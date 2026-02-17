# Denuncia ante la Comision Europea - DG CONNECT

**Destinatario:** Direccion General de Redes de Comunicacion, Contenido y Tecnologias (DG CONNECT)
**Sede:** Edificio BU25, Avenue de Beaulieu 25, 1160 Bruselas, Belgica
**Contacto:** CNECT-AUDIOVISUAL@ec.europa.eu (Unidad Audiovisual)
**Base legal:** Directiva 2010/13/UE (AVMSD) modificada por Directiva 2018/1808; Reglamento 2022/2065 (DSA); Reglamento 2022/1925 (DMA)
**Idioma:** Ingles (recomendado) o espanol
**Coste:** Gratuito

---

## Complaint regarding RTVE Play - Spanish Public Broadcaster

### 1. Complainant

- Name: [COMPLETAR]
- Nationality: Spanish
- Email: [COMPLETAR]
- Date: [FECHA]

### 2. Entity concerned

- Name: Corporacion de Radio y Television Espanola (RTVE)
- Nature: Spanish public service broadcaster (radiotelevision publica estatal)
- Tax ID: CIF Q-2876002-C
- Address: Avenida de Radio Television, 4, 28223 Pozuelo de Alarcon, Madrid, Spain
- Applicable national law: Ley 13/2022, General de Comunicacion Audiovisual (transposing the AVMSD)

### 3. Subject matter

This complaint concerns three interconnected issues relating to RTVE Play, the video-on-demand platform of the Spanish public broadcaster:

1. Conditioning access to public service audiovisual content on acceptance of third-party commercial advertising infrastructure (Google IMA SDK).
2. Dependency of a European public broadcaster on a designated gatekeeper (Google/Alphabet) for basic content delivery.
3. Compliance with the Audiovisual Media Services Directive (AVMSD) regarding commercial communications on public service media.

### 4. Facts

#### 4.1 Content blocking mechanism

RTVE Play uses the commercial video player THEOplayer 9.4.1 integrated with Google IMA SDK (Interactive Media Ads Software Development Kit) for advertising.

When a user accesses video content with an ad blocker enabled:

1. The player attempts to load Google IMA SDK from imasdk.googleapis.com.
2. If the SDK is unavailable, the player blocks content playback entirely.
3. The message displayed is "Turn off your AdBlocker and try again" -- in English, not in any EU official language.
4. THEOplayer offers a configuration option to continue playback without ads. RTVE explicitly configured the blocking option.
5. A deliberate empty fallback file (js2.rtve.es/js/vacio) ensures that content never plays when advertising fails.

This means that access to publicly funded audiovisual content is technically conditioned on the availability and acceptance of Google's advertising infrastructure.

#### 4.2 Dependency on designated gatekeeper

Google/Alphabet is a designated gatekeeper under the Digital Markets Act (DMA) for several core platform services. While the DMA primarily regulates gatekeeper behaviour, the fact that a European public broadcaster has structured its content delivery so that it cannot function without Google's infrastructure raises concerns about:

- Digital sovereignty: European public service media dependent on US gatekeeper infrastructure.
- Single point of failure: If Google's IMA SDK servers are unavailable, Spanish citizens cannot watch public television online.
- Lock-in: RTVE's choice of THEOplayer with Google IMA SDK integration creates technical dependency that is difficult to reverse.

#### 4.3 Scale of third-party commercial involvement

A forensic analysis of a single video page on RTVE Play reveals 157 HTTP requests to 29 different domains to play a single video. Third-party services include:

- Google IMA SDK (Alphabet/Google, USA) -- Advertising
- Google Tag Manager (Alphabet/Google, USA) -- Tag management
- Adobe Audience Manager (Adobe Inc., USA) -- Audience profiling
- Adobe VisitorAPI (Adobe Inc., USA) -- Visitor identification
- Conviva AppTracker (Conviva Inc., USA) -- Telemetry + network interception
- Chartbeat (Chartbeat Inc., USA) -- Real-time analytics
- VWO (Wingify, India) -- A/B testing
- OneTrust (OneTrust LLC, USA) -- Consent management

#### 4.4 Commercial incentive structure

The advertising ad tag includes a parameter PLAY_PLUS with values "0" (free user) or "1" (paid subscriber). This indicates that paid subscribers of RTVE Play+ receive different advertising treatment (fewer or no ads), while free users who block advertising are denied content entirely. This creates a de facto paywall for ad-blocker users on a public service platform.

### 5. EU law potentially engaged

#### 5.1 Audiovisual Media Services Directive (AVMSD) - Directive 2010/13/EU as amended

- Article 1(1)(a)(i): RTVE Play is an audiovisual media service under the Directive.
- Recital 5: The Directive aims to allow citizens to make use of the internal market and to ensure the free movement of audiovisual media services.
- Article 9: Rules on commercial communications, including the principle that commercial communications should be clearly identifiable and not use subliminal techniques.
- The blocking mechanism could be analysed as making commercial communications (advertising) a mandatory precondition for accessing public service content, inverting the normal regulatory logic where advertising is permitted but not required.

#### 5.2 European Democracy Action Plan and Media Freedom Act

The European Commission has emphasised the importance of independent, pluralistic public service media. The dependency of a Member State's public broadcaster on US corporate infrastructure for basic content delivery raises questions about the resilience and independence of European public service media.

#### 5.3 Digital Services Act (DSA) - Regulation 2022/2065

- Article 14: General transparency obligations for providers of intermediary services. RTVE Play operates as a platform and its terms of service should clearly inform users about content restrictions.
- Transparency: Users should understand why content is being restricted and have means to contest restrictions.

#### 5.4 Digital Markets Act (DMA) - Regulation 2022/1925

While the DMA primarily regulates gatekeepers, the European Commission may have interest in the downstream effects of gatekeeper dependency: a European public broadcaster that structures its services around a gatekeeper's advertising stack to the point where content cannot be served without it.

### 6. Evidence

The following technical evidence is available:

1. Technical report: Complete analysis of RTVE Play's loading chain, third-party service inventory, and Conviva network interception mechanism.
2. Ad blocker detection analysis: Deobfuscated THEOplayer source code demonstrating deliberate blocking configuration.
3. HAR network capture: Complete network traffic for a single video page (157 requests, 29 domains). Machine-readable evidence.
4. Source code archive: 44 JavaScript files downloaded directly from RTVE Play.

### 7. National proceedings

The following actions have been filed at national level:

- Complaint to RTVE's Viewer Ombudsman
- Freedom of information request to RTVE
- GDPR complaint to AEPD (Spanish DPA)
- Audiovisual regulation complaint to CNMC
- Proposal for parliamentary questions to the Spanish Parliament

European-level actions are also being filed with noyb, EDRi, and the EDPB.

### 8. Request

I respectfully request that DG CONNECT:

1. Examine whether RTVE's practice of blocking access to public service audiovisual content for ad blocker users is compatible with the AVMSD and the principles of universal access to public service media.

2. Assess the broader trend of European public broadcasters becoming dependent on designated gatekeeper infrastructure (particularly Google) for basic content delivery, and whether this poses risks to European digital sovereignty and media independence.

3. Consider issuing guidance to Member States on the compatibility of ad blocker detection and content blocking with public service media obligations under the AVMSD.

4. Refer the data protection aspects to the EDPB if appropriate, given the cross-border nature of the data processing.

5. Examine whether the case merits inclusion in any ongoing or planned assessments of European public service media resilience and digital sovereignty.

---

[Name / Nombre]
[ID / DNI]
[Date / Fecha]
[Signature / Firma]

---

**Nota practica:** Las denuncias a la Comision Europea pueden presentarse a traves del formulario general de contacto (ec.europa.eu/info/about-european-commission/contact) o directamente por correo electronico a la unidad audiovisual de DG CONNECT. Se recomienda redactar en ingles para facilitar el procesamiento. No se requiere representacion legal.
