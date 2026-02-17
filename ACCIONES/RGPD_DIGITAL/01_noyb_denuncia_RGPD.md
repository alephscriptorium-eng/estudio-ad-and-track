# Denuncia ante noyb - Centro Europeo de Derechos Digitales

**Destinatario:** noyb - European Center for Digital Rights
**Sede:** Goldschlagstrasse 172/4/3/2, 1140 Viena, Austria
**Contacto:** info@noyb.eu / Formulario: noyb.eu/en/file-complaint
**Idioma:** Ingles
**Coste:** Gratuito

---

## Resumen ejecutivo / Executive Summary

La Corporacion RTVE (radiotelevision publica espanola, equivalente a la ORF austriaca o la ARD/ZDF alemana) realiza transferencias masivas de datos personales a Estados Unidos a traves de su plataforma de video bajo demanda RTVE Play, sin garantias adecuadas post-Schrems II, con un sistema de consentimiento deficiente y con un servicio de telemetria (Conviva) que intercepta globalmente todas las comunicaciones de red del navegador del usuario.

---

## Complaint to noyb

### 1. Identity of the Data Controller

- **Name:** Corporacion de Radio y Television Espanola (Corporacion RTVE)
- **Tax ID:** CIF Q-2876002-C
- **Address:** Avenida de Radio Television, 4, 28223 Pozuelo de Alarcon, Madrid, Spain
- **Website:** www.rtve.es
- **DPO:** dpd@rtve.es
- **Nature:** Spanish public broadcaster, equivalent to ORF (Austria), funded primarily by the Spanish State Budget (Presupuestos Generales del Estado).

### 2. Facts

#### 2.1 Massive international data transfers to the US

When a user accesses RTVE Play to watch video content, the page makes 157 HTTP requests to 29 different domains. The following involve data transfers to US-based companies:

- Google IMA SDK (Google LLC, USA): Screen dimensions, user agent, consent string, subscription status
- Adobe Audience Manager (Adobe Inc., USA): Unique visitor ID (MID), audience profiling
- Conviva AppTracker (Conviva Inc., USA): Full session telemetry, potential full session replay
- Chartbeat (Chartbeat Inc., USA): Real-time browsing behaviour
- VWO (Wingify, India): A/B testing data, user behaviour

These transfers occur for every video playback attempt on the Spanish public broadcaster's platform.

#### 2.2 Global network interception by Conviva (US provider)

The Conviva AppTracker v1.4.3 library, loaded via Google Tag Manager on RTVE Play, performs a systematic interception of all browser network communications through monkey-patching:

- Replaces XMLHttpRequest.prototype.open, .send, and .setRequestHeader with its own functions
- Replaces window.fetch with its own interceptor
- Replaces the WebSocket constructor and WebSocket.prototype.send with its own implementations

This means all network communications made by the user's browser while the RTVE Play page is open are potentially intercepted and processed by Conviva, with data transmitted to servers in the United States (appgw.conviva.com).

#### 2.3 Full session replay capability

Conviva AppTracker includes a complete session recording module based on the rrweb library, which captures:

- Mouse movements
- User clicks
- Scroll events
- Form field changes (inputs)
- DOM mutations (page content changes)

These recordings are stored in local IndexedDB (conviva-session-record-db), compressed with gzip, and uploaded to Google Cloud Storage. Activation depends on remote configuration downloaded from rc.conviva.com, meaning RTVE or Conviva can enable full session recording without the user's knowledge and without updating the page code.

#### 2.4 Deficient consent mechanism

The cookie consent banner is managed by OneTrust, but:

- The OneTrust SDK itself is blocked by common privacy extensions, so the consent mechanism is never presented to these users.
- Multiple tracking services (Conviva, Adobe, VWO) are loaded before or independently of the user having given consent through OneTrust.
- Telemetry data (24 Conviva heartbeat POSTs) is sent even when the video never plays (because the player blocks playback upon detecting an ad blocker). Data is collected without content ever being consumed.

#### 2.5 Post-Schrems II inadequacy

Following the CJEU's Schrems II ruling (C-311/18), transfers of personal data to the United States require additional safeguards beyond Standard Contractual Clauses. There is no public evidence that RTVE has conducted a Transfer Impact Assessment for any of the 5+ US-based providers receiving personal data from RTVE Play users.

### 3. GDPR provisions potentially violated

- Art. 5(1)(a) Transparency: Users not informed of global network interception, session replay capability, or number of data processors
- Art. 5(1)(c) Data minimisation: Global interception of XHR/fetch/WebSocket far exceeds what is necessary for video analytics
- Art. 6 Lawful basis: Telemetry before consent and without content consumption lacks legal basis
- Art. 13-14 Information duty: Privacy policy does not detail all processors or international transfers adequately
- Art. 35 DPIA: No public evidence of Data Protection Impact Assessment for RTVE Play
- Art. 44-49 International transfers: Transfers to 5+ US providers without adequate safeguards post-Schrems II

### 4. Why this case matters for noyb

1. **Public broadcaster:** RTVE is not a private company -- it is the Spanish state broadcaster, funded by public money. If a public service mandated to serve all citizens transfers their data to US companies without adequate safeguards, it represents a systemic failure.

2. **Pattern across European public broadcasters:** This case may reveal a broader pattern of European public broadcasters outsourcing their digital infrastructure to US tech companies (Google IMA SDK, Adobe Analytics, etc.), creating GDPR compliance issues at scale.

3. **Schrems II enforcement gap:** Despite Schrems II being decided in 2020, public institutions continue making uncommitted US transfers. This case provides concrete, forensically documented evidence.

4. **Novel surveillance technique:** Conviva's global monkey-patching of XHR/fetch/WebSocket has not been widely examined by DPAs. A complaint here could set important precedent.

### 5. Evidence provided

1. Technical report (01_informe_tecnico_departamento_IT.md): Complete analysis of the loading chain, third-party service inventory, Conviva AppTracker analysis.
2. Ad blocker detection analysis (02_analisis_mecanismo_deteccion_adblock.md): Deobfuscated source code of the detection mechanism.
3. HAR capture (www.rtve.es.har): Complete network traffic for a single video page (157 requests to 29 domains).
4. Browser console logs (logs.md): Blocked requests and errors.
5. Source code (TVE_files/): 44 JavaScript files downloaded from the page as primary verifiable evidence.

### 6. Request

We request that noyb:

1. Evaluate this complaint for inclusion in its case portfolio.
2. File a formal complaint with the Spanish DPA (AEPD) and/or the competent lead supervisory authority under Art. 56 GDPR.
3. Consider this case as part of a broader investigation into European public broadcasters' GDPR compliance.

---

**Contact details of complainant:**

- Name: [COMPLETAR]
- Email: [COMPLETAR]
- Country: Spain
- Date: [FECHA]
