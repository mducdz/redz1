local Rayfield = loadstring(game:HttpGet('https://sirius.menu/rayfield'))()
local VirtualUser = game:GetService("VirtualUser")
local Players = game:GetService("Players")
local Workspace = game:GetService("Workspace")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local TeleportService = game:GetService("TeleportService")
local Lighting = game:GetService("Lighting")
local CoreGui = game:GetService("CoreGui")
local HttpService = game:GetService("HttpService")
local RunService = game:GetService("RunService")

-- Player variables
local Player = Players.LocalPlayer
local Character = Player.Character or Player.CharacterAdded:Wait()
local HumanoidRootPart = Character:WaitForChild("HumanoidRootPart")
local Humanoid = Character:WaitForChild("Humanoid")

-- Game variables
local Islands = {}
local Fruits = {}
local Bosses = {}
local NPCs = {}
local Chests = {}

-- Configuration
local Config = {
    AutoFarm = false,
    AutoFarmLevel = 1,
    AutoFarmDistance = 25,
    AutoFarmFastAttack = true,
    AutoFarmBringMob = true,
    AutoFarmSkipBoss = false,
    
    AutoBuy = false,
    AutoBuySword = false,
    AutoBuyFruit = false,
    AutoBuyAccessory = false,
    
    AutoQuest = false,
    AutoQuestType = "Pirate",
    
    AutoSkill = false,
    AutoSkillZ = true,
    AutoSkillX = true,
    AutoSkillC = true,
    AutoSkillV = true,
    AutoSkillF = true,
    
    ESP = false,
    ESPPlayers = false,
    ESPMobs = true,
    ESPBosses = true,
    ESPChests = true,
    ESPFruits = true,
    
    Teleport = false,
    TeleportIsland = "Starter Island",
    TeleportBoss = "The Gorilla King",
    TeleportFruit = "Random",
    
    Misc = {
        NoClip = false,
        InfiniteEnergy = false,
        InfiniteHealth = false,
        WalkSpeed = 16,
        JumpPower = 50,
        FlySpeed = 50,
        FlyEnabled = false
    }
}

-- Initialize Rayfield Window
local Window = Rayfield:CreateWindow({
    Name = "REDZ HUB V2 ADVANCED | Blox Fruits",
    LoadingTitle = "REDZ HUB Loading...",
    LoadingSubtitle = "by DarkForge-X",
    ConfigurationSaving = {
        Enabled = true,
        FolderName = "RedzHub",
        FileName = "Config.json"
    },
    Discord = {
        Enabled = true,
        Invite = "discord.gg/redz",
        RememberJoins = true
    },
    KeySystem = false, -- Set to true for key system
    KeySettings = {
        Title = "REDZ HUB",
        Subtitle = "Key System",
        Note = "Join discord for key",
        FileName = "RedzKey",
        SaveKey = true,
        GrabKeyFromSite = false,
        Key = {"REDZHUBV2"}
    }
})

-- Main Tab
local MainTab = Window:CreateTab("Main", 4483362458)

-- AutoFarm Section
local AutoFarmSection = MainTab:CreateSection("Auto Farm")

local AutoFarmToggle = MainTab:CreateToggle({
    Name = "Enable Auto Farm",
    CurrentValue = false,
    Flag = "AutoFarmToggle",
    Callback = function(Value)
        Config.AutoFarm = Value
        if Value then
            StartAutoFarm()
        else
            StopAutoFarm()
        end
    end,
})

local AutoFarmLevelSlider = MainTab:CreateSlider({
    Name = "Farm Level",
    Range = {1, 2550},
    Increment = 1,
    Suffix = "Level",
    CurrentValue = 1,
    Flag = "AutoFarmLevel",
    Callback = function(Value)
        Config.AutoFarmLevel = Value
    end,
})

local AutoFarmDistanceSlider = MainTab:CreateSlider({
    Name = "Farm Distance",
    Range = {10, 100},
    Increment = 5,
    Suffix = "Studs",
    CurrentValue = 25,
    Flag = "AutoFarmDistance",
    Callback = function(Value)
        Config.AutoFarmDistance = Value
    end,
})

