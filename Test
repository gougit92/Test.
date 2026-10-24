-- Forsaken Script | Rayfield UI Edition
-- ESP + FullBright + Touch Fling + Self Fling + Godmode 3M + Speed/Jump/Fly/Noclip + PZ + TP COMBO

local Rayfield = loadstring(game:HttpGet('https://sirius.menu/rayfield'))()

local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local Stats = game:GetService("Stats")
local Workspace = game:GetService("Workspace")
local Lighting = game:GetService("Lighting")

local LocalPlayer = Players.LocalPlayer
local Camera = Workspace.CurrentCamera

local KILLER_ROSTER = {
    ["slasher"]=true,["c00lkidd"]=true,["coolkidd"]=true,["johndoe"]=true,
    ["john doe"]=true,["1x1x1x1"]=true,["noli"]=true,["nosferatu"]=true,
    ["guest 666"]=true,["guest666"]=true,["azure"]=true,["stalker"]=true,
    ["killer"]=true,["jason"]=true
}

-- ============================================================
-- RAYFIELD WINDOW
-- ============================================================
local Window = Rayfield:CreateWindow({
    Name = "Forsaken // Menu",
    Icon = 0,
    LoadingTitle = "Forsaken Interface",
    LoadingSubtitle = "by AI",
    Theme = "Default",
    ToggleUIKeybind = "K",
    DisableRayfieldPrompts = false,
    DisableBuildWarnings = false,
    ConfigurationSaving = {
        Enabled = true,
        FolderName = "Forsaken",
        FileName = "Config"
    },
    Discord = { Enabled = false },
    KeySystem = false
})

-- ============================================================
-- TABS
-- ============================================================
local MainTab = Window:CreateTab("Main", 4483362458)
local EspTab = Window:CreateTab("ESP", 4483362458)
local KillerTab = Window:CreateTab("Killer", 4483362458)
local FuncTab = Window:CreateTab("Function", 4483362458)
local FlingTab = Window:CreateTab("Fling", 4483362458)

-- ============================================================
-- SHARED STATE
-- ============================================================
local states = {
    esp_surv=false, tr_surv=false, esp_kill=false, tr_kill=false,
    esp_item=false, tr_item=false, esp_gen=false, tr_gen=false
}
local funcStates = {
    speedEnabled=false, jumpEnabled=false, flyEnabled=false,
    noclipEnabled=false, godEnabled=false,
    fullbrightEnabled=false, touchFlingEnabled=false
}
local funcValues = {speed=50, jump=100, flySpeed=80, flingPower=1500, touchFlingPower=2500, selfFlingPower=3000}
local GOD_HP = 3000000
local keySlots = {1,1,1}
local KEY_OPTIONS = {"Q","E","LKM","R"}

-- ============================================================
-- MAIN TAB — STATS
-- ============================================================
MainTab:CreateSection("Client Stats")
local StatsLabel = MainTab:CreateParagraph({
    Title = "Client Info",
    Content = "FPS: -- | Ping: -- | Executor: --"
})

-- ============================================================
-- MAIN TAB — PLAYERS (Dropdown + кнопки)
-- ============================================================
MainTab:CreateSection("Players")

local function getPlayerNames()
    local names = {}
    for _, plr in ipairs(Players:GetPlayers()) do
        if plr ~= LocalPlayer then
            table.insert(names, plr.Name)
        end
    end
    return names
end

local mainPlayerDropdown = MainTab:CreateDropdown({
    Name = "Select Player",
    Options = getPlayerNames(),
    CurrentOption = {""},
    MultipleOptions = false,
    Flag = "MainPlayerSelect",
    Callback = function(option) end
})

local function getSelectedPlayerMain()
    local opt = mainPlayerDropdown.CurrentOption
    if opt and #opt > 0 and opt[1] ~= "" then
        return Players:FindFirstChild(opt[1])
    end
    return nil
end

MainTab:CreateButton({
    Name = "Teleport To Player (TP)",
    Callback = function()
        local plr = getSelectedPlayerMain()
        if plr then
            teleportToPlayer(plr, false)
        else
            Rayfield:Notify({Title = "Error", Content = "Select a player first.", Duration = 3})
        end
    end
})

