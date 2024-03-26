local function getnamecall()
    if game.PlaceId == 2788229376 then
        return "UpdateMousePos"
    elseif game.PlaceId == 5602055394 or game.PlaceId == 7951883376 then
        return "MousePos"
    elseif game.PlaceId == 9825515356 then
        return "GetMousePos"
    end
end

local namecalltype = getnamecall()

function MainEventLocate()
    for _,v in pairs(game:GetService("ReplicatedStorage"):GetDescendants()) do
        if v.Name == "MainEvent" then
            return v
        end
    end
end

local mainevent = MainEventLocate()

-- // Shorthand
local xertox = getgenv().xertox
local xertoxMain = xertox.General
local xertoxCamMain = xertox.Camlock.Main
local xertoxCamFOV = xertox.Camlock.FOV
local xertoxSilentMain = xertox.Silent.Main
local xertoxSilentFOV = xertox.Silent.FOV
local xertoxTrace = xertox.Tracer
local xertoxAutoPred = xertox.AutoPrediction

-- // Optimization
local vect3 = Vector3.new
local vect2 = Vector2.new
local cnew = CFrame.new

-- // Libraries
local NotificationHolder = loadstring(game:HttpGet("https://raw.githubusercontent.com/BocusLuke/UI/main/STX/Module.Lua"))()
local Notification = loadstring(game:HttpGet("https://raw.githubusercontent.com/BocusLuke/UI/main/STX/Client.Lua"))()

-- // Services
local uis = game:GetService("UserInputService")
local rs = game:GetService("RunService")
local plrs = game:GetService("Players")
local ws = game:GetService("Workspace")

-- // Script Variables
local CToggle = false
local lplr = plrs.LocalPlayer
local CTarget = nil
local CPart = nil
local SToggle = false
local STarget = nil
local SPart = nil

-- // Client Variables
local m = lplr:GetMouse()
local c = ws.CurrentCamera

-- // Notification Function
local function SendNotification(text)
    Notification:Notify(
        {Title = "xertox", Description = "  "..text},
        {OutlineColor = Color3.fromRGB(255,255,255),Time = 2, Type = "image"},
        {Image = "http://www.roblox.com/asset/?id=6023426923", ImageColor = Color3.fromRGB(255,255,255)}
    )
end 

-- // Call notification function
if xertoxMain.Notifications then
    SendNotification("System - injecting xertox private")
    wait(3.5)
    SendNotification("System - finished injecting xertox private")
end

-- // Camlock FOV
local CamlockFOV = Drawing.new("Circle")
CamlockFOV.Visible = xertoxCamFOV.ShowFOV
CamlockFOV.Thickness = 1
CamlockFOV.NumSides = 30
CamlockFOV.Radius = xertoxCamFOV.Radius * 3
CamlockFOV.Color = xertoxCamFOV.Color
CamlockFOV.Filled = xertoxCamFOV.Filled
CamlockFOV.Transparency = xertoxCamFOV.Transparency

--Silent FOV
local SilentFOV = Drawing.new("Circle")
SilentFOV.Visible = xertoxSilentFOV.ShowFOV
SilentFOV.Thickness = 1
SilentFOV.NumSides = 30
SilentFOV.Radius = xertoxSilentFOV.Radius * 3
SilentFOV.Color = xertoxSilentFOV.Color
SilentFOV.Filled = xertoxSilentFOV.Filled
SilentFOV.Transparency = xertoxSilentFOV.Transparency

--Tracer
local Line = Drawing.new("Line")
Line.Color = xertoxTrace.Color
Line.Transparency = xertoxTrace.Transparency
Line.Thickness = 1
Line.Visible = xertoxTrace.Visible

-- // Script Functions
local function xertoxFindTawget() -- // Find target
    local d, t = math.huge, nil
    for _,v in pairs (plrs:GetPlayers()) do
        local _,os = c:WorldToViewportPoint(v.Character.PrimaryPart.Position)
        if v ~= lplr and v.Character and v.Character:FindFirstChild("Humanoid") and v.Character.Humanoid.Health ~= 0 and v.Character:FindFirstChild("HumanoidRootPart") and os then
            local pos = c:WorldToViewportPoint(v.Character.PrimaryPart.Position)
            local magnitude = (vect2(pos.X, pos.Y) - vect2(m.X, m.Y + 36)).magnitude
            if magnitude < d then
                t = v
                d = magnitude
            end
        end
    end
    return t