local FastAttackToggle = MainTab:CreateToggle({
    Name = "Fast Attack",
    CurrentValue = true,
    Flag = "FastAttackToggle",
    Callback = function(Value)
        Config.AutoFarmFastAttack = Value
    end,
})

local BringMobToggle = MainTab:CreateToggle({
    Name = "Bring Mobs",
    CurrentValue = true,
    Flag = "BringMobToggle",
    Callback = function(Value)
        Config.AutoFarmBringMob = Value
    end,
})

local SkipBossToggle = MainTab:CreateToggle({
    Name = "Skip Bosses",
    CurrentValue = false,
    Flag = "SkipBossToggle",
    Callback = function(Value)
        Config.AutoFarmSkipBoss = Value
    end,
})

-- AutoBuy Section
local AutoBuySection = MainTab:CreateSection("Auto Buy")

local AutoBuyToggle = MainTab:CreateToggle({
    Name = "Enable Auto Buy",
    CurrentValue = false,
    Flag = "AutoBuyToggle",
    Callback = function(Value)
        Config.AutoBuy = Value
        if Value then
            StartAutoBuy()
        else
            StopAutoBuy()
        end
    end,
})

local AutoBuySwordToggle = MainTab:CreateToggle({
    Name = "Auto Buy Swords",
    CurrentValue = false,
    Flag = "AutoBuySwordToggle",
    Callback = function(Value)
        Config.AutoBuySword = Value
    end,
})

local AutoBuyFruitToggle = MainTab:CreateToggle({
    Name = "Auto Buy Fruits",
    CurrentValue = false,
    Flag = "AutoBuyFruitToggle",
    Callback = function(Value)
        Config.AutoBuyFruit = Value
    end,
})

local AutoBuyAccessoryToggle = MainTab:CreateToggle({
    Name = "Auto Buy Accessories",
    CurrentValue = false,
    Flag = "AutoBuyAccessoryToggle",
    Callback = function(Value)
        Config.AutoBuyAccessory = Value
    end,
})

-- AutoQuest Section
local AutoQuestSection = MainTab:CreateSection("Auto Quest")

local AutoQuestToggle = MainTab:CreateToggle({
    Name = "Enable Auto Quest",
    CurrentValue = false,
    Flag = "AutoQuestToggle",
    Callback = function(Value)
        Config.AutoQuest = Value
        if Value then
            StartAutoQuest()
        else
            StopAutoQuest()
        end
    end,
})

local QuestTypeDropdown = MainTab:CreateDropdown({
    Name = "Quest Type",
    Options = {"Pirate", "Marine", "Bounty Hunter"},
    CurrentOption = "Pirate",
    Flag = "QuestTypeDropdown",
    Callback = function(Option)
        Config.AutoQuestType = Option
    end,
})

-- Skills Tab
local SkillsTab = Window:CreateTab("Skills", 4483362458)

local AutoSkillSection = SkillsTab:CreateSection("Auto Skill")

local AutoSkillToggle = SkillsTab:CreateToggle({
    Name = "Enable Auto Skill",
    CurrentValue = false,
    Flag = "AutoSkillToggle",
    Callback = function(Value)
        Config.AutoSkill = Value
        if Value then
            StartAutoSkill()
        else
            StopAutoSkill()
        end
    end,
})

local SkillZToggle = SkillsTab:CreateToggle({
    Name = "Auto Skill Z",
    CurrentValue = true,
    Flag = "SkillZToggle",
    Callback = function(Value)
        Config.AutoSkillZ = Value
    end,
})

local SkillXToggle = SkillsTab:CreateToggle({
    Name = "Auto Skill X",
    CurrentValue = true,
    Flag = "SkillXToggle",
    Callback = function(Value)
        Config.AutoSkillX = Value
    end,
})

local SkillCToggle = SkillsTab:CreateToggle({
    Name = "Auto Skill C",
    CurrentValue = true,
    Flag = "SkillCToggle",
    Callback = function(Value)
        Config.AutoSkillC = Value
    end,
})

local SkillVToggle = SkillsTab:CreateToggle({
    Name = "Auto Skill V",
    CurrentValue = true,
    Flag = "SkillVToggle",
    Callback = function(Value)
        Config.AutoSkillV = Value
    end,
})

