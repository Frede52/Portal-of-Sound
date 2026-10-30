# Portal of Sound – IT Product Design Project

**A tangible, screen-free interface that lets Game Masters control ambient music and sound effects during Tabletop Role-Playing Games (TTRPGs) by placing minifigures on a physical portal.**

This project was developed as part of the course *IT-Product Design Project 2026* at Aarhus University. It explores how a tangible user interface can reduce the cognitive load Game Masters (GMs) face when managing sound during a session, without pulling their attention onto another screen.

**Authors:** Frederik Arno Kronborg, Anders Kornerup Kok Larsen, Oscar Skov Dinesen
**Supervised by:** Clemens Nylandsted Klokmose, Minna Pakanen & Simon Hoggan Christensen

---

## Problem Statement

TTRPGs are collaborative storytelling games that rely heavily on atmosphere and immersion. The Game Master facilitates the rules, story, and pacing, and is already juggling so many responsibilities that sound and music are usually the first thing to be dropped, even though both GMs and players agree it significantly improves immersion. Through ethnographic fieldwork (observation, interviews, and a questionnaire with 93 respondents) we found that existing solutions force the GM onto a screen — flipping through YouTube tabs or Spotify playlists — breaking contact with players and adding to an already heavy mental load.

Our design question became:

> *"How can we provide GMs with a tangible interactive IT product that supports and extends their current sound setup, and reduces the cognitive load associated with managing sounds during a session, in order to promote more frequent use of sounds to accompany storytelling?"*

## The Solution

Portal of Sound consists of two parts: a physical platform used during play, and a companion website used during prep time before a session.

- **Toys-to-life interaction:** Inspired by Skylanders and Nintendo amiibo, each minifigure carries an NFC/RFID tag. Placing it on the portal instantly triggers its associated sound; removing it stops the sound.
- **Zero-UI during play:** All sound configuration (choosing tracks, naming figures, organizing campaigns) happens beforehand on the website, so the GM never has to touch a screen mid-session — just place or remove a figure.
- **Physical volume mapping:** Following user testing of three interaction concepts (a web UI slider, a three-level platform, and continuous positioning), we settled on a **three-level hexagonal platform**, where the height of the platform a figure sits on maps to its volume — enabling GMs to adjust sound "eyes-free," by feel alone.
- **Light feedback:** An LED ring diffused through a frosted acrylic top plate communicates connection status and confirms when a minifigure has been detected, keeping visibility for the whole table, not just the GM.
- **Format-agnostic figures:** Anything with an NFC sticker can act as a "minifigure" — a 3D-printed tree, a laser-cut symbol, or even a real object like a matchbox for the sound of a struck match.

## Design Process

Our process was grounded in Contextual Design and the five stages of design thinking, moving iteratively between fieldwork and prototyping:

1. **Observation** of a live TTRPG session at a local roleplaying association (Elysion).
2. **Semi-structured interviews** with regular players and GMs, supplemented by a **questionnaire** (93 responses).
3. **Sequence and cultural models** to identify workflow breakdowns and the social pressures shaping the GM role.
4. **Idea generation and low-fidelity prototyping** (sketches, blue foam) to explore form (circle, square, hexagon, book) — hexagon won out for its existing TTRPG associations.
5. **Technical experiments** on a breadboard to validate that a single ESP32 could handle six RFID readers without signal or power issues.
5. **Field evaluations** with real GMs testing the functional prototype during their own sessions, followed by a dedicated **UX evaluation** comparing three volume-control interactions.

## Technical Implementation

The system has four main components: the physical **portal**, RFID-tagged **minifigures**, a local **backend**, and a **frontend** website.

### Hardware Stack

- **Microcontroller:** ESP32
- **Sensors:** 6× RC522 RFID readers (SPI), wired in parallel and power-cycled individually to avoid overloading the ESP32
- **Feedback:** Addressable LED ring diffused through a frosted, laser-cut acrylic top plate
- **Enclosure:** PETG (matte black, 3D printed), split into three hexagonal platforms representing three volume levels
- **Connectivity:** WiFi, with a fallback setup portal and mDNS discovery of the backend on the local network

### Software

- **Portal firmware:** Detects minifigures entering/leaving the portal and streams events over a **WebSocket** connection to minimize latency between placement and playback
- **Backend:** Python, serving both an HTTP API for the frontend and a WebSocket server for the portal; pre-loads all configured audio into memory for instant playback and handles priority-based volume balancing between simultaneous sounds
- **Database:** SQLite, storing campaigns, minifigures, sounds, and reader configuration
- **Frontend:** Svelte (HTML, TypeScript, CSS), used for pre-session configuration — campaigns, minifigure registration via a guided "wizard" flow, and a live overview of what's currently active on the portal

## Evaluation Highlights

- An initial concept evaluation confirmed the core pain point and met the concept with strongly positive reactions.
- Field testing with real GMs during their own sessions showed the tool made sound easier and more fun to manage, encouraging GMs to experiment with sound effects they'd normally skip — though it also surfaced two key needs: per-figure volume control and clearer disconnection feedback.
- A dedicated UX evaluation of three volume-control concepts led us to the three-level hexagonal platform, chosen for requiring the least attention and supporting eyes-free, intuitive use — best matching our goal of keeping the tool "ready-to-hand."
- All five evaluation participants rated their likelihood of using the concept in future sessions 4 or 5 out of 5.

## Future Work

- Merge the functional and aesthetic prototypes into a single, fully working unit
- Better support for improvisation, e.g. a pause/override control or figures with multiple selectable sounds (a goblin with both a friendly and an angry variant)
- Design dedicated affordances and figures for player-driven sound, beyond GM use
- User-test the website UI and the full prep-to-play flow as a whole

## Links

- Video demo: https://www.youtube.com/watch?v=TGqnMRk64uA
- Source code (AU login required): https://gitlab.au.dk/au790437/portal_of_sound

## About

Portal of Sound is a tangible user interface for reducing cognitive load when using audio in tabletop roleplaying games.
