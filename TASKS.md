
# TASKS.md — Development Roadmap for qbx_mission

This file lists tasks that AI agents should complete when developing the project.

--------------------------------------------------
PHASE 1 — CORE RESOURCE
--------------------------------------------------

Create resource structure:

fxmanifest.lua
config/
client/
server/
shared/

Ensure resource loads without errors.

--------------------------------------------------
PHASE 2 — ZONE SYSTEM
--------------------------------------------------

Implement:

server/zone_manager.lua

Features:

- 16 zones
- active zone tracking
- rotation every 10 minutes
- broadcast zone change event

--------------------------------------------------
PHASE 3 — MISSION CONFIG
--------------------------------------------------

Create:

config/missions.lua

Define:

10 mission types

Each mission must include:

label
pedModel
vehicleModel
spawnPoints (5 positions)

--------------------------------------------------
PHASE 4 — MISSION MANAGER
--------------------------------------------------

Create:

server/mission_manager.lua

Responsibilities:

- spawn missions
- enforce mission limit
- manage mission registry

--------------------------------------------------
PHASE 5 — CLIENT SPAWN SYSTEM
--------------------------------------------------

Create:

client/spawn.lua

Client listens for:

qbx_mission:spawnMission

Client spawns:

CreateVehicle
CreatePedInsideVehicle

--------------------------------------------------
PHASE 6 — AI ENGINE
--------------------------------------------------

Create:

server/ai_engine.lua

Checks:

player proximity
combat trigger
ped death
vehicle stuck

Tick interval:

1000 ms

--------------------------------------------------
PHASE 7 — ADMIN TOOLS
--------------------------------------------------

Create:

client/admin_menu.lua

Key:

F5

Functions:

start mission
stop mission
force zone
toggle debug

--------------------------------------------------
PHASE 8 — DEBUG MONITOR
--------------------------------------------------

Create:

client/debug_monitor.lua

Key:

F6

Displays:

mission ID
state
zone
ped HP
vehicle HP

--------------------------------------------------
PHASE 9 — OPTIMIZATION
--------------------------------------------------

Verify:

no duplicate spawning
server authoritative architecture
low server tick usage

--------------------------------------------------
PHASE 10 — FINAL REVIEW
--------------------------------------------------

AI agent must:

review all modules
verify architecture compliance
fix performance issues
