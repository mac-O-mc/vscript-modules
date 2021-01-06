# Flamboyance
A simple module that makes use of the VScript function `ClientPrint`. Instead of declaring custom constants or accessing enums located in another script, you can instead just pass the enums as strings. Additionally, any exceptions that occurs with any use of this module's function will be sent to the chatbox, with no errors in console.

Example code used with valid VScript functions:
#### With module:
```Squirrel
function ShotgunOrSMG( string )
{
	local gave_weapon = "none!";
	if (string == Shotgun)
		terrorplayer.GiveItem("shotgun_chrome");
		gave_weapon = "Shotgun"
	else if (string == "SMG")
		terrorplayer.GiveItem("smg_mp5");
		gave_weapon = "SMG"

	::Flamboyance.PrintToChatAll("Gave a weapon: " + gave_weapon, "OliveGreen");
}
```
#### Without module:
```Squirrel
function ShotgunOrSMG( string )
{
	local gave_weapon = "none!";
	if (string == Shotgun)
		terrorplayer.GiveItem("shotgun_chrome");
		gave_weapon = "Shotgun"
	else if (string == "SMG")
		terrorplayer.GiveItem("smg_mp5");
		gave_weapon = "SMG"

	ClientPrint(null, DirectorScript.HUD_PRINTTALK, "\x05Gave a weapon: " + gave_weapon);
}

//------------------------------------------------------
// or

const Beign = "\x01"	// default color
const BrightGreen = "\x03"
const Orange = "\x04"
const OliveGreen = "\x05"

const HUD_PRINTNOTIFY = 1
const HUD_PRINTCONSOLE = 2
const HUD_PRINTTALK = 3
const HUD_PRINTCENTER = 4

function ShotgunOrSMG( string )
{
	local gave_weapon = "none!";
	if (string == Shotgun)
		terrorplayer.GiveItem("shotgun_chrome");
		gave_weapon = "Shotgun"
	else if (string == "SMG")
		terrorplayer.GiveItem("smg_mp5");
		gave_weapon = "SMG"

	ClientPrint(null, HUD_PRINTTALK, OliveGreen+"Gave a weapon: " + gave_weapon);
}
```

###### _Semicolons aren't actually needed in Squirrel, but can be used for organization purposes._

## Usage
In your VScript file, add the following line:
```Squirrel
IncludeScript("modules/flamboyance.nut")
```
### Enums
To use the Enums, pass only the key under `EnumKey` as strings.
#### Colorcode
EnumKey 	| Value
----------- | ---------------
Beign   	| \x01 (_string_)
BrightGreen | \x03 (_string_)
Orange 		| \x04 (_string_)
OliveGreen 	| \x05 (_string_)

#### Print Destinations
EnumKey          | Value         | Description
---------------- | ------------- | -----------
HUD_PRINTNOTIFY  | 1 (_integer_) | 
HUD_PRINTCONSOLE | 2 (_integer_) | Prints to console.
HUD_PRINTTALK    | 3 (_integer_) | Prints to the chat box. Supports custom color codes. (Also apperas with colors in Console)
HUD_PRINTCENTER  | 4 (_integer_) | 

### Functions
#### __::Flamboyance.PrintToChatAll(string, colorcode = "Orange")__
Prints to all clients's textboxes with the desired string and colorcode enum. (see above)
```Squirrel
const ZOMBIE_TANK = 8

function OnGameEvent_player_incapacitated( params )
{
	local Chance = RandomInt(1,3)
	if( Chance >= 2)
	{
		local player = GetPlayerFromUserID( params.userid )
		if( player.GetZombieType() == ZOMBIE_TANK )
		{
			::Flamboyance.PrintToChatAll("Tank Incapacitated!", "OliveGreen")
		}
	}
}
```

#### __::Flamboyance.CustomizedPrint(string, client, printdest = 3, colorcode = "Orange")__
Print a message of any valid colorcode to any client you want (pass `null` to send to all clients), and which Print Destination to use. Very similar to `ClientPrint`. 
```Squirrel
function EHandleToPlayer(ehandle) {
	local player = null
	while (player = Entities.FindByClassname(player, "player")) {
		
		if (ehandle == player.GetEntityHandle()) {
			local ListenServerHost = GetListenServerHost();
			if (ListenServerHost) {
				::Flamboyance.CustomizedPrint("EHandleToPlayer: Found match",ListenServerHost,"HUD_PRINTTALK","BrightGreen")
			}
			return player
		}
	}
	return null
}
```

#### Notes:
- Don't pass "\x05" as strings in the 'colorcode' variable section, or your resulting string will send nothing.
- The `::` is optional and can be omitted when calling the functions, may be a VScript only quirk.
