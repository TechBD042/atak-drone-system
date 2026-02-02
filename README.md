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

### Phase 1: Learning & Prerequisites ✅ COMPLETE
Understanding TAK ecosystem and preparing your environment.
- Learned TAK architecture (Server, ATAK, MediaMTX)
- Set up Docker via Colima
- MediaMTX video server running

### Phase 2: TAK Server Installation ✅ COMPLETE
Setting up the central coordination server.
- TAK Server 5.6 running in Docker
- Web admin accessible via https://tak.cccops.com
- Client connections via Tailscale (100.77.122.114:8089)
- User authentication configured
- Certificate generation scripts ready

### Phase 3: ATAK Client Setup 🔄 IN PROGRESS
Installing and configuring the Android app.
- [ ] Install ATAK on Android device
- [ ] Install Tailscale on Android device
- [ ] Import data package (.zip with certs)
- [ ] Connect to TAK Server
- [ ] Verify map and position sharing

### Phase 4: Drone Integration ⏳ UPCOMING
Connecting drones to the system for video and control.
- [ ] Configure drone video feed to MediaMTX
- [ ] Set up ATAK UAS (drone) plugin
- [ ] Test video streaming to ATAK
- [ ] Configure drone telemetry to TAK

### Phase 5: Testing & Operations ⏳ UPCOMING
End-to-end testing and operational procedures.
- [ ] Multi-device team test
- [ ] Drone flight with live tracking
- [ ] Video feed verification
- [ ] Document operational procedures

## Server Information

| Service | Access Method | Address | Port |
|---------|---------------|---------|------|
| TAK Server (Web Admin) | Cloudflare Tunnel | https://tak.cccops.com | 443 |
| TAK Server (Client TLS) | Tailscale VPN | 100.77.122.114 | 8089 |
| TAK Server (Local) | Local Network | 10.0.0.217 | 8089, 9443 |
| MediaMTX | Local/Tailscale | 100.77.122.114 | 8554, 8889 |

### TAK Server Credentials

| Username | Password | Role |
|----------|----------|------|
| admin | TakAdmin-Gabe-12345 | Administrator |

### Connection Methods

**For ATAK/iTAK Clients (via Tailscale):**
- Server: `100.77.122.114`
- Port: `8089`
- Protocol: SSL/TLS
- Certificate Password: `atakatak`

**For Web Admin (Public):**
- URL: https://tak.cccops.com
- Login with username/password above

## Prerequisites

- [x] Android device (tablet recommended) for ATAK
- [x] TAK.gov account (free registration)
- [x] Docker installed (via Colima)
- [x] MediaMTX running
- [x] Tailscale VPN configured
- [x] Cloudflare Tunnel configured

## Creating User Certificates

To add new team members to the TAK Server:

```bash
cd ~/tak-server
./create-user-cert.sh <username>
```

This generates a data package at `~/tak-server/data-packages/<username>-TAK-Connection.zip`

Send this file to the team member along with:
- Certificate password: `atakatak`
- Instructions to install Tailscale and join the network

### Manual Certificate Creation

```bash
# Generate certificate inside container
docker exec takserver bash -c "cd /opt/tak/certs && STATE=California CITY=Oakland ORGANIZATIONAL_UNIT=CCC ./makeCert.sh client <username>"

# Files created in: ~/tak-server/takserver-docker-5.6-RELEASE-6/tak/certs/files/
```

## Creating Web Login Users

To create username/password logins for the web admin:

```bash
docker exec takserver java -jar /opt/tak/utils/UserManager.jar usermod -A -p '<password>' <username>
```

Password requirements: 15+ characters, uppercase, lowercase, number, hyphen (avoid special chars like !@#)

## Resources

- [TAK.gov](https://tak.gov) - Official TAK downloads
- [ATAK User Guide](https://tak.gov/products/atak)
- [TAK Server Documentation](https://tak.gov/products/tak-server)
- [CCC MediaMTX Server](../MediaMTX-Server)

## Startup After Reboot

If your Mac restarts, run these commands to bring everything back online:

```bash
# 1. Start Docker (Colima)
colima start --cpu 4 --memory 6

# 2. TAK Server containers auto-start, but verify:
docker ps

# 3. Cloudflared usually auto-starts, but if needed:
brew services restart cloudflared

# 4. Verify services
curl -sk https://tak.cccops.com | head -5
```

## Video References from Boss

1. [ATAK Drone Flying Basics](https://www.youtube.com/watch?v=cza3E6FmYbo)
2. [ATAK Additional Tutorial](https://www.youtube.com/watch?v=tWRxwb8_9nA)
3. [ATAK Advanced Features](https://www.youtube.com/watch?v=3JNP3uRVwmo)
