# Open Letter to the European Broadcasting Union (EBU)

**Channel:** Email to ebu@ebu.ch / Legal & Policy: legalpolicy@ebu.ch
**Language:** English
**Subject:** RTVE Play practices incompatible with EBU public service media values - Request for guidance

---

Dear EBU,

## 1. Context

I am writing to you about practices on **RTVE Play**, the streaming platform of your member organization **Corporacion RTVE** (Spain). I believe these practices are incompatible with the EBU's stated values for public service media in the digital age.

The EBU has consistently advocated that public service media should be **universally accessible**, **trustworthy**, and should **protect audience privacy**. The findings below suggest that RTVE Play falls short of these principles.

## 2. Findings

A forensic technical analysis of the RTVE Play video playback infrastructure reveals three areas of concern:

### 2.1 Content access blocked for privacy-conscious users

RTVE Play **blocks video playback** when it detects that the user has an ad-blocking or privacy extension installed in their browser. The user sees the message: "Turn off your AdBlocker and try again."

The video player (THEOplayer 9.4.1) supports a configuration that would allow content to play without ads when ads fail to load. RTVE has deliberately chosen the blocking configuration. Citizens who use legitimate privacy and security tools -- recommended by cybersecurity agencies across Europe -- are denied access to publicly funded content.

This contradicts the EBU's principle that public service media should be **universally accessible** and should not create barriers to access based on users' technology choices.

### 2.2 Disproportionate third-party surveillance

To play a single video on RTVE Play, the user's browser contacts **29 different domains** operated by 12+ companies. These include:

- **Google** (advertising, analytics, tag management) -- USA
- **Adobe** (analytics, audience profiling) -- USA
- **Conviva** (video telemetry with full network interception) -- USA
- **Chartbeat** (real-time behavioral analytics) -- USA
- **VWO/Wingify** (A/B testing) -- India

Most critically, the **Conviva AppTracker** service replaces the browser's core network communication functions (XMLHttpRequest, fetch, WebSocket) with its own wrappers, enabling interception of all network traffic on the page. It also includes a session replay module capable of recording all user interactions.

This contradicts the EBU's advocacy for **audience trust** and **data protection** in public service media.

### 2.3 Dependence on commercial advertising gatekeepers

RTVE Play's video playback is technically dependent on the availability of Google's advertising infrastructure (IMA SDK). If Google's servers are unavailable, the content cannot play. A European public broadcaster should not have a hard dependency on a US commercial advertising platform for content delivery.

This contradicts the EBU's advocacy for **media sovereignty** and **independence from commercial platforms**.

## 3. Relevant EBU principles and recommendations

The EBU has published extensively on the role of public service media in the digital environment:

- **EBU "Big Tech and Public Service Media" reports:** Advocate for reducing dependence on commercial platforms
- **EBU Privacy and Data Protection guidelines:** Recommend that PSM organizations lead by example in protecting audience data
- **EBU "Vision 2030":** Emphasizes trust, universality, and independence as core PSM values
- **EBU "Recommendation on PSM in the Platform Environment":** Calls for PSM to maintain direct, unmediated relationships with audiences

RTVE Play's current architecture does the opposite: it intermediates the audience relationship through 12+ commercial third parties and conditions access on accepting their tracking.

## 4. What we are asking of the EBU

We are not asking the EBU to sanction or criticize its member. We are asking for **constructive guidance:**

1. **Clarify whether RTVE Play's practices align with EBU recommendations** on universal access, audience trust, and data protection for public service media platforms.

2. **Issue or reaffirm guidance** on the acceptable level of third-party tracking in PSM digital platforms, particularly regarding:
   - Blocking content for users with privacy tools
   - Third-party network-level interception (Conviva-type services)
   - Dependency on commercial advertising infrastructure for content delivery

3. **Facilitate a dialogue** between civil society organizations concerned about these practices and RTVE's technical leadership, within the framework of the EBU's member community.

4. **Consider a broader audit** of tracking practices across EBU member organizations' streaming platforms. If RTVE is doing this, others may be too.

## 5. Why the EBU is the right forum

This issue is not just about RTVE. It is about the **standard of digital conduct for public service media** across Europe. The EBU is uniquely positioned to:

- Set expectations for member organizations without regulatory enforcement
- Share best practices between PSM organizations
- Protect the reputation of public service media as trustworthy alternatives to commercial platforms

If public broadcasters replicate the surveillance models of commercial streaming services, they lose their unique value proposition: the trust of the public.

## 6. Evidence

Full technical documentation is available upon request:
- Architectural analysis of RTVE Play (157 network requests, 29 domains)
- Reverse-engineered ad-blocker detection mechanism
- Analysis of Conviva AppTracker network interception code
- Browser network capture (HAR format)

---

Respectfully,

[COMPLETAR - Name or pseudonym]
[COMPLETAR - Email]
[DATE]

---

**Note:** This letter is also being shared with civil society organizations (EDRi, Access Now, Privacy International, EFF) and regulatory bodies (AEPD, CNMC, EDPB, European Commission DG CONNECT) for coordination.
