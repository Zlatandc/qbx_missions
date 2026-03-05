
# ARCHITECTURE.md — qbx_mission System Architecture

This document describes the architecture of the qbx_mission FiveM resource.
AI agents and developers must follow this structure when implementing or modifying the system.

--------------------------------------------------
CORE PRINCIPLE
--------------------------------------------------

The system is SERVER AUTHORITATIVE.

Server responsibilities:
- mission spawning
- mission lifecycle
- zone management
- AI behavior
- entity state tracking

Client responsibilities:
- spawning entities from server payload
- UI
- debug monitor
- admin menu

Clients must never control mission logic.

--------------------------------------------------
MISSION FLOW
--------------------------------------------------

SPAWNING
↓
ACTIVE
↓
ENGAGED
↓
COMBAT
↓
LOOT_AVAILABLE
↓
COMPLETED or FAILED

Mission states are controlled by server/ai_engine.lua.

--------------------------------------------------
ZONE SYSTEM
--------------------------------------------------

The map is divided into 16 zones.

Each zone contains:
- center (vector3)
- radius

Only one zone can be active at a time.

Zone rotation interval:

10 minutes

Handled by:

server/zone_manager.lua

--------------------------------------------------
MISSION SYSTEM
--------------------------------------------------

10 mission types defined in:

config/missions.lua

Each mission contains:

label
pedModel
vehicleModel
spawnPoints

Each mission must support 5 spawn locations.

--------------------------------------------------
SPAWN SYSTEM
--------------------------------------------------

Mission spawning process:

1. Mission Manager selects mission type
2. Random spawn point selected
3. Spawn validated against active zone
4. Mission created
5. Payload broadcast to clients

Server event:

qbx_mission:spawnMission

--------------------------------------------------
MISSION REGISTRY
--------------------------------------------------

Active missions are stored in a server registry.

Mission object structure:

missionId
type
state
zone
vehicle
ped
spawnCoords

--------------------------------------------------
PERFORMANCE RULES
--------------------------------------------------

Maximum active missions:

10

AI tick interval:

1000 ms

All loops must include Wait().

--------------------------------------------------
NETWORKING
--------------------------------------------------

All entity creation must originate from server instructions.

Clients spawn entities but must use networked entities.

--------------------------------------------------
END
