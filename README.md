
-- Function to teleport the character to Temple of Time
V4Tab:NewButton({
    Title = "Teleport to Temple of Time",
    Description = "Teleport your character to Temple of Time.",
    Callback = function()
        TweenTemple()
    end
})

-- Function to tween to the current race door
V4Tab:NewButton({
    Title = "Tween to current race door",
    Description = "Tween to current character race door.",
    Callback=  function() 
        TweentoCurrentRaceDoor()
    end
})

-- Toggle to auto kill trial players
V4Tab:NewToggle("Kill Trial Players",{
    Title = "Auto Kill Trial Players",
    Description = "Auto Kill All Players In Trial Stage 2.",
})

-- Section for race related functions
V4Tab:AddSection("Race")

-- Toggle to auto upgrade race
V4Tab:NewToggle("Auto Upgrade Race",{
    Title = "Auto Upgrade Race",
    Description = "Auto Upgrade Your Race V1 -> V3.",
})

-- Section for local player functions
PlRTAB:AddSection("Local Player")

-- Button to remove fog in the game
PlRTAB:NewButton({
    Title = "Remove Fog",
    Callback = function()
        local lighting = game.Lighting
        lighting.FogEnd = 100000
        for _, object in pairs(lighting:GetDescendants()) do
            if object:IsA("Atmosphere") then
                object:Destroy()
            end
        end
    end
})

-- Toggle to auto activate race V4
AutoActiveRace_Toggle = PlRTAB:NewToggle("Auto Active Race",{
    Title = "Auto Active Race V4 When Full Meter",
    Description = "Automatically activates race V4 at full meter."
})

-- Toggle to mod character abilities
PlRTAB:NewToggle("Mods Character",{
    Title = "Mods Character",
    Description = "Mod special abilities on your character."
})

-- Functions to get and manage islands and NPCs
Islands = GetAllIsland()
RealIsland = {}
NPCs = GetALLNPC()

-- Task to update settings paragraph with current configuration
task.spawn(
    function()
        while task.wait() do
            local configDisplay = ""
            for key, value in pairs(Config) do
                if typeof(value) ~= "table" then
                    configDisplay = configDisplay .. tostring(key) .. " : " .. tostring(value) .. "\n"
                else
                    local subConfigDisplay = ""
                    for subKey, subValue in pairs(value) do
                        if subValue then
                            subConfigDisplay = subConfigDisplay .. tostring(subKey) .. ", "
                        end
                    end
                    configDisplay = configDisplay .. tostring(key) .. " : " .. subConfigDisplay .. "\n"
                end
            end
            
            local bossDisplay = ""
            local bossTable = GetBossTable()
            for key, boss in pairs(bossTable) do
                bossTable[key] = RemoveLevelTitle(boss)
            end
            
            for _, bossName in pairs(getBossSeaHub()) do
                if CheckBoss(bossName) then
                    bossDisplay = bossDisplay .. bossName .. ": ✅\n"
                else
                    bossDisplay = bossDisplay .. bossName .. ": ❌\n"
                end
            end
            Settings_Paragraph:Set({Title = MySea.." | " .. tostring(MMBStatus), Content = bossDisplay})
        end
    end
)

-- Load external script for fast attack functionality
loadstring(game:HttpGet("https://raw.githubusercontent.com/memaybeohub/Function-Scripts/main/FastAttackLoader.lua"))()
