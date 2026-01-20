# ATAK Drone System

CCC Drone Control & Situational Awareness System using TAK (Team Awareness Kit).

## What is This Project?

This project sets up a complete drone control and monitoring system using military-grade situational awareness technology. When complete, you'll be able to:

- Control multiple drones from a single Android tablet
- See live video feeds from all drones
- Track drone positions on a map in real-time
- Share situational awareness with your team

## Understanding the Components

### TAK Server (The Brain)
**What it is:** A central server that coordinates all communication between devices.

**Think of it like:** A group chat server - everyone connects to it to share information.

**What it does:**
- Stores map data and overlays
- Routes messages between devices
- Manages user authentication
- Keeps track of everyone's position

### ATAK - Android Team Awareness Kit (The Controller)
**What it is:** An Android app originally built for military situational awareness.

**Think of it like:** Google Maps + Walkie Talkie + Drone Controller in one app.

**What it does:**
- Shows your position and teammates on a map
- Displays live drone video feeds
- Sends commands to drones
- Shares markers, routes, and alerts with team

### MediaMTX (The Video Highway)
**What it is:** A video streaming server (you already have this running!).

**Think of it like:** A TV broadcast station that takes video from drones and sends it to viewers.

**What it does:**
- Receives video streams from drones
- Converts to different formats (RTSP, RTMP, WebRTC)
- Distributes video to ATAK and other viewers

## System Architecture

```
                    ┌─────────────────┐
                    │   TAK Server    │
                    │  (Coordinator)  │
                    └────────┬────────┘
                             │
         ┌───────────────────┼───────────────────┐
         │                   │                   │
         ▼                   ▼                   ▼
    ┌─────────┐        ┌─────────┐        ┌─────────┐
    │  ATAK   │        │  ATAK   │        │  ATAK   │
    │ Device 1│        │ Device 2│        │ Device 3│
    └─────────┘        └─────────┘        └─────────┘
         │
         │ Video Request
         ▼
    ┌─────────────┐         ┌─────────────┐
    │  MediaMTX   │◄────────│   Drone     │
    │  (Video)    │  Stream │   Camera    │
    └─────────────┘         └─────────────┘
```

## Project Phases

### Phase 1: Learning & Prerequisites
Understanding TAK ecosystem and preparing your environment.

### Phase 2: TAK Server Installation
Setting up the central coordination server.

### Phase 3: ATAK Client Setup
Installing and configuring the Android app.

### Phase 4: Drone Integration
Connecting drones to the system for video and control.

### Phase 5: Testing & Operations
End-to-end testing and operational procedures.

## Server Information

| Service | Location | Port |
|---------|----------|------|
| TAK Server | Mac Desktop (10.0.0.217) | 8089, 8443 |
| MediaMTX | Mac Desktop (10.0.0.217) | 8554, 8889 |

## Prerequisites

- [ ] Android device (tablet recommended) for ATAK
- [ ] TAK.gov account (free registration)
- [ ] Docker installed (already done!)
- [ ] MediaMTX running (already done!)

## Resources

- [TAK.gov](https://tak.gov) - Official TAK downloads
- [ATAK User Guide](https://tak.gov/products/atak)
- [TAK Server Documentation](https://tak.gov/products/tak-server)
- [CCC MediaMTX Server](../MediaMTX-Server)

## Video References from Boss

1. [ATAK Drone Flying Basics](https://www.youtube.com/watch?v=cza3E6FmYbo)
2. [ATAK Additional Tutorial](https://www.youtube.com/watch?v=tWRxwb8_9nA)
3. [ATAK Advanced Features](https://www.youtube.com/watch?v=3JNP3uRVwmo)