MainTab:CreateButton({
    Name = "Elliot Pizza (PZ)",
    Callback = function()
        local plr = getSelectedPlayerMain()
        if plr then
            if not isElliot() then
                Rayfield:Notify({Title = "Warning", Content = "You are not Elliot.", Duration = 3})
            end
            teleportToPlayer(plr, true)
        else
            Rayfield:Notify({Title = "Error", Content = "Select a player first.", Duration = 3})
        end
    end
})

Players.PlayerAdded:Connect(function()
    task.wait(0.5)
    mainPlayerDropdown:Refresh(getPlayerNames())
end)
Players.PlayerRemoving:Connect(function()
    task.wait(0.5)
    mainPlayerDropdown:Refresh(getPlayerNames())
end)

-- ============================================================
-- ESP TAB
-- ============================================================
EspTab:CreateSection("Player ESP")
EspTab:CreateToggle({
    Name = "ESP Survivor",
    CurrentValue = false,
    Flag = "ESP_Survivor",
    Callback = function(v) states.esp_surv = v end
})
EspTab:CreateToggle({
    Name = "Tracker Survivor",
    CurrentValue = false,
    Flag = "Tracker_Survivor",
    Callback = function(v) states.tr_surv = v end
})
EspTab:CreateToggle({
    Name = "ESP Killer",
    CurrentValue = false,
    Flag = "ESP_Killer",
    Callback = function(v) states.esp_kill = v end
})
EspTab:CreateToggle({
    Name = "Tracker Killer",
    CurrentValue = false,
    Flag = "Tracker_Killer",
    Callback = function(v) states.tr_kill = v end
})

EspTab:CreateSection("Object ESP")
EspTab:CreateToggle({
    Name = "ESP Items",
    CurrentValue = false,
    Flag = "ESP_Items",
    Callback = function(v) states.esp_item = v end
})
EspTab:CreateToggle({
    Name = "Tracker Items",
    CurrentValue = false,
    Flag = "Tracker_Items",
    Callback = function(v) states.tr_item = v end
})
EspTab:CreateToggle({
    Name = "ESP Generator",
    CurrentValue = false,
    Flag = "ESP_Generator",
    Callback = function(v) states.esp_gen = v end
})
EspTab:CreateToggle({
    Name = "Tracker Generator",
    CurrentValue = false,
    Flag = "Tracker_Generator",
    Callback = function(v) states.tr_gen = v end
})

-- ============================================================
-- KILLER TAB
-- ============================================================
KillerTab:CreateSection("Killer Combo")

local killerDropdown = KillerTab:CreateDropdown({
    Name = "Select Target",
    Options = getPlayerNames(),
    CurrentOption = {""},
    MultipleOptions = false,
    Flag = "KillerTarget",
    Callback = function(option) end
})

local function getSelectedPlayerKiller()
    local opt = killerDropdown.CurrentOption
    if opt and #opt > 0 and opt[1] ~= "" then
        return Players:FindFirstChild(opt[1])
    end
    return nil
end

KillerTab:CreateButton({
    Name = "Execute TP Combo",
    Callback = function()
        local plr = getSelectedPlayerKiller()
        if plr then
            killerComboTeleport(plr)
        else
            Rayfield:Notify({Title = "Error", Content = "Select a target first.", Duration = 3})
        end
    end
})

KillerTab:CreateSection("Combo Key Slots")

local slotLabels = {"Slot 1", "Slot 2", "Slot 3"}
local slotDropdowns = {}

for i = 1, 3 do
    slotDropdowns[i] = KillerTab:CreateDropdown({
        Name = slotLabels[i],
        Options = KEY_OPTIONS,
        CurrentOption = {KEY_OPTIONS[keySlots[i]]},
        MultipleOptions = false,
        Flag = "KillerSlot"..i,
        Callback = function(option)
            for idx, val in ipairs(KEY_OPTIONS) do
                if val == option[1] then
                    keySlots[i] = idx
                    break
                end
            end
        end
    })
end

Players.PlayerAdded:Connect(function()
    task.wait(0.5)
    killerDropdown:Refresh(getPlayerNames())
end)
Players.PlayerRemoving:Connect(function()
    task.wait(0.5)
    killerDropdown:Refresh(getPlayerNames())
end)

