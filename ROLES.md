
# ROLES.md — AI Development Roles for qbx_mission

This file defines specialized roles that AI coding agents should simulate when
working on the qbx_mission repository. Using role separation improves code quality
and reduces architectural mistakes.

Agents should conceptually follow these roles in order.

--------------------------------------------------
ROLE 1 — SOFTWARE ARCHITECT
--------------------------------------------------

Responsibilities:

• Read README.md
• Read ARCHITECTURE.md
• Read CONSTRAINTS.md

Design the system architecture before coding.

Ensure:

- modular file structure
- correct server/client separation
- mission lifecycle design
- zone system design
- configuration system layout

The architect must NOT write large amounts of code.
The architect defines the system structure.

--------------------------------------------------
ROLE 2 — SYSTEM ENGINEER
--------------------------------------------------

Responsibilities:

Implement the systems defined by the architect.

Files typically implemented by this role:

server/mission_manager.lua
server/zone_manager.lua
server/ai_engine.lua
server/mission_state.lua

Rules:

• Follow CONSTRAINTS.md
• Respect server authoritative architecture
• Keep modules independent

--------------------------------------------------
ROLE 3 — CLIENT ENGINEER
--------------------------------------------------

Responsibilities:

Implement client-side systems.

Files:

client/spawn.lua
client/admin_menu.lua
client/debug_monitor.lua

Client responsibilities:

- spawn entities from server payload
- render UI
- display debug information

Clients must NEVER:

- control mission spawning
- control mission state

--------------------------------------------------
ROLE 4 — NETWORK ENGINEER
--------------------------------------------------

Responsibilities:

Design safe event communication between server and clients.

Key events:

qbx_mission:spawnMission
qbx_mission:zoneChanged

Ensure:

- minimal network traffic
- correct payload structure
- no duplicate entity spawning

--------------------------------------------------
ROLE 5 — PERFORMANCE ENGINEER
--------------------------------------------------

Responsibilities:

Ensure resource efficiency.

Targets:

• < 2 ms server tick
• support 60+ players
• avoid excessive loops

Rules:

Every loop must contain Wait().

AI tick interval:

1000 ms

--------------------------------------------------
ROLE 6 — CODE REVIEWER
--------------------------------------------------

Responsibilities:

Review all generated code.

Check:

• architecture compliance
• constraint violations
• duplicated logic
• performance issues

If issues are found:

refactor the code.

--------------------------------------------------
ROLE 7 — QA TESTER
--------------------------------------------------

Responsibilities:

Simulate testing scenarios.

Verify:

• missions spawn correctly
• zone rotation works
• admin tools function
• debug monitor displays correct data

Ensure no runtime errors.

--------------------------------------------------
WORKFLOW ORDER
--------------------------------------------------

Agents should follow this order:

1. Architect
2. System Engineer
3. Client Engineer
4. Network Engineer
5. Performance Engineer
6. Code Reviewer
7. QA Tester

--------------------------------------------------
FINAL RULE
--------------------------------------------------

Before implementing any feature agents must read:

README.md
AGENTS.md
ARCHITECTURE.md
TASKS.md
CONSTRAINTS.md
ROLES.md

Then implement the feature following all architectural rules.

--------------------------------------------------
END OF FILE
