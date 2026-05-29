# OTP Shield — AI-Powered Firewall for OTP Scam Prevention

<div align="center">

**🏆 3rd Place — Future of Business · Cal Poly Pomona AI for a Better Future Hackathon 2025**
**Sponsored by Avanade & Databricks · 239 Students · 51 Teams · 60 Hours**

[![React Native](https://img.shields.io/badge/React_Native-Expo-0EA5E9?logo=react)](https://expo.dev)
[![JavaScript](https://img.shields.io/badge/JavaScript-ES6+-F7DF1E?logo=javascript&logoColor=black)](https://developer.mozilla.org/en-US/docs/Web/JavaScript)
[![Platform](https://img.shields.io/badge/Platform-Android_%7C_iOS-lightgrey?logo=android)](https://reactnative.dev)
[![License](https://img.shields.io/badge/License-MIT-green)](LICENSE)

**Team Trio**
Gokul Srinath Seetha Ram · Rashmi Elavazhagan · Shreyas Chaudhary

[🎥 Watch Demo](https://www.youtube.com/watch?v=pWo-e7Ve1Hk) · [🏆 Official Winners Announcement](https://www.cpp.edu/cba/news/ai-for-a-better-future-cal-poly-pomona-hosts-transformative-hackathon-and-ai-fair.shtml)

</div>

---

## What Is OTP Shield?

OTP Shield is a **mobile-first AI firewall** that protects users from OTP (One-Time Password) fraud in real time. It intercepts incoming SMS messages, analyses them using a multi-factor risk engine, and blocks fraudulent OTP requests before the user can act on them.

OTP fraud is one of the fastest-growing vectors of financial crime globally — a scammer sends a fake bank SMS, social-engineers the victim into sharing their OTP, and drains their account in seconds. OTP Shield sits between your SMS inbox and your actions, giving every message a threat score before you see it.

Built in 60 hours at the **Cal Poly Pomona "AI for a Better Future" Hackathon 2025** — themed *"Artificial Intelligence for All: Bridging Disciplines, Expanding Possibilities"* — and recognised as **3rd Place in the Future of Business category** out of 51 competing teams.

---

## Hackathon Context

| Detail | Info |
|--------|------|
| **Event** | AI for a Better Future Hackathon & Fair 2025 |
| **Host** | Cal Poly Pomona — College of Business Administration |
| **Sponsors** | Avanade (Platinum) · Databricks |
| **Challenge** | Future of Business |
| **Placement** | 🥉 3rd Place |
| **Team** | Trio — Gokul Srinath Seetha Ram, Rashmi Elavazhagan, Shreyas Chaudhary |
| **Duration** | 60 hours |
| **Scale** | 239 students · 51 teams · 7 challenge categories |
| **Judges** | Jeff Farinich · Rob Leach · Jason James · Johans Acosta · Molly Strawn-Carreño |

> *"AI is transforming the future of work."* — Rob Leach, Global Microsoft Executive & General Manager, Avanade (Keynote Speaker)

---

## The Problem

Every year, millions of people fall victim to OTP fraud:

- A scammer impersonates your bank via SMS
- They create urgency: *"Your account will be suspended — verify with OTP immediately"*
- The victim, panicked, shares their OTP
- The account is drained in seconds

Existing solutions require technical literacy or rely on after-the-fact fraud detection. **OTP Shield acts before the user responds** — scoring every incoming message in real time and blocking high-risk OTPs automatically.

---

## How It Works

```
Incoming SMS
     │
     ▼
┌─────────────────────────────────────────────┐
│           OTP Extraction Engine             │
│   Regex patterns detect 4–8 digit codes     │
│   with contextual clues (OTP, code, verify) │
└────────────────────┬────────────────────────┘
                     │
                     ▼
┌─────────────────────────────────────────────┐
│         Multi-Factor Risk Scorer            │
│                                             │
│  Sender Risk     — trusted ID whitelist     │
│  Message Risk    — phishing keyword scan    │
│  Frequency Risk  — too many OTPs = suspect  │
│  Location Risk   — unusual geography flag   │
│  Time Risk       — night-time OTPs flagged  │
│                                             │
│  Risk Score = weighted sum (0.0 → 1.0)      │
└────────────────────┬────────────────────────┘
                     │
          ┌──────────┴──────────┐
          │                     │
    Score > 0.5            Score ≤ 0.5
          │                     │
          ▼                     ▼
    🔴 BLOCKED            🟢 SAFE
    Reason shown          OTP verified
```

---

## Risk Scoring Model

The core detection engine (`MessageService.js`, `helpers.js`) computes a composite risk score from five independent signals:

| Signal | Weight | Trigger |
|--------|--------|---------|
| **Sender Risk** | +0.50 | Sender ID not in trusted whitelist (HDFCBK, SBIBANK, ICICI, AXISBK, etc.) |
| **Message Risk** | up to +0.30 | Phishing keywords: `urgent`, `click`, `share otp`, `account locked`, `suspended`, `bit.ly` URLs |
| **Frequency Risk** | up to +0.30 | Multiple OTP requests in a short window |
| **Location Risk** | +0.20 | Geolocation flagged as unusual for the user |
| **Time Risk** | +0.10 | OTP received between midnight and 6 AM |

**Threshold:** `riskScore > 0.5` → message blocked. Final score capped at 1.0.

### Phishing Classification (`classifyMessage`)

Beyond the risk score, every message goes through a dedicated phishing classifier:

- **Keyword matching** — 14 high-signal phrases including `"send your otp"`, `"verify account"`, `"claim"`, `"win"`
- **URL detection** — regex catches `bit.ly`, `tinyurl`, `goo.gl`, and raw HTTP links
- **OTP-sharing requests** — regex catches any message asking the user to *send/share/provide/give* a code or PIN (+3 weight)
- **Trusted sender check** — known bank sender IDs reduce phishing confidence

---

## Features

| Feature | Description |
|---------|-------------|
| **Real-Time Message Analysis** | Every incoming message is scored and classified before display |
| **OTP Extraction** | Regex engine detects 4–8 digit OTPs with contextual awareness |
| **Trusted Sender Whitelist** | HDFCBK, SBIBANK, ICICI, AXISBK, YESBNK, Netflix, Amazon, Uber, Swiggy |
| **Risk Score Breakdown** | Drill-down modal shows per-component score (sender, message, frequency, location, time) |
| **Colour-Coded Indicators** | Green (safe) · Yellow (suspicious) · Red (blocked) |
| **Message History** | Scrollable inbox with analysis status on every message |
| **Analysis History Tab** | Track all previous risk assessments in one view |
| **Dark Mode UI** | Full dark theme with animated shield logo and gradient accents |
| **Simulation Mode** | Built-in message generator with 50/50 mix of legitimate and scam templates for demo purposes |

---

## Tech Stack

| Layer | Technology |
|-------|-----------|
| **Framework** | React Native 0.73 + Expo |
| **Navigation** | React Navigation (Stack + Tab) |
| **UI Components** | `expo-linear-gradient`, `@expo/vector-icons` (MaterialIcons, Ionicons) |
| **Animations** | React Native `Animated` API |
| **Location** | `expo-location` |
| **OTP Detection** | Custom regex engine |
| **Risk Scoring** | Rule-based multi-factor model |

---

## Project Structure

```
OTPShield/
├── App.js                          ← Root navigator
├── src/
│   ├── screens/
│   │   ├── HomeScreen.js           ← Main inbox + analysis modal
│   │   └── SettingsScreen.js       ← User preferences
│   ├── components/
│   │   ├── MessageList.js          ← Scrollable message feed
│   │   ├── MessageItem.js          ← Single message card with risk indicator
│   │   └── AnalysisResult.js       ← Risk breakdown modal component
│   ├── services/
│   │   └── MessageService.js       ← Core: analysis, classification, history
│   └── utils/
│       ├── helpers.js              ← OTP extractor, risk scorer, classifier
│       └── SplashGenerator.js      ← Splash screen utility
├── assets/
│   └── splash.png
└── app.json                        ← Expo config
```

---

## Getting Started

### Prerequisites
- Node.js 18+
- Expo CLI (`npm install -g expo-cli`)
- Expo Go app on your phone, or an Android/iOS emulator

### Install & Run

```bash
git clone https://github.com/gokulsrinaths/OTPShield.git
cd OTPShield
npm install
npx expo start
```

Scan the QR code with **Expo Go** on your phone, or press `a` for Android emulator / `i` for iOS simulator.

---

## Demo Flow

1. Launch the app — the home screen shows a live stream of incoming messages
2. Tap any message to trigger analysis
3. The risk scorer runs in real time and returns a score with full breakdown
4. Messages above the 0.5 threshold are marked **BLOCKED** in red with the specific reason
5. Safe messages from trusted senders pass through in green
6. Switch to the **History** tab to review all past analyses

---

## Award & Press

**🥉 3rd Place — Future of Business Category**
**Cal Poly Pomona AI for a Better Future Hackathon 2025**

Hosted by the Mitchell C. Hill Center for Digital Innovation, College of Business Administration, Cal Poly Pomona. Sponsored by **Avanade** (co-founded by CBA alumnus Mitchell C. Hill) and **Databricks**.

Evaluated on creativity, real-world impact, feasibility, and storytelling by industry judges from Avanade and Microsoft.

| | |
|---|---|
| **🎥 Demo Video** | [Watch on YouTube](https://www.youtube.com/watch?v=pWo-e7Ve1Hk) |
| **🏆 Official Announcement** | [Cal Poly Pomona — AI for a Better Future](https://www.cpp.edu/cba/news/ai-for-a-better-future-cal-poly-pomona-hosts-transformative-hackathon-and-ai-fair.shtml) |

> *Listed under "Future of Business — 3rd Place: OptiShield AI - AI Powered Firewall for OTP Scam Prevention, Team: Trio"* on the official Cal Poly Pomona winners page. *(Note: the official listing uses an alternate spelling — the project name is OTP Shield.)*

---

## License

MIT — free to use, modify, and build on.

---

<div align="center">

*Built in 60 hours at the Cal Poly Pomona AI for a Better Future Hackathon 2025*

**Team Trio** — Gokul Srinath Seetha Ram · Rashmi Elavazhagan · Shreyas Chaudhary

[🎥 Demo](https://www.youtube.com/watch?v=pWo-e7Ve1Hk) · [🏆 Official Announcement](https://www.cpp.edu/cba/news/ai-for-a-better-future-cal-poly-pomona-hosts-transformative-hackathon-and-ai-fair.shtml)

</div>
