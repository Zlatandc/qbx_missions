
# TESTS.md — Testing Guide for qbx_mission

This document defines the testing procedures that AI agents and developers must
follow when validating the qbx_mission FiveM resource.

The goal is to ensure that the system works correctly before deploying it on a live server.

--------------------------------------------------
TESTING PRINCIPLES
--------------------------------------------------

Tests should verify:

• correct mission spawning
• correct zone rotation
• correct client/server synchronization
• no duplicate entities
• correct mission lifecycle transitions
• acceptable performance

Agents must simulate tests logically when possible.

--------------------------------------------------
TEST ENVIRONMENT
--------------------------------------------------

Recommended setup:

FiveM development server
1–4 players connected
server console visible
client F8 console visible

Ensure the resource is started with:

ensure qbx_mission

--------------------------------------------------
TEST 1 — RESOURCE STARTUP
--------------------------------------------------

Goal:

Verify that the resource loads correctly.

Steps:

1. Start the server
2. Load qbx_mission
3. Check server console

Expected result:

No errors appear in the console.

--------------------------------------------------
TEST 2 — ZONE SYSTEM
--------------------------------------------------

Goal:

Verify zone manager functionality.

Steps:

1. Start server
2. Observe zone_manager.lua logs
3. Wait for zone rotation

Expected result:

Active zone changes every 10 minutes.

Console example:

[MISSIONS] Active zone changed: X

--------------------------------------------------
TEST 3 — ADMIN MENU
--------------------------------------------------

Goal:

Verify admin tools.

Steps:

1. Press F5
2. Open admin menu
3. Select "Start Mission"

Expected result:

A mission spawn request is sent to the server.

--------------------------------------------------
TEST 4 — MISSION SPAWN
--------------------------------------------------

Goal:

Verify server authoritative spawning.

Steps:

1. Trigger mission start
2. Observe server console
3. Observe clients

Expected result:

Server logs mission creation.
Clients receive spawn event.
Vehicle and ped appear correctly.

--------------------------------------------------
TEST 5 — RANDOM SPAWN POINTS
--------------------------------------------------

Goal:

Ensure spawn locations are randomized.

Steps:

1. Start the same mission multiple times
2. Record spawn location

Expected result:

Spawn location varies among the 5 configured points.

--------------------------------------------------
TEST 6 — MISSION STATE MACHINE
--------------------------------------------------

Goal:

Verify mission lifecycle transitions.

Steps:

1. Spawn mission
2. Approach mission vehicle
3. Engage NPC

Expected state transitions:

SPAWNING → ACTIVE → ENGAGED → COMBAT

--------------------------------------------------
TEST 7 — NPC DEATH
--------------------------------------------------

Goal:

Verify loot state activation.

Steps:

1. Kill mission NPC
2. Observe mission state

Expected result:

Mission state becomes:

LOOT_AVAILABLE

--------------------------------------------------
TEST 8 — MISSION COMPLETION
--------------------------------------------------

Goal:

Verify mission cleanup.

Steps:

1. Collect loot
2. End mission

Expected result:

Mission state becomes:

COMPLETED

Entities are cleaned up.

--------------------------------------------------
TEST 9 — MISSION LIMIT
--------------------------------------------------

Goal:

Ensure mission cap enforcement.

Steps:

1. Start missions repeatedly
2. Reach limit

Expected result:

Server refuses to spawn missions beyond the limit.

--------------------------------------------------
TEST 10 — PERFORMANCE TEST
--------------------------------------------------

Goal:

Ensure acceptable server performance.

Steps:

1. Spawn multiple missions
2. Monitor server tick time

Expected result:

Server tick remains below:

2 ms

--------------------------------------------------
FINAL VALIDATION
--------------------------------------------------

Before release confirm:

✓ no server errors
✓ no duplicated entities
✓ correct zone rotation
✓ correct mission lifecycle
✓ acceptable performance

--------------------------------------------------
END OF FILE
