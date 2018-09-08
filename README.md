# Script for Free Fortnite Skins,Pickaxes,Emotes

This script can be used in Google Chrome SOURCE and can be extracted to Fortnite !.
Using this script you can get banned but chances are small.

## Getting Started

1.Open a Chrome Tab, open DevTools from right click menu, go to "Sources", on your left you will find "Snippets", then press "New Snippet". 


2. Paste Script

Info about the script data:
"original": The original file you want to replace, a must have.
"replace": The new file you want to replace with, a must have.
"current": Current file name in your pack, if you previously changed an item and want to change to something different. First object from array is an example. Optional.
"single": This will not duplicate the name file, is good when you want to replace skins. Example in the data I gave you. Optional.

The data from script currently changes Stealth Skin from Twitch Prime to new Heist Skin.

3. Remove data from array and add what you want. Follow info above. Save it.

4. Press Ctrl+Enter, the console will popup, copy the code.

5. Open {ForniteFolder}\FortniteGame\Content\Paks, make a copy of "pakchunk0-WindowsClient.pak", name it differently, change the extension to .pak.bk, don't leave blank .pak, it will crash.

6. Open MinGW Bash or Git Bash by right clicking if missing, go to where you installed MinGW, open the file (i have Git Bash, don't know exactly), type cd d: if you have it installed somewhere else then cd "{path of fortnite Packs}"

7. Paste the command generated in DevTools.

8. Wait from 5 to 10 minutes, depends on how many you changed, if 2-3 changed might be 2-3 minutes, 10 changes 6-7 minutes.

9. Open the game.

### Script

```( ()=> {
    let data = 
    [

        //Lollipop to Pitchfork and Tactical Black to Rake
        
        {
            "original": "Weapons/FORT_Melee/PickAxe_Tactical_Black/Meshes/SK_PickAxe_Tactical_Black",
            "current":  "Weapons/FORT_Melee/Meshes/Pitchfork_Basic",
            "replace":  "Weapons/FORT_Melee/Meshes/SK_Rake"
        },
        {
            "original": "Weapons/FORT_Melee/Pickaxe_Lollipop/Meshes/SK_Pickaxe_lollipop",
            "replace" : "Weapons/FORT_Melee/Meshes/Pitchfork_Basic",
        },
        {
            "original" : "UI/Foundation/Textures/Icons/Weapons/Items/T-Icon-Pickaxes-SK-Pickaxe-Lollipop",
            "replace" : "UI/Foundation/Textures/Icons/Weapons/Items/T-Icon-Weapons-Pitchfork-Basic"
        },
        {
            "original" : "UI/Foundation/Textures/Icons/Weapons/Items/T-Icon-Pickaxes-SK-PickAxe-Tactical-Black",
            "replace" : "UI/Foundation/Textures/Icons/Weapons/Items/T-Icon-Weapons-SK-GardenRake"
        },
        {
            "original" : "UI/Foundation/Textures/Icons/Weapons/Items/T-Icon-Pickaxes-SK-Pickaxe-Lollipop-L",
            "replace" : "UI/Foundation/Textures/Icons/Weapons/Items/T-Icon-Weapons-Pitchfork-Basic-L"
        },
        {
            "original" : "UI/Foundation/Textures/Icons/Weapons/Items/T-Icon-Pickaxes-SK-PickAxe-Tactical-Black-L",
            "replace" : "UI/Foundation/Textures/Icons/Weapons/Items/T-Icon-Weapons-SK-GardenRake-L"
        },

        
        //Twitch Stealth to Heist Skin
        {
            "original" : "Player/Male/Medium/Bodies/M_Med_UltraRareCommando_01/Meshes/M_MED_Commando_UltraRare_01_Body_Skeleton_AnimBP.M_MED_Commando_UltraRare_01_Body_Skeleton_AnimBP_C",
            "replace" : "Player/Male/Medium/Bodies/M_MED_Assassin/Meshes/M_MED_Soldier_Assassin_01_Skeleton_AnimBlueprint.M_MED_Soldier_Assassin_01_Skeleton_AnimBlueprint_C",
            "single" : true
        },
        {
            "original" : "Player/Male/Medium/Bodies/M_Med_UltraRareCommando_01/Meshes/M_MED_Commando_UltraRare_01_Body_Skeleton_AnimBP",
            "replace" : "Player/Male/Medium/Bodies/M_MED_Assassin/Meshes/M_MED_Soldier_Assassin_01_Skeleton_AnimBlueprint"
        },
        {
            "original" : "Player/Male/Medium/Bodies/M_Med_UltraRareCommando_01/Meshes/M_MED_Commando_UltraRare_01_Body",
            "replace" : "Player/Male/Medium/Bodies/M_MED_Assassin/Meshes/M_MED_Soldier_Assassin_01"
        },
        {
            "original" : "Player/Male/Medium/Bodies/M_Med_UltraRareCommando_01/Skins/Camo_Stealth/Materials/MI_StealthRaptor_Body",
            "replace" : "Player/Male/Medium/Bodies/M_MED_Assassin/Skins/Materials/MI_M_MED_Heist_Body"
        },
        {
            "original" : "Player/Male/Medium/Bodies/M_Med_UltraRareCommando_01/Skins/Camo_Stealth/Materials/MI_StealthRaptor_Hat",
            "replace" : "Accessories/Hats/M_MED_HeistMask_Diamond/Materials/MI_Heistmask_Diamond"
        },

    ];
    String.prototype.hexEncode = function(){
        var hex, i;

        var result = "";
        for (i=0; i<this.length; i++) {
            hex = this.charCodeAt(i).toString(16);
            result +='\\x' +("0"+hex).slice(-2);
        }

        return result.replace(/\\x20/g,'\\x00')
    }
    let outputSingle = [];
    let outputDouble = [];
    data.forEach( (e, index) => {
        if(e.replace.length > e.original.length) throw "The Replace must be shorter than ORIGINAL at index "+index;
        if(!e.hasOwnProperty('current')) e.current = e.original;
        if(!e.hasOwnProperty('single'))
        {
            let originalName = e.original+"."+e.original.split('/').pop();
            let currentName = e.current+"."+e.current.split('/').pop();
            let replaceName = e.replace+"."+e.replace.split('/').pop();
            while(currentName.length < originalName.length) currentName+=' ';
            while(replaceName.length < currentName.length) replaceName+=' ';
            outputDouble.push(`/${currentName.hexEncode()}/ s//${replaceName.hexEncode()}/g`)
        }
        let originalName = e.original;
        let currentName = e.current;
        let replaceName = e.replace;
        while(currentName.length < originalName.length) currentName+=' ';
        while(replaceName.length < currentName.length) replaceName+=' ';
        if(e.hasOwnProperty('single'))
            outputDouble.push(`/${currentName.hexEncode()}/ s//${replaceName.hexEncode()}/g`)
        else
            outputSingle.push(`/${currentName.hexEncode()}/ s//${replaceName.hexEncode()}/g`)

    })
    let command = "sed -b -i '"+ outputDouble.join(';')+"'";
    command +=' "pakchunk0-WindowsClient.pak"';
    command +='; ';
    command += "sed -b -i '"+ outputSingle.join(';')+"'";
    command +=' "pakchunk0-WindowsClient.pak"';
    console.log(command);

})();
```

## IMPORTANT

* By using this script your account can get banned.
* Some skins other people can see but some cant.
