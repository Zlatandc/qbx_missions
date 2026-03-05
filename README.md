README — QBX Mission (Enterprise V12)
Resource Name
qbx_mission

FiveM resource that implements a dynamic mission system with zone-based spawning.

The system is designed for high-population servers and must support:

60+ players
10 active missions
minimal server load
Core Concept

The mission system is server-authoritative.

This means:

SERVER decides everything
CLIENT only spawns entities

The server controls:

mission creation

spawn location

mission state

AI logic

Mission Lifecycle

Every mission follows this lifecycle:

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
Map Zone System

The map is divided into 16 zones.

Only one zone is active at a time.

Zones rotate automatically.

Rotation interval:

10 minutes

Example structure:

Zones = {

[1] = { center = vector3(-1037.0,-2737.0,20.0), radius = 500 },
[2] = { center = vector3(-75.0,-818.0,326.0), radius = 500 },
[3] = { center = vector3(215.0,-925.0,30.0), radius = 500 },
[4] = { center = vector3(1200.0,-1400.0,35.0), radius = 500 },

...

[16] = { center = vector3(3000.0,500.0,50.0), radius = 500 }

}
Zone Manager

File:

server/zone_manager.lua

Responsibilities:

track current active zone

rotate zone every 10 minutes

broadcast zone changes to clients

Example event:

TriggerClientEvent("qbx_mission:zoneChanged",-1,ActiveZone)
Mission Types

The system supports 10 configurable mission types.

Configuration file:

config/missions.lua

Structure:

Config.Missions = {

[1] = {
label = "Smuggler Convoy",

pedModel = "g_m_y_mexgoon_02",

vehicleModel = "sultanrs",

spawnPoints = {

vector4(215.0,-925.0,30.0,180.0),
vector4(1200.0,-1400.0,35.0,90.0),
vector4(-1037.0,-2737.0,20.0,270.0),
vector4(1700.0,3250.0,40.0,180.0),
vector4(-450.0,6000.0,30.0,180.0)

}

},

...

}
Spawn System

Each mission has:

5 random spawn points

When the server spawns a mission:

mission type selected
↓
random spawn point selected
↓
spawn must be inside active zone
↓
mission created
Server Authoritative Spawning

Mission spawning must be server-controlled.

The server sends a payload to all clients.

Example event:

TriggerClientEvent("qbx_mission:spawnMission",-1,payload)

Payload contains:

missionId
zone
spawnCoords
pedModel
vehicleModel
Client Spawn

Client file:

client/spawn.lua

Clients listen for:

RegisterNetEvent("qbx_mission:spawnMission")

Then spawn:

CreateVehicle
CreatePedInsideVehicle

Entities must be networked.

AI System

File:

server/ai_engine.lua

Runs every:

1000 ms

Responsibilities:

detect player proximity

transition mission states

detect stuck vehicles

detect ped death

enable loot

Loot System

When the mission NPC dies:

state → LOOT_AVAILABLE

Loot is placed inside the vehicle trunk.

Example:

exports["qs-inventory"]:AddToTrunk(...)
Admin Menu

File:

client/admin_menu.lua

Key:

F5

Functions:

start mission
stop mission
force zone
toggle debug
Debug Monitor

File:

client/debug_monitor.lua

Key:

F6

Displays:

Mission ID
State
Zone
Ped HP
Vehicle HP
Mission Manager

File:

server/mission_manager.lua

Responsibilities:

track active missions
limit active missions
start missions
stop missions

Maximum active missions:

10
Anti-Lag Design

The system must ensure:

no duplicate spawns
no client-controlled spawning
distance checks for AI
zone-based filtering
Resource Structure
qbx_mission
│
├ fxmanifest.lua
│
├ config
│   ├ config.lua
│   ├ missions.lua
│   └ zones.lua
│
├ client
│   ├ spawn.lua
│   ├ admin_menu.lua
│   └ debug_monitor.lua
│
├ server
│   ├ main.lua
│   ├ mission_manager.lua
│   ├ zone_manager.lua
│   ├ ai_engine.lua
│   ├ engage.lua
│   └ mission_state.lua
│
└ shared
    └ utils.lua
Performance Target

The system must support:

60 players
10 missions
low server tick usage
Expected Result

The final system should behave like:

GTA Online Freemode Events

Dynamic missions appearing around the map.

Commit Message
Initial implementation of qbx_mission dynamic mission system
