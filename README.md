
getgenv().BananaPremium = {
    Name = "BANANA PREMIUM HUB",
    Version = "v7.9.5",
    Logo = "https://i.imgur.com/bananalogo.png",
    Author = "Banana Team",
    GameId = 2753915549
}

-- LOADER MODULE
local function LoadScript()
    local StarterGui = game:GetService("StarterGui")
    local Players = game:GetService("Players")
    local LocalPlayer = Players.LocalPlayer
    local PlayerGui = LocalPlayer:WaitForChild("PlayerGui")
    
    -- CREATE MAIN SCREEN GUI
    local MainGUI = Instance.new("ScreenGui")
    MainGUI.Name = "BananaPremiumHub"
    MainGUI.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
    MainGUI.ResetOnSpawn = false
    MainGUI.Parent = PlayerGui
    
    -- BACKGROUND FRAME
    local MainFrame = Instance.new("Frame")
    MainFrame.Name = "MainFrame"
    MainFrame.Size = UDim2.new(0, 600, 0, 450)
    MainFrame.Position = UDim2.new(0.5, -300, 0.5, -225)
    MainFrame.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
    MainFrame.BorderSizePixel = 0
    MainFrame.Parent = MainGUI
    
    -- TOP BAR
    local TopBar = Instance.new("Frame")
    TopBar.Name = "TopBar"
    TopBar.Size = UDim2.new(1, 0, 0, 40)
    TopBar.BackgroundColor3 = Color3.fromRGB(255, 184, 28)
    TopBar.BorderSizePixel = 0
    TopBar.Parent = MainFrame
    
    -- LOGO
    local Logo = Instance.new("ImageLabel")
    Logo.Name = "Logo"
    Logo.Size = UDim2.new(0, 35, 0, 35)
    Logo.Position = UDim2.new(0, 10, 0.5, -17.5)
    Logo.BackgroundTransparency = 1
    Logo.Image = "rbxassetid://1234567890"
    Logo.Parent = TopBar
    
    -- TITLE
    local Title = Instance.new("TextLabel")
    Title.Name = "Title"
    Title.Size = UDim2.new(0, 200, 0, 40)
    Title.Position = UDim2.new(0, 50, 0, 0)
    Title.BackgroundTransparency = 1
    Title.Text = "BANANA PREMIUM HUB"
    Title.TextColor3 = Color3.fromRGB(255, 255, 255)
    Title.TextSize = 18
    Title.Font = Enum.Font.GothamBold
    Title.TextXAlignment = Enum.TextXAlignment.Left
    Title.Parent = TopBar
    
    -- CLOSE BUTTON
    local CloseButton = Instance.new("TextButton")
    CloseButton.Name = "CloseButton"
    CloseButton.Size = UDim2.new(0, 40, 0, 40)
    CloseButton.Position = UDim2.new(1, -40, 0, 0)
    CloseButton.BackgroundColor3 = Color3.fromRGB(220, 0, 0)
    CloseButton.Text = "X"
    CloseButton.TextColor3 = Color3.fromRGB(255, 255, 255)
    CloseButton.TextSize = 18
    CloseButton.Font = Enum.Font.GothamBold
    CloseButton.Parent = TopBar
    
    -- SIDEBAR
    local SideBar = Instance.new("Frame")
    SideBar.Name = "SideBar"
    SideBar.Size = UDim2.new(0, 150, 1, -40)
    SideBar.Position = UDim2.new(0, 0, 0, 40)
    SideBar.BackgroundColor3 = Color3.fromRGB(35, 35, 35)
    SideBar.BorderSizePixel = 0
    SideBar.Parent = MainFrame
    
    -- MAIN CONTENT
    local ContentFrame = Instance.new("Frame")
    ContentFrame.Name = "ContentFrame"
    ContentFrame.Size = UDim2.new(1, -150, 1, -40)
    ContentFrame.Position = UDim2.new(0, 150, 0, 40)
    ContentFrame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
    ContentFrame.BorderSizePixel = 0
    ContentFrame.Parent = MainFrame
    
    -- SIDEBAR BUTTONS
    local TabButtons = {
        "Auto Farm",
        "Teleports",
        "Combat",
        "Player",
        "Misc",
        "Settings"
    }
    
    local CurrentTab = "Auto Farm"
    
    for i, tabName in pairs(TabButtons) do
        local TabButton = Instance.new("TextButton")
        TabButton.Name = tabName .. "Tab"
        TabButton.Size = UDim2.new(1, 0, 0, 40)
        TabButton.Position = UDim2.new(0, 0, 0, (i-1)*40)
        TabButton.BackgroundColor3 = Color3.fromRGB(45, 45, 45)
        TabButton.BorderSizePixel = 0
        TabButton.Text = tabName
        TabButton.TextColor3 = Color3.fromRGB(255, 255, 255)
        TabButton.TextSize = 14
        TabButton.Font = Enum.Font.Gotham
        TabButton.Parent = SideBar
        
        TabButton.MouseButton1Click:Connect(function()
            CurrentTab = tabName
            UpdateContent()
        end)
    end
    
    -- CONTENT UPDATE FUNCTION
    function UpdateContent()
        for _, child in pairs(ContentFrame:GetChildren()) do
            child:Destroy()
        end
        
        if CurrentTab == "Auto Farm" then
            -- AUTO FARM TAB
            local ScrollFrame = Instance.new("ScrollingFrame")
            ScrollFrame.Size = UDim2.new(1, 0, 1, 0)
            ScrollFrame.BackgroundTransparency = 1
            ScrollFrame.ScrollBarThickness = 5
            ScrollFrame.CanvasSize = UDim2.new(0, 0, 0, 800)
            ScrollFrame.Parent = ContentFrame
            
            -- Enable Auto Farm
            local EnableFrame = CreateToggle("Enable Auto Farm", false, function(state)
                getgenv().AutoFarm = state
            end)
            EnableFrame.Parent = ScrollFrame
            
            -- Farm Level
            local FarmLevel = CreateToggle("Farm Level", false, function(state)
                getgenv().FarmLevel = state
            end)
            FarmLevel.Position = UDim2.new(0, 0, 0, 50)
            FarmLevel.Parent = ScrollFrame
            
            -- Farm Fruits
            local FarmFruits = CreateToggle("Farm Fruits", false, function(state)
                getgenv().FarmFruits = state
            end)
            FarmFruits.Position = UDim2.new(0, 0, 0, 100)
            FarmFruits.Parent = ScrollFrame
            
            -- Farm Sea Beasts
            local FarmSeaBeasts = CreateToggle("Farm Sea Beasts", false, function(state)
                getgenv().FarmSeaBeasts = state
            end)
            FarmSeaBeasts.Position = UDim2.new(0, 0, 0, 150)
            FarmSeaBeasts.Parent = ScrollFrame
            
            -- Auto New World
            local AutoNewWorld = CreateToggle("Auto New World", false, function(state)
                getgenv().AutoNewWorld = state
            end)
            AutoNewWorld.Position = UDim2.new(0, 0, 0, 200)
            AutoNewWorld.Parent = ScrollFrame
            
            -- Auto Third Sea
            local AutoThirdSea = CreateToggle("Auto Third Sea", false, function(state)
                getgenv().AutoThirdSea = state
            end)
            AutoThirdSea.Position = UDim2.new(0, 0, 0, 250)
            AutoThirdSea.Parent = ScrollFrame
            
            -- Select Mob
            local MobLabel = Instance.new("TextLabel")
            MobLabel.Size = UDim2.new(0, 200, 0, 30)
            MobLabel.Position = UDim2.new(0, 0, 0, 300)
            MobLabel.BackgroundTransparency = 1
            MobLabel.Text = "Select Mob:"
            MobLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
            MobLabel.TextSize = 14
            MobLabel.TextXAlignment = Enum.TextXAlignment.Left
            MobLabel.Parent = ScrollFrame
            
            local MobDropdown = Instance.new("TextButton")
            MobDropdown.Size = UDim2.new(0, 200, 0, 30)
            MobDropdown.Position = UDim2.new(0, 0, 0, 330)
            MobDropdown.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
            MobDropdown.Text = "Select Mob"
            MobDropdown.TextColor3 = Color3.fromRGB(255, 255, 255)
            MobDropdown.TextSize = 14
            MobDropdown.Parent = ScrollFrame
            
            -- Fast Attack
            local FastAttack = CreateToggle("Fast Attack", true, function(state)
                getgenv().FastAttack = state
            end)
            FastAttack.Position = UDim2.new(0, 0, 0, 380)
            FastAttack.Parent = ScrollFrame
            
            -- Bring Mob
            local BringMob = CreateToggle("Bring Mob", true, function(state)
                getgenv().BringMob = state
            end)
            BringMob.Position = UDim2.new(0, 0, 0, 430)
            BringMob.Parent = ScrollFrame
            
            -- Auto Haki
            local AutoHaki = CreateToggle("Auto Haki", true, function(state)
                getgenv().AutoHaki = state
            end)
            AutoHaki.Position = UDim2.new(0, 0, 0, 480)
            AutoHaki.Parent = ScrollFrame
            
        elseif CurrentTab == "Teleports" then
            -- TELEPORTS TAB
            local ScrollFrame = Instance.new("ScrollingFrame")
            ScrollFrame.Size = UDim2.new(1, 0, 1, 0)
            ScrollFrame.BackgroundTransparency = 1
            ScrollFrame.ScrollBarThickness = 5
            ScrollFrame.CanvasSize = UDim2.new(0, 0, 0, 1000)
            ScrollFrame.Parent = ContentFrame
            
            local Islands = {
                "Start Island",
                "Marine Starter Island",
                "Middle Town",
                "Jungle",
                "Pirate Village",
                "Desert",
                "Frozen Village",
                "Colosseum",
                "Prison",
                "Magma Village",
                "Underwater City",
                "Fountain City",
                "Sky Island 1",
                "Sky Island 2",
                "Sky Island 3"
            }
            
            for i, island in pairs(Islands) do
                local TeleportButton = Instance.new("TextButton")
                TeleportButton.Size = UDim2.new(0, 200, 0, 40)
                TeleportButton.Position = UDim2.new(0, 0, 0, (i-1)*45)
                TeleportButton.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
                TeleportButton.Text = island
                TeleportButton.TextColor3 = Color3.fromRGB(255, 255, 255)
                TeleportButton.TextSize = 14
                TeleportButton.Parent = ScrollFrame
                
                TeleportButton.MouseButton1Click:Connect(function()
                    TeleportToIsland(island)
                end)
            end
            
        elseif CurrentTab == "Combat" then
            -- COMBAT TAB
            local ScrollFrame = Instance.new("ScrollingFrame")
            ScrollFrame.Size = UDim2.new(1, 0, 1, 0)
            ScrollFrame.BackgroundTransparency = 1
            ScrollFrame.ScrollBarThickness = 5
            ScrollFrame.CanvasSize = UDim2.new(0, 0, 0, 600)
            ScrollFrame.Parent = ContentFrame
            
            -- Kill Aura
            local KillAura = CreateToggle("Kill Aura", false, function(state)
                getgenv().KillAura = state
            end)
            KillAura.Parent = ScrollFrame
            
            -- AimBot
            local AimBot = CreateToggle("AimBot", false, function(state)
                getgenv().AimBot = state
            end)
            AimBot.Position = UDim2.new(0, 0, 0, 50)
            AimBot.Parent = ScrollFrame
            
            -- No Cooldown
            local NoCooldown = CreateToggle("No Cooldown", false, function(state)
                getgenv().NoCooldown = state
            end)
            NoCooldown.Position = UDim2.new(0, 0, 0, 100)
            NoCooldown.Parent = ScrollFrame
            
            -- Infinit Energy
            local InfiniteEnergy = CreateToggle("Infinite Energy", false, function(state)
                getgenv().InfiniteEnergy = state
            end)
            InfiniteEnergy.Position = UDim2.new(0, 0, 0, 150)
            InfiniteEnergy.Parent = ScrollFrame
            
            -- God Mode
            local GodMode = CreateToggle("God Mode", false, function(state)
                getgenv().GodMode = state
            end)
            GodMode.Position = UDim2.new(0, 0, 0, 200)
            GodMode.Parent = ScrollFrame
            
            -- Fly Hack
            local FlyHack = CreateToggle("Fly Hack", false, function(state)
                getgenv().FlyHack = state
            end)
            FlyHack.Position = UDim2.new(0, 0, 0, 250)
            FlyHack.Parent = ScrollFrame
            
            -- Speed Hack
            local SpeedLabel = Instance.new("TextLabel")
            SpeedLabel.Size = UDim2.new(0, 200, 0, 30)
            SpeedLabel.Position = UDim2.new(0, 0, 0, 300)
            SpeedLabel.BackgroundTransparency = 1
            SpeedLabel.Text = "Walk Speed:"
            SpeedLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
            SpeedLabel.TextSize = 14
            SpeedLabel.TextXAlignment = Enum.TextXAlignment.Left
            SpeedLabel.Parent = ScrollFrame
            
            local SpeedSlider = Instance.new("TextBox")
            SpeedSlider.Size = UDim2.new(0, 200, 0, 30)
            SpeedSlider.Position = UDim2.new(0, 0, 0, 330)
            SpeedSlider.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
            SpeedSlider.Text = "16"
            SpeedSlider.TextColor3 = Color3.fromRGB(255, 255, 255)
            SpeedSlider.TextSize = 14
            SpeedSlider.Parent = ScrollFrame
            
            -- Jump Power
            local JumpLabel = Instance.new("TextLabel")
            JumpLabel.Size = UDim2.new(0, 200, 0, 30)
            JumpLabel.Position = UDim2.new(0, 0, 0, 370)
            JumpLabel.BackgroundTransparency = 1
            JumpLabel.Text = "Jump Power:"
            JumpLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
            JumpLabel.TextSize = 14
            JumpLabel.TextXAlignment = Enum.TextXAlignment.Left
            JumpLabel.Parent = ScrollFrame
            
            local JumpSlider = Instance.new("TextBox")
            JumpSlider.Size = UDim2.new(0, 200, 0, 30)
            JumpSlider.Position = UDim2.new(0, 0, 0, 400)
            JumpSlider.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
            JumpSlider.Text = "50"
            JumpSlider.TextColor3 = Color3.fromRGB(255, 255, 255)
            JumpSlider.TextSize = 14
            JumpSlider.Parent = ScrollFrame
            
        elseif CurrentTab == "Player" then
            -- PLAYER TAB
            local ScrollFrame = Instance.new("ScrollingFrame")
            ScrollFrame.Size = UDim2.new(1, 0, 1, 0)
            ScrollFrame.BackgroundTransparency = 1
            ScrollFrame.ScrollBarThickness = 5
            Scroll
