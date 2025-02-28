# arma-3-life-house-security

This is a house security script for ArmA 3 RPG Life.

<a href="https://www.buymeacoffee.com/julianbauer" target="_blank"><img src="https://cdn.buymeacoffee.com/buttons/v2/default-red.png" alt="Buy Me A Coffee" style="height: 60px !important;width: 217px !important;" ></a>

## Installation

A working installation of ArmA Life RPG Framework is required for a successful installation. Modifying the ArmA Life RPG Framework could cause errors – feel free to connect to our discord if you have a problem.

### Step 1

Download the newest release and extract the archive. Copy the folder "alarm" in your “cation” folder that can be found in the  root folder (subsequently called \<mission\>) of your mission.

### Step 2

Open \<mission\>/cation/cation_functions.cpp and insert

`#include "alarm\functions.cpp"`

and save the file.

### Step 3

Open \<mission\>/cation/cation_master.cpp and insert

`#include "alarm\config.cpp"`

and save the file.

### Step 4

Open \<mission\>/cation/cation_remoteExec.cpp and insert

`#include "alarm\remoteExec.cpp"`

and save the file.

### Step 5

Open \<mission\>/core/housin/fn_copBreakDoor.sqf and depending on your ArmA Life RPG version insert after

`[2,"STR_House_Raid_NOTF",true,[(_house getVariable "house_owner") select 1]] remoteExecCall ["life_fnc_broadcast",RCLIENT];`

or

`[[2,"STR_House_Raid_NOTF",true,[(_house getVariable "house_owner") select 1]],"life_fnc_broadcast",true,false] spawn life_fnc_MP;`

or

`[[2,"STR_House_Raid_NOTF",true,[(_house getVariable "house_owner") select 1]],"life_fnc_broadcast",true,false] call life_fnc_MP;`

following lines

```
if (_house getVariable ["security",false]) then {
    if (!(isNil {(_house getVariable "house_owner")})) then {
        private "_owner";
        _owner = objNull;
        {
            if (((_house getVariable "house_owner") select 0) isEqualTo (getPlayerUID _x)) then {
                _owner = _x;
            };
        } forEach playableUnits;
        [_house] remoteExec ["cat_alarm_fnc_houseAlarm",_owner];
    };
};
```

and save the file.

### Step 6

Open \<mission\>/core/housin/fn_houseMenu.sqf and insert at the end of the file, BUT before the last two closing braces (`}; };`) following lines

```
if (life_pInact_curTarget getVariable ["security",false]) then {
    _Btn6 ctrlSetText format [["resetAlarm"] call cat_alarm_fnc_getText];
    if (life_pInact_curTarget getVariable ["alarm",false]) then {
        _Btn6 buttonSetAction "[life_pInact_curTarget] call cat_alarm_fnc_houseAlarmOff;";
    } else {
        _Btn6 ctrlEnable false;
    };
} else {
    _Btn6 ctrlSetText format [["buyAlarm"] call cat_alarm_fnc_getText];
    _Btn6 buttonSetAction "[life_pInact_curTarget] spawn cat_alarm_fnc_houseAlarmBuy;";
};
_Btn6 ctrlShow true;
if (!(((_curTarget getVariable "house_owner") select 0) isEqualTo (getPlayerUID player))) then {
    _Btn6 ctrlEnable false;
};
```

and save the file. Example file: fn_houseMenu.sqf

### Step 7

Open \<mission\>/core/housing/fn_sellHouse.sqf and insert after

`if(_action) then {`

following lines

```
_house setVariable ["alarm",false,true];
_house setVariable ["security",false,true];
deleteMarkerLocal format ["house_%1",(_house getVariable "house_id")];
deleteMarkerLocal format ["alarm_%1",(_house getVariable "house_id")];
```

and save the file.

### Step 8

Open \<mission\>/core/items/fn_boltcutter.sqf and depending on your ArmA Life RPG version insert after

`[0,"STR_ISTR_Bolt_AlertHouse",true,[profileName]] remoteExecCall ["life_fnc_broadcast",RCLIENT];`

or

`[[0,"STR_ISTR_Bolt_AlertHouse",true,[profileName]],"life_fnc_broadcast",true,false] spawn life_fnc_MP;`

or

`[[0,"STR_ISTR_Bolt_AlertHouse",true,[profileName]],"life_fnc_broadcast",true,false] call life_fnc_MP;`

following lines

```
if (_building getVariable ["security",false]) then {
    if (!(isNil {(_building getVariable "house_owner")})) then {
        private "_owner";
        _owner = objNull;
        {
            if (((_building getVariable "house_owner") select 0) isEqualTo (getPlayerUID _x)) then {
                _owner = _x;
            };
        } forEach playableUnits;
        [_building] remoteExec ["cat_alarm_fnc_houseAlarm",_owner];
    };
};
```

and save the file.

### Step 9

Run the following code in your database:

```
ALTER TABLE `houses` ADD `security` tinyint(1) DEFAULT '0' AFTER `owned`;
```

**That’s it!*

You have installed the cationstudio house security system successfully!

## Configuration

You can adjust settings in \<mission\>/cation/spawn/config.cpp.

Texts and translations can be edited in \<mission\>/cation/alarm/language.cpp.
