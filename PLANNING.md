
# PLANNING.md — Development Planning for qbx_mission

This document forces AI agents and developers to PLAN before writing code.

The goal is to reduce architectural mistakes and ensure that every new feature
is implemented according to the system architecture.

Agents must always read this file before starting a new implementation task.

--------------------------------------------------
PLANNING PRINCIPLE
--------------------------------------------------

Before writing code, the agent must:

1. Read all documentation
2. Understand the current project state
3. Define the implementation plan
4. Identify affected modules
5. Confirm constraints
6. Only then begin coding

Documentation to read first:

README.md
AGENTS.md
ARCHITECTURE.md
TASKS.md
CONSTRAINTS.md
ROLES.md
TESTS.md
STATE.md
PLANNING.md

--------------------------------------------------
PLANNING TEMPLATE
--------------------------------------------------

Before implementing a new feature, the agent must outline:

FEATURE NAME:
(short description)

GOAL:
(what problem the feature solves)

AFFECTED FILES:
(list server/client/config files to modify)

ARCHITECTURAL IMPACT:
(does it affect mission manager, AI engine, zones, etc.)

CONSTRAINT CHECK:
(confirm that CONSTRAINTS.md will not be violated)

PERFORMANCE IMPACT:
(estimate CPU/network impact)

TEST PLAN:
(which tests from TESTS.md will validate this feature)

--------------------------------------------------
EXAMPLE PLAN
--------------------------------------------------

FEATURE:
Dynamic convoy mission

GOAL:
Spawn a convoy with multiple AI vehicles.

AFFECTED FILES:

server/mission_manager.lua
server/ai_engine.lua
config/missions.lua
client/spawn.lua

ARCHITECTURAL IMPACT:

Adds multiple vehicles per mission.

CONSTRAINT CHECK:

Server-authoritative spawning respected.

PERFORMANCE IMPACT:

Minimal increase in AI processing.

TEST PLAN:

Run TEST 4 (mission spawn)
Run TEST 6 (mission lifecycle)

--------------------------------------------------
IMPLEMENTATION RULE
--------------------------------------------------

Agents must ALWAYS produce a plan before coding.

The plan must be shown to the user or logged internally.

After the plan is validated, code implementation may begin.

--------------------------------------------------
STATE UPDATE RULE
--------------------------------------------------

After completing implementation:

Update STATE.md with:

• new version number
• implemented feature
• updated next tasks

--------------------------------------------------
END OF FILE