local SkillFToggle = SkillsTab:CreateToggle({
    Name = "Auto Skill F",
    CurrentValue = true,
    Flag = "SkillFToggle",
    Callback = function(Value)
        Config.AutoSkillF = Value
    end,
})

-- Skill Cooldown Reduction
local CooldownSection = SkillsTab:CreateSection("Cooldown Reduction")

local NoCooldownToggle = SkillsTab:CreateToggle({
    Name = "No Cooldown",
    CurrentValue = false,
    Flag = "NoCooldownToggle",
    Callback = function(Value)
        if Value then
            EnableNoCooldown()
        else
            DisableNoCooldown()
        end
    end,
})

local CooldownMultiplierSlider = SkillsTab:CreateSlider({
    Name = "Cooldown Multiplier",
    Range = {0.1, 1},
    Increment = 0.1,
    Suffix = "x",
    CurrentValue = 1,
    Flag = "CooldownMultiplier",
    Callback = function(Value)
        SetCooldownMultiplier(Value)
    end,
})

-- Teleport Tab
local TeleportTab = Window:CreateTab("Teleport", 4483362458)

-- Island Teleport
local IslandSection = TeleportTab:CreateSection("Island Teleport")

local IslandDropdown = TeleportTab:CreateDropdown({
    Name = "Select Island",
    Options = {
        "Starter Island",
        "Jungle Island",
        "Pirate Village",
        "Desert Island",
        "Snow Island",
        "Marine Fortress",
        "Sky Island",
        "Prison",
        "Colosseum",
        "Magma Village",
        "Underwater City",
        "Fountain City"
    },
    CurrentOption = "Starter Island",
    Flag = "IslandDropdown",
    Callback = function(Option)
        Config.TeleportIsland = Option
    end,
})

local TeleportIslandButton = TeleportTab:CreateButton({
    Name = "Teleport to Island",
    Callback = function()
        TeleportToIsland(Config.TeleportIsland)
    end,
})

-- Boss Teleport
local BossSection = TeleportTab:CreateSection("Boss Teleport")

local BossDropdown = TeleportTab:CreateDropdown({
    Name = "Select Boss",
    Options = {
        "The Gorilla King",
        "Bobby",
        "Yeti",
        "Vice Admiral",
        "Warden",
        "Chief Warden",
        "Swan",
        "Magma Admiral",
        "Fishman Lord",
        "Wysper",
        "Thunder God",
        "Darkbeard",
        "Dough King"
    },
    CurrentOption = "The Gorilla King",
    Flag = "BossDropdown",
    Callback = function(Option)
        Config.TeleportBoss = Option
    end,
})

local TeleportBossButton = TeleportTab:CreateButton({
    Name = "Teleport to Boss",
    Callback = function()
        TeleportToBoss(Config.TeleportBoss)
    end,
})

-- Fruit Teleport
local FruitSection = TeleportTab:CreateSection("Fruit Teleport")

local FruitDropdown = TeleportTab:CreateDropdown({
    Name = "Select Fruit",
    Options = {
        "Random",
        "Bomb",
        "Spike",
        "Chop",
        "Spring",
        "Kilo",
        "Smoke",
        "Spin",
        "Flame",
        "Falcon",
        "Ice",
        "Sand",
        "Dark",
        "Diamond",
        "Light",
        "Rubber",
        "Barrier",
        "Ghost",
        "Magma",
        "Quake",
        "Buddha",
        "Love",
        "Spider",
        "Sound",
        "Phoenix",
        "Portal",
        "Rumble",
        "Pain",
        "Blizzard",
        "Gravity",
        "Mammoth",
        "Dough",
        "Shadow",
        "Venom",
        "Control",
        "Spirit",
        "Dragon"
    },
    CurrentOption = "Random",
    Flag = "FruitDropdown",
    Callback = function(Option)
        Config.TeleportFruit = Option
    end,
})

local TeleportFruitButton = TeleportTab:CreateButton({
    Name = "Teleport to Fruit",
    Callback = function()
        TeleportToFruit(Config.TeleportFruit)
    end,
})

-- ESP Tab
local ESPTab = Window:CreateTab("ESP", 4483362458)

