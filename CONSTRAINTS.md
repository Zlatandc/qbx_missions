
# CONSTRAINTS.md — Architectural Constraints for qbx_mission

This document defines strict rules that AI coding agents must follow when working
on this repository. These constraints prevent architectural errors and unstable code.

Agents (Codex, Copilot, Cursor, etc.) MUST read this file before implementing changes.

--------------------------------------------------
CORE ARCHITECTURE RULE
--------------------------------------------------

The system MUST remain SERVER AUTHORITATIVE.

Server responsibilities:

- mission spawning
- mission lifecycle
- zone management
- AI logic
- entity state tracking

Clients must NEVER:

- decide when missions spawn
- control mission state
- implement mission logic

Clients are allowed only to:

- spawn entities when instructed
- display UI
- show debug information

--------------------------------------------------
MODULE ISOLATION
--------------------------------------------------

Each system must remain in its dedicated module.

Do NOT merge systems into a single file.

Correct separation:

server/mission_manager.lua → mission creation and registry

server/zone_manager.lua → zone rotation and active zone tracking

server/ai_engine.lua → mission behavior and AI updates

client/spawn.lua → entity creation from server payload

client/admin_menu.lua → admin tools

client/debug_monitor.lua → debug display

--------------------------------------------------
MISSION SPAWN CONSTRAINT
--------------------------------------------------

Mission spawning must ONLY occur on the server.

The server must broadcast mission creation using:

TriggerClientEvent("qbx_mission:spawnMission", -1, payload)

Clients must NEVER create missions independently.

--------------------------------------------------
MISSION LIMIT
--------------------------------------------------

The system must enforce a strict maximum of:

10 active missions

The mission manager must verify the limit before spawning new missions.

--------------------------------------------------
ZONE CONSTRAINT
--------------------------------------------------

Missions may only spawn inside the ACTIVE ZONE.

Zone information is defined in:

config/zones.lua

Zone manager rotates zones every:

10 minutes

--------------------------------------------------
SPAWN POINT CONSTRAINT
--------------------------------------------------

Each mission must support exactly:

5 spawn points

Spawn point selection must be random.

The selected spawn must be validated against the active zone.

--------------------------------------------------
NETWORK SAFETY
--------------------------------------------------

All spawned entities must be networked.

Clients must not spawn local-only entities.

Vehicle and ped creation must follow FiveM network rules.

--------------------------------------------------
LOOP SAFETY
--------------------------------------------------

Every loop must include:

Wait()

No tight infinite loops are allowed.

AI tick interval:

1000 ms

--------------------------------------------------
PERFORMANCE LIMITS
--------------------------------------------------

The resource must remain efficient.

Target metrics:

- <2 ms server tick
- 60+ player compatibility
- minimal network traffic

--------------------------------------------------
CONFIGURATION RULES
--------------------------------------------------

All configurable values must remain in config files:

config/config.lua → global settings

config/missions.lua → mission definitions

config/zones.lua → zone definitions

Do not hardcode values in server or client scripts.

--------------------------------------------------
AI MODIFICATION RULE
--------------------------------------------------

When modifying AI behavior:

Do not change the mission lifecycle states.

Allowed states:

SPAWNING
ACTIVE
ENGAGED
COMBAT
LOOT_AVAILABLE
COMPLETED
FAILED

--------------------------------------------------
FINAL RULE
--------------------------------------------------

When implementing new features:

1. read README.md
2. read AGENTS.md
3. read ARCHITECTURE.md
4. read TASKS.md
5. read CONSTRAINTS.md

Then implement the change without breaking architecture.

--------------------------------------------------
END OF FILE
