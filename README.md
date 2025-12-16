# Ladder Spawning Mod

## How To Install

Copy the .pk3 for your current Medal of Honor game (AA, SH, BT) and paste in the "Medal of Honor/main" folder (same folder as Pak0.pk3, Pak1.pk3, Pak2.pk3).
If using Elgan's Admin Pro mod, copy the .pk3 from the Elgan's Admin Pro folder instead (applies for all 3 Medal of Honor games).
Uninstall by deleting the .pk3 mod file from the "main" folder.

## Adding Ladders To Maps

Spawn an invisible ladder trigger_multiple (visible if using the "spawn laser" parameter).
All ladders will be 20+ units away from any walls to prevent players getting stuck when climbing (20 units behind origin's coords when facing ladder's angle).
If the ladder cannot be climbed, the origin may need to be moved 1+ units away from walls or 1+ units above the floor.
Add an "example" line from below into an .scr map script after "level waittill prespawn". Change the origin, angle, height parameters, etc accordingly.

Parameters: origin, angle for players to attach, ladder height (minimum = 70), ladder width from center (default = 13.5), thickness of ladder (default = 1) extending into the wall when facing ladder's angle, ...
... spawn solid clip (0 = no, 1 = yes), spawn laser (0 = no, 1 = 1 laser, 2 = 2 lasers outlining ladder, 3 = ladder with visible rungs), laser color [default = ( 0 0 1 ) or blue].

Example: exec global/spawnladder.scr ( -200 300 0 ) 180 500 13.5 1 0 0

Example2: local.ladder = thread global/spawnladder.scr::main ( -200 300 0 ) 180 500 13.5 1 1 3 ( 0 0 1 )

## Making Compatible With All Other Mods

Spawned ladders will not work outside of this .pk3 mod unless the global "torso.st" file is included, or unless another mike_torso.st file is modified.

In another mod's mike_torso.st (or similar torso.st): remove all 8 CAN_CLIMB_DOWN_LADDER, CAN_CLIMB_UP_LADDER conditions from these 2 states: LADDER_IDLE_LEFT, LADDER_IDLE_RIGHT.
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

In "state GET_ON_LADDER" and "state LATCH_ONTO_LADDER", add this line inside the "entrycommands": exec global/legs_movement.scr "ON_LADDER".
In "state FINISHED_GET_OFF_LADDER" and "state JUMP_OFF_LADDER", add this line inside the "entrycommands": exec global/legs_movement.scr "OFF_LADDER"
Examples below...

state GET_ON_LADDER
{
	movetype climbwall


	// these entry commands ensure that the player 
	// will be standing when he gets off the ladder
	entrycommands
	{
		modheight "stand"
		movementstealth "1.0"
		moveposflags "standing"
		exec global/legs_movement.scr "ON_LADDER"
	}

	action
	{
		ladder_geton_top		: AT_TOP_OF_LADDER
		ladder_geton_bottom		: default
	}
	
	states
	{
//		STAND					: KILLED
		FINISHED_GET_OFF_LADDER	: KILLED
	
		// we go to LADDER_LEFT because the left hand will be up once we're on
		LADDER_IDLE_LEFT		: ANIMDONE_TORSO
	}
}

// This is for when the player grabs the ladder while not on the ground
state LATCH_ONTO_LADDER
{
	movetype climbwall

	// these entry commands ensure that the player 
	// will be standing when he gets off the ladder
	entrycommands
	{
		modheight "stand"
		movementstealth "1.0"
		moveposflags "standing"
		tweakladderpos // make sure we're positioned properly on the ladder
		exec global/legs_movement.scr "ON_LADDER"
	}
	
	states
	{
//		LADDER_IDLE_LEFT		: default
		LADDER_DOWN_LEFT		: default
	}
}

state FINISHED_GET_OFF_LADDER
{
	movetype legs

	entrycommands
	{
		unattachfromladder
		safeholster 0 // pull weapon back out if we put it away to get on the ladder
		exec global/legs_movement.scr "OFF_LADDER"
	}

	states
	{
		STAND					: default
	}
}

// same as FINISHED_GET_OFF_LADDER, except that the player is jumping off
state JUMP_OFF_LADDER
{
	movetype legs
	
	entrycommands
	{
		unattachfromladder
		safeholster 0 // pull weapon back out if we put it away to get on the ladder
		jumpxy -70 0 150
		exec global/legs_movement.scr "OFF_LADDER"
	}

	states
	{
		STAND			: default
	}
}

<><><> <><><>