-- ============================================================
-- FUNCTION TAB
-- ============================================================
FuncTab:CreateSection("Movement")

FuncTab:CreateToggle({
    Name = "Speed Hack (CFrame)",
    CurrentValue = false,
    Flag = "SpeedHack",
    Callback = function(v) funcStates.speedEnabled = v end
})
FuncTab:CreateSlider({
    Name = "Speed",
    Range = {0, 400},
    Increment = 1,
    Suffix = "studs/s",
    CurrentValue = 50,
    Flag = "SpeedValue",
    Callback = function(v) funcValues.speed = v end
})

FuncTab:CreateToggle({
    Name = "Jump Power (CFrame)",
    CurrentValue = false,
    Flag = "JumpPower",
    Callback = function(v) funcStates.jumpEnabled = v end
})
FuncTab:CreateSlider({
    Name = "Jump",
    Range = {0, 400},
    Increment = 1,
    Suffix = "power",
    CurrentValue = 100,
    Flag = "JumpValue",
    Callback = function(v) funcValues.jump = v end
})

FuncTab:CreateToggle({
    Name = "Fly (CFrame)",
    CurrentValue = false,
    Flag = "FlyEnabled",
    Callback = function(v) funcStates.flyEnabled = v end
})
FuncTab:CreateSlider({
    Name = "Fly Speed",
    Range = {0, 500},
    Increment = 1,
    Suffix = "studs/s",
    CurrentValue = 80,
    Flag = "FlySpeedValue",
    Callback = function(v) funcValues.flySpeed = v end
})

FuncTab:CreateSection("Protection")

FuncTab:CreateToggle({
    Name = "Noclip (Bypass)",
    CurrentValue = false,
    Flag = "Noclip",
    Callback = function(v) funcStates.noclipEnabled = v end
})
FuncTab:CreateToggle({
    Name = "Godmode (3M HP)",
    CurrentValue = false,
    Flag = "Godmode",
    Callback = function(v) funcStates.godEnabled = v end
})
FuncTab:CreateToggle({
    Name = "Fullbright",
    CurrentValue = false,
    Flag = "Fullbright",
    Callback = function(v) funcStates.fullbrightEnabled = v end
})

FuncTab:CreateSection("Touch Fling")

FuncTab:CreateToggle({
    Name = "Touch Fling (Touch = Launch)",
    CurrentValue = false,
    Flag = "TouchFling",
    Callback = function(v)
        funcStates.touchFlingEnabled = v
        refreshTouchFling()
    end
})
FuncTab:CreateSlider({
    Name = "Touch Fling Power",
    Range = {500, 8000},
    Increment = 100,
    Suffix = "power",
    CurrentValue = 2500,
    Flag = "TouchFlingPower",
    Callback = function(v) funcValues.touchFlingPower = v end
})

-- ============================================================
-- FLING TAB
-- ============================================================
FlingTab:CreateSection("Self Fling")

FlingTab:CreateButton({
    Name = "FLING YOURSELF",
    Callback = function()
        selfFling(funcValues.selfFlingPower)
    end
})
FlingTab:CreateSlider({
    Name = "Self Fling Power",
    Range = {500, 10000},
    Increment = 100,
    Suffix = "power",
    CurrentValue = 3000,
    Flag = "SelfFlingPower",
    Callback = function(v) funcValues.selfFlingPower = v end
})

FlingTab:CreateSection("Fling Other Players")

local flingDropdown = FlingTab:CreateDropdown({
    Name = "Select Player",
    Options = getPlayerNames(),
    CurrentOption = {""},
    MultipleOptions = false,
    Flag = "FlingTarget",
    Callback = function(option) end
})

local function getSelectedPlayerFling()
    local opt = flingDropdown.CurrentOption
    if opt and #opt > 0 and opt[1] ~= "" then
        return Players:FindFirstChild(opt[1])
    end
    return nil
end

FlingTab:CreateButton({
    Name = "FLING SELECTED PLAYER",
    Callback = function()
        local plr = getSelectedPlayerFling()
        if plr then
            flingPlayer(plr, funcValues.flingPower)
        else
            Rayfield:Notify({Title = "Error", Content = "Select a player first.", Duration = 3})
        end
    end
})
FlingTab:CreateSlider({
    Name = "Fling Power",
    Range = {500, 5000},
    Increment = 100,
    Suffix = "power",
    CurrentValue = 1500,
    Flag = "FlingPower",
    Callback = function(v) funcValues.flingPower = v end
})

