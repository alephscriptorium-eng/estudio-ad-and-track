# Communication to the UN Special Rapporteur on Freedom of Opinion and Expression

**Channel:** Submission via OHCHR portal (spsubmission.ohchr.org) or email to: freedex@ohchr.org
**Language:** English
**Subject:** Spain - Public broadcaster conditions access to publicly funded audiovisual content on acceptance of commercial surveillance infrastructure

---

## Communication under the mandate of the Special Rapporteur on the promotion and protection of the right to freedom of opinion and expression

### 1. Author information

- Name: [COMPLETAR]
- Nationality: Spanish / EU citizen
- Contact: [COMPLETAR - email]
- Date: [DATE]
- Confidentiality requested: Yes / No [SELECT]

### 2. State concerned

**Kingdom of Spain**

### 3. Summary of the situation

The Corporacion de Radio y Television Espanola (**RTVE**), Spain's national public broadcaster and a state-owned entity, operates a streaming platform called **RTVE Play** that serves as the primary means for citizens to access publicly funded audiovisual content online.

RTVE Play **blocks access to video content** for users who employ ad-blocking or privacy-enhancing browser extensions. Users are shown the message "Turn off your AdBlocker and try again" and cannot view the content unless they disable their privacy tools.

To access the content, users must accept loading **29 third-party tracking domains** including services that:
- Intercept all browser network communications at the API level
- Include remote-activated session replay capabilities (screen recording)
- Transfer personal data to servers in the United States

This creates a situation where citizens must choose between:
- **Exercising their right to access publicly funded information** (by disabling privacy tools and accepting surveillance), or
- **Protecting their privacy** (by keeping their ad blocker and being denied access to public content)

### 4. Relevant rights

#### 4.1 Right to access information (Article 19, ICCPR)

The International Covenant on Civil and Political Rights establishes the right to "seek, receive and impart information and ideas of all kinds." RTVE's content is publicly funded information that citizens have a right to access. Conditioning this access on the acceptance of commercial surveillance infrastructure constitutes a restriction that is neither necessary nor proportionate.

#### 4.2 Right to privacy (Article 17, ICCPR)

The surveillance infrastructure deployed by RTVE Play (29 tracking domains, network-level interception, session replay capability) constitutes an interference with the user's privacy that goes far beyond what is necessary for content delivery. The Special Rapporteur has noted in previous reports that **surveillance of online activity has a chilling effect on the exercise of freedom of expression** (A/HRC/23/40, A/HRC/29/32).

#### 4.3 Right to freedom of expression in the digital age

The Special Rapporteur has affirmed that **States have a positive obligation to promote a free, open and secure Internet** (A/HRC/17/27) and that **encryption and anonymity tools deserve protection** as enablers of freedom of expression (A/HRC/29/32, para. 56-60).

Ad-blocking and privacy extensions serve a similar function to encryption and anonymity tools: they protect users from unwanted surveillance and allow them to access information without being tracked. A state broadcaster that blocks access to content for users of these tools undermines the principles established by the mandate.

### 5. Factual basis

#### 5.1 RTVE's public service mandate

RTVE is governed by Ley 17/2006, which establishes its mission as a public service. It is funded primarily through Spain's national budget (Presupuestos Generales del Estado). Its content is produced with taxpayer money and should be universally accessible.

#### 5.2 Technical mechanism of the blocking

A forensic analysis of RTVE Play's video playback page reveals:

1. RTVE uses a commercial video player (THEOplayer 9.4.1) with integrated ad-blocker detection
2. The player checks whether Google's advertising SDK (IMA SDK) is available
3. If the SDK is blocked (by an ad blocker), the player refuses to play the content
4. RTVE has configured a deliberate fallback to an empty file (`js2.rtve.es/js/vacio`) that guarantees the blocking behavior
5. The player software offers a mode that would play content without ads, but RTVE has chosen the blocking mode
6. The detection is reported to three analytics systems simultaneously (Conviva, MarkCollector, Google Tag Manager)

#### 5.3 Surveillance infrastructure

The page loads tracking services from **29 different domains** including:

| Service | Company | Country | Function |
|---------|---------|---------|----------|
| Google IMA SDK | Google LLC | USA | Advertising (mandatory for content access) |
| Conviva AppTracker | Conviva Inc. | USA | Network interception + session replay |
| Adobe Audience Manager | Adobe Inc. | USA | User profiling |
| Adobe Analytics | Adobe Inc. | USA | Behavioral analytics |
| Chartbeat | Chartbeat Inc. | USA | Real-time behavioral tracking |
| VWO | Wingify | India | A/B testing / behavior tracking |

The Conviva service replaces the browser's core network functions (`XMLHttpRequest`, `fetch`, `WebSocket`) with its own implementations, enabling interception of all network traffic.

#### 5.4 Coercion mechanism

Users face a binary choice:
- Disable privacy tools -> gain access to content + accept full surveillance
- Keep privacy tools -> denied access to publicly funded content

There is no intermediate option (e.g., accessing content without tracking, or with minimal tracking).

### 6. Previous relevant work by the mandate

The Special Rapporteur has addressed related issues in:

- **A/HRC/17/27 (2011):** Report on the right to freedom of expression on the Internet, establishing that States should promote access to the Internet
- **A/HRC/23/40 (2013):** Report on State surveillance and freedom of expression, noting the chilling effect of surveillance
- **A/HRC/29/32 (2015):** Report on encryption and anonymity, affirming that tools protecting privacy online are essential for freedom of expression
- **A/HRC/32/38 (2016):** Report on the role of the State in protecting freedom of expression online, including the obligation not to restrict access to information
- **A/76/258 (2021):** Report on disinformation and freedom of expression, addressing the role of public service media

### 7. Request

I respectfully request that the Special Rapporteur:

1. **Consider this situation** in the context of the mandate's work on digital rights, surveillance, and access to information

2. **Address a communication to the Kingdom of Spain** requesting information on:
   - Whether the Government considers RTVE Play's content blocking compatible with Spain's obligations under Article 19 ICCPR
   - What measures are being taken to ensure universal access to publicly funded audiovisual content online
   - Whether the Government has assessed the surveillance infrastructure deployed by RTVE Play

3. **Include this issue** in future thematic reports on digital rights, particularly regarding:
   - The right to use privacy-enhancing technologies
   - The obligation of public institutions to provide surveillance-free access to information
   - The tension between advertising-funded public media and citizens' digital rights

### 8. Evidence

Complete technical documentation is available:
- Forensic analysis of the network architecture (157 requests, 29 domains)
- Deobfuscated source code of the ad-blocker detection and blocking mechanism
- HTTP Archive (HAR) file of the complete network traffic
- 44 JavaScript source files from the page

---

[COMPLETAR - Name]
[COMPLETAR - Contact email]
[DATE]

---

**Procedural notes:**
- Communications to the Special Rapporteur can be submitted by individuals, regardless of nationality
- The Special Rapporteur can send "allegation letters" or "urgent appeals" to States
- The process is confidential until the State responds (or after 60 days)
- The communication will be included in the annual report to the Human Rights Council (A/HRC/XX/XX)
- No legal representation is required