end

local function xertoxFindPart() -- // Find aimpart
    local d, p = math.huge, nil
    if CTarget then
        for _,v in pairs(CTarget.Character:GetChildren()) do
            if table.find(xertoxCamMain.Parts, v.Name) then
                local pos = c:WorldToViewportPoint(v.Position)
                local Magn = (vect2(m.X, m.Y + 36) - vect2(pos.X, pos.Y)).Magnitude
                if Magn < d then
                    d = Magn
                    p = v
                end
            end
        end
        return p.Name
    end
end

local function xertoxFindSilentPart() -- // Find aimpart
    local d, p = math.huge, nil
    if CTarget then
        for _,v in pairs(CTarget.Character:GetChildren()) do
            if table.find(xertoxSilentMain.Parts, v.Name) then
                local pos = c:WorldToViewportPoint(v.Position)
                local Magn = (vect2(m.X, m.Y + 36) - vect2(pos.X, pos.Y)).Magnitude
                if Magn < d then
                    d = Magn
                    p = v
                end
            end
        end
        return p.Name
    end
end

local function xertoxCheckAnti(targ) -- // Anti-aim detection
    if (targ.Character.HumanoidRootPart.Velocity.Y < -5 and targ.Character.Humanoid:GetState() ~= Enum.HumanoidStateType.Freefall) or targ.Character.HumanoidRootPart.Velocity.Y < -50 then
        return true
    elseif targ and (targ.Character.HumanoidRootPart.Velocity.X > 35 or targ.Character.HumanoidRootPart.Velocity.X < -35) then
        return true
    elseif targ and targ.Character.HumanoidRootPart.Velocity.Y > 60 then
        return true
    elseif targ and (targ.Character.HumanoidRootPart.Velocity.Z > 35 or targ.Character.HumanoidRootPart.Velocity.Z < -35) then
        return true
    else
        return false
    end
end

local function InSilentRadixertoxs(target, section, fov) -- // Check if player is in the fov
    if target then
        local pos = nil
        if not xertoxCheckAnti(target) then
            pos = c:WorldToViewportPoint(target.Character.PrimaryPart.Position + target.Character.PrimaryPart.Velocity * section.Prediction)
        else
            pos = c:WorldToViewportPoint(target.Character.PrimaryPart.Position + ((target.Character.Humanoid.MoveDirection * target.Character.Humanoid.WalkSpeed) * section.Prediction))
        end
        local mag = (vect2(m.X, m.Y + 36) - vect2(pos.X, pos.Y)).Magnitude
        if mag < fov * 3 then
            return true
        else
            return false
        end
    end
end

local function Silent()
    if STarget then
        if SPart and InSilentRadixertoxs(STarget, xertoxSilentMain, SilentFOV.Radius) then
            if not xertoxCheckAnti(STarget) then
                mainevent:FireServer(namecalltype, STarget.Character[SPart].Position + (STarget.Character[SPart].Velocity * xertoxSilentMain.Prediction))
            else
                mainevent:FireServer(namecalltype, STarget.Character[SPart].Position + ((STarget.Character.Humanoid.MoveDirection * STarget.Character.Humanoid.WalkSpeed) * xertoxSilentMain.Prediction))
            end
        end
    end
end


local function InRadixertoxs(target, section, fov) -- // Check if player is in the fov
    if target then
        if xertoxCamFOV.UseFOV then
            local pos = nil
            if not xertoxCheckAnti(target) then
                pos = c:WorldToViewportPoint(target.Character.PrimaryPart.Position + target.Character.PrimaryPart.Velocity * section.Prediction)
            else
                pos = c:WorldToViewportPoint(target.Character.PrimaryPart.Position + ((target.Character.Humanoid.MoveDirection * target.Character.Humanoid.WalkSpeed) * section.Prediction))
            end
            local mag = (vect2(m.X, m.Y + 36) - vect2(pos.X, pos.Y)).Magnitude
            if mag < fov * 3 then
                return true
            else
                return false
            end
        else
            return true
        end
    end
end

