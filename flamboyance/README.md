# Flamboyance
A simple module that makes use of the VScript function `ClientPrint`. Instead of declaring custom constants or accessing enums located in another script, you can instead just pass the enums as strings.

#### With module:
```Squirrel
function OnGameEvent_ammo_pickup( params )
{
	local terrorplayer = GetPlayerFromUserID(params.userid)
	if (RandomInt(1,3) < 3)
		terrorplayer.GiveItem("pumpshotgun");
	else
		terrorplayer.GiveItem("shotgun_chrome");

	::Flamboyance.PrintToChatAll("OnGameEvent_ammo_pickup: fired", "OliveGreen");
}
```
#### Without module:
```Squirrel
function OnGameEvent_ammo_pickup( params )
{
	local terrorplayer = GetPlayerFromUserID(params.userid)
	if (RandomInt(1,3) < 3)
		terrorplayer.GiveItem("pumpshotgun");
	else
		terrorplayer.GiveItem("shotgun_chrome");

	ClientPrint(null, DirectorScript.HUD_PRINTTALK, "\x05OnGameEvent_ammo_pickup: fired");
}

//------------------------------------------------------
// or

const Beign = "\x01"	// default color
const BrightGreen = "\x03"
const Orange = "\x04"
const OliveGreen = "\x05"

const "HUD_PRINTNOTIFY" = 1
const "HUD_PRINTCONSOLE" = 2
const "HUD_PRINTTALK" = 3
const "HUD_PRINTCENTER" = 4

function OnGameEvent_ammo_pickup( params )
{
	local terrorplayer = GetPlayerFromUserID(params.userid)
	if (RandomInt(1,3) < 3)
		terrorplayer.GiveItem("pumpshotgun");
	else
		terrorplayer.GiveItem("shotgun_chrome");

	ClientPrint(null, DirectorScript.HUD_PRINTTALK, "OnGameEvent_ammo_pickup: fired");
}
```
(Semicolons aren't actually needed in Squirrel, but can be used for organization purposes.)