Players.PlayerAdded:Connect(function()
    task.wait(0.5)
    flingDropdown:Refresh(getPlayerNames())
end)
Players.PlayerRemoving:Connect(function()
    task.wait(0.5)
    flingDropdown:Refresh(getPlayerNames())
end)

-- ============================================================
-- CORE LOGIC (без изменений)
-- ============================================================

local function isElliot()
    local char = LocalPlayer.Character
    if not char then return false end
    local n1 = string.lower(char.Name)
    local n2 = string.lower(LocalPlayer.Name)
    local n3 = string.lower(LocalPlayer.DisplayName or "")
    return string.find(n1,"elliot") or string.find(n2,"elliot") or string.find(n3,"elliot")
end

local function teleportToPlayer(targetPlr, throwPizza)
    local char = LocalPlayer.Character; if not char then return end
    local hrp = char:FindFirstChild("HumanoidRootPart"); if not hrp then return end
    local targetChar = targetPlr and targetPlr.Character; if not targetChar then return end
    local targetHrp = targetChar:FindFirstChild("HumanoidRootPart"); if not targetHrp then return end

    local originalCF = hrp.CFrame
    local function getBehindCFrame()
        local tPos = targetHrp.Position
        local tLook = targetHrp.CFrame.LookVector
        return CFrame.new(tPos - tLook * 4, tPos)
    end

    if not throwPizza then
        pcall(function() hrp.CFrame = targetHrp.CFrame end)
        task.wait(0.15)
        if char.Parent and hrp.Parent then pcall(function() hrp.CFrame = originalCF end) end
        return
    end

    pcall(function() hrp.CFrame = getBehindCFrame() end)
    local tracking = true
    task.spawn(function()
        while tracking do
            if char.Parent and hrp.Parent and targetChar.Parent and targetHrp.Parent then
                pcall(function() hrp.CFrame = getBehindCFrame() end)
            else break end
            RunService.RenderStepped:Wait()
        end
    end)

    task.wait(0.2)
    local pizzaDetected = false
    local detectedPizza = nil
    local detectConn
    detectConn = Workspace.DescendantAdded:Connect(function(d)
        if pizzaDetected then return end
        if d:IsA("BasePart") and string.find(string.lower(d.Name), "pizza") then
            pizzaDetected = true; detectedPizza = d
        end
    end)

    pressKeyByName("Q")
    local waited = 0
    while not pizzaDetected and waited < 1.5 do
        task.wait(0.02); waited = waited + 0.02
    end
    task.wait(0.25)
    if detectConn then detectConn:Disconnect() end

    tracking = false
    if char.Parent and hrp.Parent then pcall(function() hrp.CFrame = originalCF end) end

    task.spawn(function()
        local pizza = detectedPizza
        if not pizza then
            task.wait(0.1)
            for _, d in pairs(Workspace:GetDescendants()) do
                if d:IsA("BasePart") and string.find(string.lower(d.Name), "pizza") and not d:IsDescendantOf(char) then
                    pizza = d; break
                end
            end
        end
        if not pizza then return end
        for _ = 1, 40 do
            if not pizza or not pizza.Parent then break end
            if not targetHrp or not targetHrp.Parent then break end
            pcall(function()
                local tp = targetHrp.Position
                local dir = tp - pizza.Position
                if dir.Magnitude > 0.3 then
                    pizza.CFrame = CFrame.new(tp - dir.Unit * 1.5, tp)
                    pizza.AssemblyLinearVelocity = dir.Unit * 300
                else
                    pizza.CFrame = CFrame.new(tp)
                    pizza.AssemblyLinearVelocity = Vector3.new(0,0,0)
                end
            end)
            RunService.RenderStepped:Wait()
        end
    end)
end