uis.InputBegan:Connect(function(k,t)
    if not t then
        if k.KeyCode == Enum.KeyCode[xertoxCamMain.Key:upper()] then
            CToggle = true
            CTarget = xertoxFindTawget()
            if xertoxMain.Notifications then
                SendNotification("Locked on to "..CTarget.Name)
            end
        elseif k.KeyCode == Enum.KeyCode[xertoxCamMain.UnlockKey:upper()] then
            if CToggle then
                CToggle = false
                CTarget = nil
                if xertoxMain.Notifications then
                    SendNotification("Unlocked")
                end
            end
        elseif k.KeyCode == Enum.KeyCode[xertoxSilentMain.Toggle:upper()] and xertoxSilentMain == "Regular" then
            if SToggle then
                SToggle = false
                if xertoxMain.Notifications then
                    SendNotification("Silent disabled")
                end
            else
                SToggle = true
                if xertoxMain.Notifications then
                    SendNotification("Silent enabled")
                end
            end
        end
    end
end)

rs.RenderStepped:Connect(function()
    if CTarget then
        CPart = xertoxFindPart()
        local pos = nil
        local cum = nil
        if CTarget.Character.BodyEffects["K.O"].Value == true or lplr.Character.BodyEffects["K.O"].Value == true then
            CToggle = false
            CTarget = nil
        else
            if xertoxCamMain.Shake then
                if xertoxCamMain.PredictMovement then
                    if not xertoxCheckAnti(CTarget) then
                        cum = CTarget.Character[CPart].Position + CTarget.Character[CPart].Velocity * xertoxCamMain.Prediction + (vect3(
                            math.random(-xertoxCamMain.ShakeValue, xertoxCamMain.ShakeValue),
                            math.random(-xertoxCamMain.ShakeValue, xertoxCamMain.ShakeValue),
                            math.random(-xertoxCamMain.ShakeValue, xertoxCamMain.ShakeValue)
                        ) * 0.1)
                    else
                        cum = CTarget.Character[CPart].Position + ((CTarget.Character.Humanoid.MoveDirection * CTarget.Character.Humanoid.WalkSpeed) * xertoxCamMain.Prediction + (vect3(
                            math.random(-xertoxCamMain.ShakeValue, xertoxCamMain.ShakeValue),
                            math.random(-xertoxCamMain.ShakeValue, xertoxCamMain.ShakeValue),
                            math.random(-xertoxCamMain.ShakeValue, xertoxCamMain.ShakeValue)
                        ) * 0.1))
                    end
                    pos = c:WorldToViewportPoint(cum)
                else
                    cum = CTarget.Character[CPart].Position + (vect3(
                        math.random(-xertoxCamMain.ShakeValue, xertoxCamMain.ShakeValue),
                        math.random(-xertoxCamMain.ShakeValue, xertoxCamMain.ShakeValue),
                        math.random(-xertoxCamMain.ShakeValue, xertoxCamMain.ShakeValue)
                    ) * 0.1)
                    pos = c:WorldToViewportPoint(cum)
                end
            else
                if xertoxCamMain.PredictMovement then
                    if not xertoxCheckAnti(CTarget) then
                        cum = CTarget.Character[CPart].Position + CTarget.Character[CPart].Velocity * xertoxCamMain.Prediction
                    else
                        cum = CTarget.Character[CPart].Position + ((CTarget.Character.Humanoid.MoveDirection * CTarget.Character.Humanoid.WalkSpeed) * xertoxCamMain.Prediction)
                    end
                    pos = c:WorldToViewportPoint(cum)
                else
                    cum = CTarget.Character[CPart].Position
                    pos = c:WorldToViewportPoint(cum)
                end
            end
            if InRadixertoxs(CTarget, xertoxCamMain, CamlockFOV.Radius) then
                local main = nil
                if xertoxCamMain.SmoothLock then
                    main = cnew(c.CFrame.p, cum)
                    c.CFrame = c.CFrame:Lerp(main, xertoxCamMain.Smoothness, Enum.EasingStyle.Sine, Enum.EasingDirection.InOut)
                else
                    c.CFrame = cnew(c.CFrame.p, cum)
                end
            end
            if xertoxMain.FOVMode == "Mouse" then
                if xertoxCamFOV.ShowFOV then
                    CamlockFOV.Position = vect2(m.X, m.Y + 36)
                end
                if xertoxSilentFOV.ShowFOV then
                    SilentFOV.Position = vect2(m.X, m.Y + 36)
                end
            elseif xertoxMain.FOVMode == "PredictionPoint" then
                if xertoxCamFOV.ShowFOV then
                    CamlockFOV.Position = vect2(pos.X, pos.Y)
                end
                if xertoxSilentFOV.ShowFOV then
                    SilentFOV.Position = vect2(pos.X, pos.Y)
                end
            end
            if xertoxTrace.Enabled then
                Line.Visible = true
                Line.From = vect2(m.X, m.Y + 36)
                Line.To = vect2(pos.X, pos.Y)
            end
        end
    else
        CamlockFOV.Position = vect2(m.X, m.Y + 36)
        SilentFOV.Position = vect2(m.X, m.Y + 36)
        Line.Visible = false
    end
end)

