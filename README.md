( ()=> {
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
