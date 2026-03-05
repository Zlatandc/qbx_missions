
# AGENTS.md — Development Guide for AI Coding Agents
Project: qbx_mission (FiveM Resource)

This file provides instructions for AI coding agents (Codex, Copilot, Cursor, etc.) working on this repository.
Agents MUST read this file before modifying code.

--------------------------------------------------
PROJECT OVERVIEW
--------------------------------------------------

Resource name:
qbx_mission

Type:
FiveM Lua resource implementing a dynamic mission system.

Architecture goals:

- Server authoritative mission system
- Dynamic zone based spawning
- Configurable mission types
- AI driven mission state machine
- High performance for large servers (60+ players)

Agents must maintain modular architecture and avoid monolithic files.

--------------------------------------------------
RESOURCE STRUCTURE
--------------------------------------------------

qbx_mission
│
├ fxmanifest.lua
│
├ config/
│   ├ config.lua
│   ├ missions.lua
│   └ zones.lua
│
├ client/
│   ├ spawn.lua
│   ├ admin_menu.lua
│   └ debug_monitor.lua
│
├ server/
│   ├ main.lua
│   ├ mission_manager.lua
│   ├ zone_manager.lua
│   ├ ai_engine.lua
│   ├ engage.lua
│   └ mission_state.lua
│
└ shared/
    └ utils.lua

Agents must respect this structure.
Do NOT merge modules unnecessarily.

--------------------------------------------------
SERVER AUTHORITATIVE RULE
--------------------------------------------------

The server controls:

- mission creation
- spawn location
- mission state
- AI logic

Clients ONLY:

- spawn entities
- render UI
- display debug information

Never move mission logic to client scripts.

--------------------------------------------------
ZONE SYSTEM
--------------------------------------------------

Zones are defined in:

config/zones.lua

Properties:

- 16 zones
- each zone has center + radius
- only one active zone at a time

Zone rotation interval:

10 minutes

Zone manager file:

server/zone_manager.lua

Broadcast event:

TriggerClientEvent("qbx_mission:zoneChanged",-1,ActiveZone)

--------------------------------------------------
MISSION CONFIGURATION
--------------------------------------------------

File:

config/missions.lua

Each mission contains:

label
pedModel
vehicleModel
spawnPoints

Example:

spawnPoints = {
    vector4(...),
    vector4(...),
    vector4(...),
    vector4(...),
    vector4(...)
}

Agents must support 5 random spawn points per mission.

--------------------------------------------------
MISSION SPAWNING
--------------------------------------------------

Handled by:

server/mission_manager.lua

Steps:

1. select mission type
2. select random spawn point
3. verify spawn inside active zone
4. create mission object
5. broadcast spawn payload

Event:

qbx_mission:spawnMission

Payload fields:

missionId
zone
spawnCoords
pedModel
vehicleModel

--------------------------------------------------
MISSION LIFECYCLE
--------------------------------------------------

States:

SPAWNING
ACTIVE
ENGAGED
COMBAT
LOOT_AVAILABLE
COMPLETED
FAILED

Transitions handled by:

server/ai_engine.lua

Tick interval:

1000 ms

--------------------------------------------------
MISSION LIMIT
--------------------------------------------------

Maximum active missions:

10

Agents must prevent spawning if limit reached.

--------------------------------------------------
CLIENT RESPONSIBILITIES
--------------------------------------------------

Client spawn file:

client/spawn.lua

Client must listen for:

qbx_mission:spawnMission

Client then spawns:

CreateVehicle
CreatePedInsideVehicle

Entities must be networked.

--------------------------------------------------
DEBUG SYSTEM
--------------------------------------------------

Key:

F6

Displays:

Mission ID
State
Zone
Ped HP
Vehicle HP

File:

client/debug_monitor.lua

--------------------------------------------------
ADMIN MENU
--------------------------------------------------

Key:

F5

Functions:

start mission
stop mission
force zone
toggle debug

File:

client/admin_menu.lua

--------------------------------------------------
PERFORMANCE RULES
--------------------------------------------------

Agents must ensure:

- no infinite loops without Wait()
- minimal network spam
- server authoritative control
- no duplicated entity spawning

Target performance:

< 2 ms server tick impact

--------------------------------------------------
CODE STYLE
--------------------------------------------------

Lua conventions:

- use local variables
- modular functions
- clear naming

Avoid:

global variables
client-side mission logic
duplicated systems

--------------------------------------------------
AGENT WORKFLOW
--------------------------------------------------

When implementing features:

1. read README.md
2. read AGENTS.md
3. analyze architecture
4. implement feature
5. verify compatibility
6. avoid breaking systems

Agents should modify only relevant modules.

--------------------------------------------------
END OF FILE
--------------------------------------------------