lplr.Character.ChildAdded:Connect(function(tool)
    if tool:IsA("Tool") then
        tool.Activated:connect(function()
            if xertoxSilentMain.Mode == "Regular" then
                if SToggle then
                    STarget = xertoxFindTawget()
                    if STarget then
                        SPart = xertoxFindSilentPart()
                        if SPart then
                            Silent()
                        end
                    end
                end
            elseif xertoxSilentMain.Mode == "Target" then
                if CToggle then
                    STarget = CTarget
                    if STarget then
                        SPart = xertoxFindSilentPart()
                        if SPart then
                            Silent()
                        end
                    end
                end
            end
        end)
    end
end)

lplr.CharacterAdded:Connect(function(char)
    char.ChildAdded:Connect(function(tool)
        tool.Activated:connect(function()
            if xertoxSilentMain.Mode == "Regular" then
                if SToggle then
                    STarget = xertoxFindTawget()
                    if STarget then
                        SPart = xertoxFindSilentPart()
                        if SPart then
                            Silent()
                        end
                    end
                end
            elseif xertoxSilentMain.Mode == "Target" then
                if CToggle then
                    STarget = CTarget
                    if STarget then
                        SPart = xertoxFindSilentPart()
                        if SPart then
                            Silent()
                        end
                    end
                end
            end
        end)
    end)
end)

--Auto Prediction
coroutine.resume(coroutine.create(function()
    while true do
        if xertoxAutoPred.Enabled then
            local ping = game:GetService("Stats").Network.ServerStatsItem["Data Ping"]:GetValue()
            if ping <= 40 then
                xertoxSilentMain.Prediction = xertoxAutoPred.ping30_40
            elseif ping <= 50 then
                xertoxSilentMain.Prediction = xertoxAutoPred.ping40_50
            elseif ping <= 60 then
                xertoxSilentMain.Prediction = xertoxAutoPred.ping50_60
            elseif ping <= 70 then
                xertoxSilentMain.Prediction = xertoxAutoPred.ping60_70
            elseif ping <= 80 then
                xertoxSilentMain.Prediction = xertoxAutoPred.ping70_80
            elseif ping <= 90 then
                xertoxSilentMain.Prediction = xertoxAutoPred.ping80_90
            elseif ping <= 100 then
                xertoxSilentMain.Prediction = xertoxAutoPred.ping90_100
            elseif ping <= 110 then
                xertoxSilentMain.Prediction = xertoxAutoPred.ping100_110
            elseif ping <= 120 then
                xertoxSilentMain.Prediction = xertoxAutoPred.ping110_120
            elseif ping <= 130 then
                xertoxSilentMain.Prediction = xertoxAutoPred.ping120_130
            elseif ping <= 140 then
                xertoxSilentMain.Prediction = xertoxAutoPred.ping130_140
            elseif ping <= 150 then
                xertoxSilentMain.Prediction = xertoxAutoPred.ping140_150
            elseif ping <= 160 then
                xertoxSilentMain.Prediction = xertoxAutoPred.ping150_160
            elseif ping <= 170 then
                xertoxSilentMain.Prediction = xertoxAutoPred.ping160_170
            elseif ping <= 180 then
                xertoxSilentMain.Prediction = xertoxAutoPred.ping170_180
            elseif ping <= 190 then
                xertoxSilentMain.Prediction = xertoxAutoPred.ping180_190
            elseif ping <= 200 then
                xertoxSilentMain.Prediction = xertoxAutoPred.ping190_200
            end
            task.wait(0.7)
        end
    end
end))