local function killerComboTeleport(targetPlr)
    local char = LocalPlayer.Character; if not char then return end
    local hrp = char:FindFirstChild("HumanoidRootPart"); if not hrp then return end
    local originalCF = hrp.CFrame
    for i = 1, 3 do
        local targetChar = targetPlr and targetPlr.Character
        local targetHrp = targetChar and targetChar:FindFirstChild("HumanoidRootPart")
        if not targetHrp then break end
        pcall(function() hrp.CFrame = targetHrp.CFrame end)
        task.wait(0.12)
        pressKeyByName(KEY_OPTIONS[keySlots[i]])
        task.wait(0.2)
    end
    if char.Parent and hrp.Parent then pcall(function() hrp.CFrame = originalCF end) end
end

local function flingPlayer(targetPlr, power)
    if not targetPlr or targetPlr == LocalPlayer then return end
    local targetChar = targetPlr.Character; if not targetChar then return end
    local targetHrp = targetChar:FindFirstChild("HumanoidRootPart"); if not targetHrp then return end

    for _, part in pairs(targetChar:GetDescendants()) do
        if part:IsA("BasePart") then
            pcall(function() part:SetNetworkOwner(LocalPlayer) end)
            pcall(function() sethiddenproperty(part, "NetworkIsSleeping", false) end)
        end
    end

    local myChar = LocalPlayer.Character
    local myHrp = myChar and myChar:FindFirstChild("HumanoidRootPart")
    local dir
    if myHrp then
        dir = (targetHrp.Position - myHrp.Position)
        if dir.Magnitude < 0.5 then dir = Vector3.new(0, 1, 0) end
        dir = dir.Unit
    else
        dir = Vector3.new(0, 1, 0)
    end

    power = power or funcValues.flingPower

    pcall(function()
        targetHrp.AssemblyLinearVelocity = dir * power + Vector3.new(0, power * 0.6, 0)
        targetHrp.AssemblyAngularVelocity = Vector3.new(power, power, power)
    end)

    for _, part in pairs(targetChar:GetDescendants()) do
        if part:IsA("BasePart") and part ~= targetHrp then
            pcall(function()
                part.AssemblyLinearVelocity = dir * power
                part.AssemblyAngularVelocity = Vector3.new(power, power, power)
            end)
        end
    end
end

local function selfFling(power)
    local char = LocalPlayer.Character; if not char then return end
    local hrp = char:FindFirstChild("HumanoidRootPart"); if not hrp then return end

    power = power or funcValues.selfFlingPower

    local dir = Vector3.new(
        math.random(-100, 100) / 100,
        1,
        math.random(-100, 100) / 100
    ).Unit

    for _, part in pairs(char:GetDescendants()) do
        if part:IsA("BasePart") then
            pcall(function() part:SetNetworkOwner(LocalPlayer) end)
        end
    end

    pcall(function()
        hrp.AssemblyLinearVelocity = dir * power + Vector3.new(0, power * 0.8, 0)
        hrp.AssemblyAngularVelocity = Vector3.new(power * 2, power * 2, power * 2)
    end)

    for _, part in pairs(char:GetDescendants()) do
        if part:IsA("BasePart") and part ~= hrp then
            pcall(function()
                part.AssemblyLinearVelocity = dir * power
                part.AssemblyAngularVelocity = Vector3.new(power * 2, power * 2, power * 2)
            end)
        end
    end

    local hum = char:FindFirstChildOfClass("Humanoid")
    if hum then
        pcall(function() hum.Sit = false end)
    end

    for _, part in pairs(char:GetDescendants()) do
        if part:IsA("BasePart") then
            for _, weld in pairs(part:GetChildren()) do
                if weld:IsA("Weld") or weld:IsA("WeldConstraint") or weld:IsA("Motor6D") then
                    local other = weld.Part0 == part and weld.Part1 or weld.Part0
                    if other and not other:IsDescendantOf(char) then
                        pcall(function() weld:Destroy() end)
                    end
                end
            end
        end
    end
end

local touchFlingConn = nil
local lastTouchFling = 0

local function refreshTouchFling()
    if touchFlingConn then touchFlingConn:Disconnect(); touchFlingConn = nil end
    if not funcStates.touchFlingEnabled then return end

    local char = LocalPlayer.Character; if not char then return end
    local hrp = char:FindFirstChild("HumanoidRootPart"); if not hrp then return end

    to
