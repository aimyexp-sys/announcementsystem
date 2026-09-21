# Local Sound Transfer / Parish Box Program

A hardware and systems design for local audio distribution to 500 households within 2 km, running unattended 35 times a week, built without licensed RF spectrum.

## Project Overview

This is a community audio distribution system that enables institutions (churches, masjids, temples) to broadcast announcements and services to their neighborhoods through existing household infrastructure—phones, Wi-Fi, and telephone networks—rather than traditional FM radio.

**Core constraints:**
- ₹300 per household (hard ceiling)
- 35 broadcasts per week, fully unattended
- No licensed spectrum available
- Zero setup required from households
- Silent failure is the worst outcome

## Technical Architecture

The system consists of five subsystems:

1. **Origination** — Line tap from the PA mixer into a local encoder
2. **Transport** — Opus 32 kbit/s HTTP stream, token-gated per household  
3. **Endpoints** — Three distribution tiers:
   - Tier 1: Household's own smartphone (₹0)
   - Tier 2: Dedicated Wi-Fi speaker box (₹285)
   - Tier 3: Telephone gateway for landlines (per-minute cost)

## Key Specifications

| Aspect | Specification |
|--------|---------------|
| **Coverage** | 500 homes, ~2 km radius |
| **Broadcasts/week** | 35 (5 daily + weekly) |
| **Endpoint cost** | ₹285 (all-in BOM) |
| **Standby power** | 0.35 W (₹30/year) |
| **Latency** | <4 seconds glass-to-ear |
| **Per-listener egress** | 32 kbit/s |

## Documentation

📊 **Full technical brief (interactive):** https://claude.ai/artifact/EWUxCgw4n1XFNppcqicoHZ

This dynamic document includes:
- Detailed system architecture diagrams
- Interactive BOM explorer (drag to change volume)
- Latency budget analysis with toggle conditions
- Power consumption breakdown
- Firmware & OTA strategy
- Unattended operation scheduling
- Codec selection gate
- Field test protocol

## Project Files

- **Community Audio BRD.pdf** — Business requirements and market analysis
- **Engineering Solution.pdf** — Technical deep dive and design rationale
- **Marketing and Sales Solution.pdf** — Go-to-market strategy and positioning
- **Step-by-Step Build Guide_Implementation.pdf** — Implementation roadmap and timeline
- **info.txt** — Quick reference notes

## Key Design Decisions

### No RF Transmission
Religious institutions in India are ineligible for community radio licenses under Clause 2(b) of the Community Radio guidelines. Licensed FM and unlicensed transmitters are not available paths, so the system rides on existing infrastructure households already own.

### Three Endpoints, Not One
Different households have different connectivity:
- **Tier 1**: Smartphone users get free distribution
- **Tier 2**: Older households or those without smartphones get a dedicated device
- **Tier 3**: Those without internet access still have phone access

### Silent Failure Prevention
The largest engineering challenge: preventing a box that stopped working 3 weeks ago from disappearing silently. Heavy instrumentation and local 30-day schedule caching ensure connectivity is monitored continuously.

### Unattended Scheduling
35 broadcasts per week with no staff intervention. Prayer times are computed locally with the institution's printed timetable as the source of truth, validated before first enrollment.

## Electrical Specifications

**Endpoint (Parish Box):**
- Microcontroller: ESP32-C3 (Wi-Fi decode)
- Audio: MAX98357A (DAC + 3W class-D amplifier)
- Speaker: 66mm 4Ω
- Power: External certified 5V 1A adapter
- Standby draw: 0.35 W (₹30/year)
- Volume control: Potentiometer with digital gain

**Deliberate omissions:**
- No microphone (privacy hardware guarantee)
- No SD card (eliminates common field failure)
- No battery (adds cost, disposal obligation, shorter lifespan)
- No local scheduling (timing decisions made by platform, not device)

## Economics

**Bill of Materials** (500-unit volume):
- Landed cost: ₹285 per unit
- Headroom to ₹300 ceiling: ₹15
- Mould tooling ROI point: ~4,000 units (not justified for pilot)

See the interactive BOM explorer in the technical brief for volume sensitivity analysis.

## Development Status

This is the **announcement system** repository containing:
- Hardware design specifications
- Firmware architecture
- Deployment and operations planning  
- BOM and cost analysis
- Field test protocols

## Next Steps

1. **Week 1**: Codec validation gate (Opus on C3 @ −70 dBm for 1 hour)
2. **Week 2**: Blind listening test (adhan, with community)
3. **Weeks 3–4**: Prototype build and field testing (25-box pilot)
4. **Month 2**: OTA rollback verification and production prep
5. **Month 3**: Full enrollment and monitoring setup

---

*Engineering brief compiled September 2026*  
*For technical questions, refer to the interactive documentation linked above.*
