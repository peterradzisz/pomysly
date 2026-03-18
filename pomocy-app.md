# Pomocy - Personal Safety SOS App (MVP)

## TL;DR

> **Quick Summary**: Privacy-first personal safety app with configurable SOS - user chooses what happens when triggered (112 call, contact notification, audio/video recording, location sharing).
> 
> **Deliverables** (v1 - MVP):
> - One-tap SOS button + Voice trigger (with confirmation)
> - Audio + Video recording during emergency
> - Configurable settings (112 call, contacts, recording, location)
> - Test mode (test button without triggering real emergency)
> - Basic location info for responders
> - Open source (MIT License)
> 
> **NOT in v1**: AI (deferred), Ads (deferred)
> 
> **Estimated Effort**: Medium-Large
> **Parallel Execution**: YES - 4 waves

---

## Context

### Core Concept
User configures BEFORE emergency what should happen:
- Call 112: Yes/No
- Call emergency contacts: Yes/No (select who)
- Record audio: Yes/No  
- Record video: Yes/No
- Share location: Yes/No

Plus: Test button feature to verify setup works

### Key Decisions (v1)
- **Emergency number**: 112 (configurable)
- **Trigger**: Button + Voice (with confirmation)
- **Recording**: Audio + Video
- **AI**: Basic (GPS + time + info) - no AI in v1, investigate TensorFlow Lite for future
- **Contacts**: Yes - configurable
- **Ads**: Skip for v1 (free, no monetization)
- **Open source**: MIT License

### Competition
| App | Pomocy v1 Differentiation |
|-----|-------------------------|
| Red Panic Button | More configurable, test mode, open source |
| Google Personal Safety | Open source, configurable recording |
| Noonlight | Free (no subscription), no call center required |

---

## Work Objectives

### Core Deliverables (v1)
- [ ] React Native/Expo app (iOS + Android)
- [ ] SOS button (prominent on home screen)
- [ ] Voice trigger with confirmation
- [ ] Emergency call (112, configurable)
- [ ] Audio recording (configurable on/off)
- [ ] Video recording (configurable on/off)
- [ ] Location sharing (configurable on/off)
- [ ] Emergency contacts system (select who to notify)
- [ ] **Pre-emergency configuration screen** with all toggles
- [ ] **Test mode** - test SOS without triggering real emergency
- [ ] Open source (MIT License)

### Must Have
- One-tap SOS button
- Voice trigger (with confirmation step)
- 112 emergency call
- Configurable: 112 call, contacts call, audio/video recording, location
- Test mode (critical - user can verify setup)
- Works offline

### Must NOT Have (v1 Guardrails)
- No AI (deferred to future)
- No ads (deferred to future)
- No mandatory call center
- No cloud data collection

---

## Execution Strategy

### Wave 1: Foundation
1. Initialize Expo project
2. Navigation + state management
3. UI component library

### Wave 2: Core SOS
4. SOS button + state machine
5. Voice trigger with confirmation
6. Emergency call (112)
7. Location services
8. Test mode implementation

### Wave 3: Recording + Contacts
9. Audio recording
10. Video recording
11. Emergency contacts system
12. Pre-emergency configuration screen

### Wave 4: Polish + Release
13. Settings refinement
14. Onboarding flow
15. E2E testing
16. App store submission + GitHub

---

## TODOs

### Wave 1: Foundation

- [ ] 1. Initialize React Native project with Expo

  **What to do**: `npx create-expo-app@latest Pomocy`, navigation, state, TypeScript, git

  **Acceptance Criteria**: Project runs, structure in place

- [ ] 2. Set up navigation + Zustand state

  **What to do**: React Navigation (tabs + stack), Zustand stores

  **Acceptance Criteria**: Navigation works, state persists

- [ ] 3. Create UI component library

  **What to do**: Button (SOS variant), Toggle/Slider, Card, Header components

  **Acceptance Criteria**: Reusable components with design tokens

### Wave 2: Core SOS Features

- [ ] 4. SOS button + emergency state machine

  **What to do**: Large SOS button, state: idle→confirming→active→resolved, countdown

  **Must NOT do**: Skip confirmation step

  **Acceptance Criteria**: 
  - Button prominent on home
  - 3-second countdown with cancel option
  - State transitions work correctly

- [ ] 5. Voice trigger with confirmation

  **What to do**: On-device speech recognition, wake words ("help", "pomocy", "SOS"), confirmation before SOS activates

  **Must NOT do**: Trigger without confirmation

  **Acceptance Criteria**:
  - Voice triggers countdown
  - Confirmation required before SOS
  - Works offline

- [ ] 6. Emergency call functionality

  **What to do**: expo-linking, dial 112, configurable number

  **Acceptance Criteria**: Opens dialer with correct number

- [ ] 7. Location services

  **What to do**: expo-location, get GPS, format for sharing

  **Acceptance Criteria**: Gets current location, works in emergency

- [ ] 8. Test mode implementation

  **What to do**: Separate test button - runs through SOS flow WITHOUT actually calling 112 or contacts

  **This is critical**: User must be able to verify their setup works

  **Acceptance Criteria**:
  - Test mode doesn't call 112
  - Test mode doesn't notify contacts
  - Shows what WOULD happen
  - Clear "TEST MODE" indicator

### Wave 3: Recording + Contacts

- [ ] 9. Audio recording

  **What to do**: expo-av, auto-start in emergency (if enabled in config), local save

  **Acceptance Criteria**: Records if config allows, saves locally

- [ ] 10. Video recording

  **What to do**: expo-camera, front/back toggle, auto-start (if enabled)

  **Acceptance Criteria**: Records video if config allows

- [ ] 11. Emergency contacts system

  **What to do**: Add/edit/delete contacts, select who to notify, import from device

  **Acceptance Criteria**:
  - Can add contacts
  - Can select which contacts get notified
  - Max 5 contacts

- [ ] 12. Pre-emergency configuration screen

  **CRITICAL SCREEN** - User configures BEFORE emergency what happens:

  | Setting | Type | Default |
  |---------|------|---------|
  | Call 112 | Toggle | ON |
  | Call emergency contacts | Toggle | OFF |
  | Record audio | Toggle | ON |
  | Record video | Toggle | OFF |
  | Share location | Toggle | ON |

  Also shows: Configured contacts list

  **Acceptance Criteria**:
  - All toggles work
  - Settings persist
  - Contact selection works

### Wave 4: Polish + Release

- [ ] 13. Settings refinement

  **What to do**: Emergency number config, about screen, data handling info

  **Acceptance Criteria**: Settings persist correctly

- [ ] 14. Onboarding flow

  **What to do**: First-launch permission requests, explain features, test mode introduction

  **Acceptance Criteria**: Runs once, permissions requested

- [ ] 15. E2E testing + bug fixes

  **What to do**: Test all flows, fix issues

  **Acceptance Criteria**: Core flows work

- [ ] 16. App store submission + GitHub

  **What to do**: Submit to Play/App Store, create GitHub repo with MIT license

  **Acceptance Criteria**: Submitted, open source

---

## Future Considerations (Post-v1)

- **AI integration**: TensorFlow Lite for on-device AI context (see ERTRIAGE as reference)
- **Ads**: AdMob integration
- **More voice triggers**: Language support
- **Background monitoring**: Igatha-style distress detection

---

## Final Verification

- [ ] F1. All toggles/configurable options work
- [ ] F2. Test mode verified (doesn't trigger real emergency)
- [ ] F3. Core SOS flows tested
- [ ] F4. Open source ready
