# Ladder Spawning Mod

## How To Install

Copy the .pk3 for your current Medal of Honor game (AA, BT, SH) and paste in the "Medal of Honor/main" folder (same folder as Pak0.pk3, Pak1.pk3, Pak2.pk3).
If using Elgan's Admin Pro mod, copy the .pk3 from the Elgan's Admin Pro folder instead (applies for all 3 Medal of Honor games).
Uninstall by deleting the .pk3 mod file from the "main" folder.

## Adding Ladders To Maps

Spawn an invisible ladder trigger_multiple (visible if using the "spawn laser" parameter).
All ladders will be 20+ units away from any walls to prevent players getting stuck when climbing (20 units behind origin's coords when facing ladder's angle).

Parameters: origin, angle for players to attach, ladder height (minimum = 70), ladder width from center (default = 13.5), thickness of ladder (default = 1) extending into the wall when facing ladder's angle, ...
... spawn solid clip (0 = no, 1 = yes), spawn laser (0 = no, 1 = 1 laser, 2 = 2 lasers outlining ladder, 3 = ladder with visible rungs), laser color [default = ( 0 0 1 ) or blue].

Example: exec global/spawnladder.scr ( -200 300 0 ) 180 500 13.5 1 0 0

Example2: local.ladder = thread global/spawnladder.scr::main ( -200 300 0 ) 180 500 13.5 1 1 3 ( 0 0 1 )

This mod will not work outside of UBER MODS unless nangla_aa_torso.st is also included, or unless another mike_torso.st file is modified.
In another mod's mike_torso.st (or similar torso.st), remove all 8 CAN_CLIMB_DOWN_LADDER, CAN_CLIMB_UP_LADDER conditions from these 2 states: LADDER_IDLE_LEFT, LADDER_IDLE_RIGHT.

Turn these...

	LADDER_UP_RIGHT             : FORWARD LOOKING_UP "-30" CAN_CLIMB_UP_LADDER
    LADDER_UP_RIGHT             : BACKWARD !LOOKING_UP "-30" CAN_CLIMB_UP_LADDER
    LADDER_DOWN_LEFT            : FORWARD !LOOKING_UP "-30" CAN_CLIMB_DOWN_LADDER
    LADDER_DOWN_LEFT            : BACKWARD LOOKING_UP "-30" CAN_CLIMB_DOWN_LADDER

into these...

    LADDER_UP_RIGHT             : FORWARD LOOKING_UP "-30"
    LADDER_UP_RIGHT             : BACKWARD !LOOKING_UP "-30"
    LADDER_DOWN_LEFT            : FORWARD !LOOKING_UP "-30"
    LADDER_DOWN_LEFT            : BACKWARD LOOKING_UP "-30"