local ESPToggle = ESPTab:CreateToggle({
    Name = "Enable ESP",
    CurrentValue = false,
    Flag = "ESPToggle",
    Callback = function(Value)
        Config.ESP = Value
        if Value then
            EnableESP()
        else
            DisableESP()
        end
    end,
})

local PlayersESPToggle = ESPTab:CreateToggle({
    Name = "Player ESP",
    CurrentValue = false,
    Flag = "PlayersESPToggle",
    Callback = function(Value)
        Config.ESPPlayers = Value
        UpdateESP()
    end,
})

local MobsESPToggle = ESPTab:CreateToggle({
    Name = "Mob ESP",
    CurrentValue = true,
    Flag = "MobsESPToggle",
    Callback = function(Value)
        Config.ESPMobs = Value
        UpdateESP()
    end,
})

local BossesESPToggle = ESPTab:CreateToggle({
    Name = "Boss ESP",
    CurrentValue = true,
    Flag = "BossesESPToggle",
    Callback = function(Value)
        Config.ESPBosses = Value
        UpdateESP()
    end,
})

local ChestsESPToggle = ESPTab:CreateToggle({
    Name = "Chest ESP",
    CurrentValue = true,
    Flag = "ChestsESPToggle",
    Callback = function(Value)
        Config.ESPChests = Value
        UpdateESP()
    end,
})

local FruitsESPToggle = ESPTab:CreateToggle({
    Name = "Fruit ESP",
    CurrentValue = true,
    Flag = "FruitsESPToggle",
    Callback = function(Value)
        Config.ESPFruits = Value
        UpdateESP()
    end,
})

-- ESP Color Customization
local ColorSection = ESPTab:CreateSection("ESP Colors")

local PlayerColorPicker = ESPTab:CreateColorPicker({
    Name = "Player Color",
    Color = Color3.fromRGB(0, 170, 255),
    Flag = "PlayerColor",
    Callback = function(Color)
        ESPColors.Players = Color
        UpdateESP()
    end
})

local MobColorPicker = ESPTab:CreateColorPicker({
    Name = "Mob Color",
    Color = Color3.fromRGB(255, 85, 0),
    Flag = "MobColor",
    Callback = function(Color)
        ESPColors.Mobs = Color
        UpdateESP()
    end
})

local BossColorPicker = ESPTab:CreateColorPicker({
    Name = "Boss Color",
    Color = Color3.fromRGB(255, 0, 0),
    Flag = "BossColor",
    Callback = function(Color)
        ESPColors.Bosses = Color
        UpdateESP()
    end
})

-- Misc Tab
local MiscTab = Window:CreateTab("Miscellaneous", 4483362458)

-- Character Modifications
local CharacterSection = MiscTab:CreateSection("Character Modifications")

local WalkSpeedSlider = MiscTab:CreateSlider({
    Name = "Walk Speed",
    Range = {16, 500},
    Increment = 5,
    Suffix = "Studs",
    CurrentValue = 16,
    Flag = "WalkSpeedSlider",
    Callback = function(Value)
        Config.Misc.WalkSpeed = Value
        if Character and Humanoid then
            Humanoid.WalkSpeed = Value
        end
    end,
})

local JumpPowerSlider = MiscTab:CreateSlider({
    Name = "Jump Power",
    Range = {50, 500},
    Increment = 10,
    Suffix = "Studs",
    CurrentValue = 50,
    Flag = "JumpPowerSlider",
    Callback = function(Value)
        Config.Misc.JumpPower = Value
        if Character and Humanoid then
            Humanoid.JumpPower = Value
        end
    end,
})

local FlyToggle = MiscTab:CreateToggle({
    Name = "Enable Fly",
    CurrentValue = false,
    Flag = "FlyToggle",
    Callback = function(Value)
        Config.Misc.FlyEnabled = Value
        if Value then
            EnableFly()
        else
            DisableFly()
        end
    end,
})

local FlySpeedSlider = MiscTab:CreateSlider({
    Name = "Fly Speed",
    Range = {10, 200},
    Increment = 5,
    Suffix = "Studs",
    CurrentValue = 50,
    Flag = "FlySpeedSlider",
    Callback = function(Value)
        Config.Misc.FlySpeed = Value
    end,
})

local NoClipToggle = MiscTab:CreateToggle({
    Name = "

