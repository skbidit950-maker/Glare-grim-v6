-- Glare Grim (Mobile) — Orbit + Void + Get Close + Bullet Redirect + Desync (FIXED FOR RIVALS)
-- Fixed bullet manipulation for Rivals: detects actual bullet parts and redirects them
-- LocalScript in StarterPlayerScripts

local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local Workspace = game:GetService("Workspace")
local Camera = workspace.CurrentCamera
local player = Players.LocalPlayer
local pg = player:WaitForChild("PlayerGui")

-- Void spots
local voids = {
    Vector3.new(50,-520,100), Vector3.new(-120,-480,-80), Vector3.new(250,-550,150),
    Vector3.new(-200,-490,200), Vector3.new(180,-600,-150), Vector3.new(-90,-450,300),
    Vector3.new(300,-510,-50), Vector3.new(-150,-580,120), Vector3.new(120,-530,-220),
    Vector3.new(-250,-495,80), Vector3.new(160,-565,-180), Vector3.new(-50,-500,-300),
    Vector3.new(350,-540,50), Vector3.new(-300,-470,-100), Vector3.new(80,-610,250),
}

local function root()
    local c = player.Character
    return c and c:FindFirstChild("HumanoidRootPart")
end

local function getVoid()
    return voids[math.random(#voids)] + Vector3.new(math.random(-120,120), math.random(-60,30), math.random(-120,120))
end

-- ============================================
-- SETTINGS
-- ============================================
local OrbitSettings = { Radius = 8, Speed = 3, Height = 0 }
local DistanceSettings = { Enabled = false, Distance = 5, HeightOffset = 0 }
local DesyncEnabled = false
local BulletRedirect = { Enabled = false }

-- ============================================
-- GET NEAREST ENEMY
-- ============================================
local function getNearestEnemy()
    local r = root()
    if not r then return nil end
    local best, bestDist = nil, math.huge
    for _, p in pairs(Players:GetPlayers()) do
        if p ~= player then
            local pr = p.Character and p.Character:FindFirstChild("HumanoidRootPart")
            if pr then
                local d = (pr.Position - r.Position).Magnitude
                if d < bestDist then bestDist, best = d, pr end
            end
        end
    end
    return best
end

-- ============================================
-- GET ENEMY HEAD
-- ============================================
local function getEnemyHead(enemyRoot)
    if not enemyRoot then return nil end
    local char = enemyRoot.Parent
    if char then
        return char:FindFirstChild("Head") or enemyRoot
    end
    return enemyRoot
end

-- ============================================
-- BULLET REDIRECT (FIXED FOR RIVALS)
-- ============================================
local function BulletRedirectLoop()
    if not BulletRedirect.Enabled then return end
    
    local target = getNearestEnemy()
    if not target then return end
    
    local targetPos = getEnemyHead(target)
    if not targetPos then return end
    
    -- Rivals specific: find bullet parts
    -- Common names for bullets in Rivals: "Projectile", "Bullet", "Pellet", "Slug", or unnamed parts with velocity
    for _, obj in pairs(Workspace:GetDescendants()) do
        if obj:IsA("BasePart") and obj.Name ~= "Handle" and obj.Name ~= "Head" then
            -- Check if it's a bullet by velocity and size
            local vel = obj.Velocity
            if vel and vel.Magnitude > 50 then
                -- Check if it's near the player (not too far)
                local myRoot = root()
                if myRoot then
                    local dist = (obj.Position - myRoot.Position).Magnitude
                    if dist < 200 then
                        -- Redirect to enemy head
                        obj.CFrame = CFrame.new(obj.Position, targetPos.Position)
                        obj.Velocity = (targetPos.Position - obj.Position).Unit * 300
                        obj.RotVelocity = Vector3.new(0,0,0)
                    end
                end
            end
        end
    end
end

-- ============================================
-- DESYNC
-- ============================================
local desyncTimer = 0
local desyncState = false

local function DesyncLoop()
    if not DesyncEnabled then return end
    local r = root()
    if not r then return end
    desyncTimer = desyncTimer + RunService.Heartbeat:Wait()
    if desyncTimer > 0.06 then
        desyncTimer = 0
        desyncState = not desyncState
    end
    if desyncState then
        local offset = Vector3.new((math.random()-0.5)*3, (math.random()-0.5)*1, (math.random()-0.5)*3)
        r.CFrame = r.CFrame + offset
        r.Velocity = Vector3.new((math.random()-0.5)*40, (math.random()-0.5)*15, (math.random()-0.5)*40)
        r.RotVelocity = Vector3.new((math.random()-0.5)*30, (math.random()-0.5)*30, (math.random()-0.5)*30)
    else
        local offset = Vector3.new((math.random()-0.5)*0.5, (math.random()-0.5)*0.3, (math.random()-0.5)*0.5)
        r.CFrame = r.CFrame + offset
        r.Velocity = Vector3.new(0, -2, 0)
        r.RotVelocity = Vector3.new(0, 0, 0)
    end
end

-- ============================================
-- CREATE GUI
-- ============================================
local function CreateGUI()
    local oldGui = pg:FindFirstChild("GlareGrim")
    if oldGui then oldGui:Destroy() end

    local gui = Instance.new("ScreenGui")
    gui.Name = "GlareGrim"
    gui.ResetOnSpawn = false
    gui.Parent = pg

    local frame = Instance.new("Frame")
    frame.Size = UDim2.new(0, 260, 0, 480)
    frame.Position = UDim2.new(0.5, -130, 0.1, 0)
    frame.BackgroundColor3 = Color3.fromRGB(15, 15, 20)
    frame.BackgroundTransparency = 0.05
    frame.Active = true
    frame.Draggable = true
    frame.Parent = gui
    Instance.new("UICorner", frame).CornerRadius = UDim.new(0, 16)

    local stroke = Instance.new("UIStroke")
    stroke.Color = Color3.fromRGB(120, 60, 220)
    stroke.Thickness = 2
    stroke.Parent = frame

    local title = Instance.new("Frame")
    title.Size = UDim2.new(1, 0, 0, 44)
    title.BackgroundColor3 = Color3.fromRGB(25, 10, 50)
    title.Parent = frame
    Instance.new("UICorner", title).CornerRadius = UDim.new(0, 16)

    local lbl = Instance.new("TextLabel")
    lbl.Size = UDim2.new(1, -50, 1, 0)
    lbl.Position = UDim2.new(0, 14, 0, 0)
    lbl.BackgroundTransparency = 1
    lbl.Text = "✦ GLARE GRIM"
    lbl.TextColor3 = Color3.fromRGB(180, 100, 255)
    lbl.TextSize = 20
    lbl.Font = Enum.Font.GothamBold
    lbl.TextXAlignment = Enum.TextXAlignment.Left
    lbl.Parent = title

    local close = Instance.new("TextButton")
    close.Size = UDim2.new(0, 32, 0, 32)
    close.Position = UDim2.new(1, -38, 0, 6)
    close.BackgroundColor3 = Color3.fromRGB(180, 40, 80)
    close.Text = "✕"
    close.TextColor3 = Color3.fromRGB(255, 255, 255)
    close.TextSize = 16
    close.Font = Enum.Font.GothamBold
    close.Parent = title
    Instance.new("UICorner", close).CornerRadius = UDim.new(0, 8)

    local container = Instance.new("ScrollingFrame")
    container.Size = UDim2.new(1, -20, 1, -50)
    container.Position = UDim2.new(0, 10, 0, 48)
    container.BackgroundTransparency = 1
    container.BorderSizePixel = 0
    container.ScrollBarThickness = 4
    container.CanvasSize = UDim2.new(0, 0, 0, 650)
    container.Parent = frame

    local layout = Instance.new("UIListLayout")
    layout.Padding = UDim.new(0, 8)
    layout.SortOrder = Enum.SortOrder.LayoutOrder
    layout.Parent = container

    -- ============================================
    -- TOGGLE
    -- ============================================
    local function makeToggle(name, getter, setter, color)
        local btn = Instance.new("TextButton")
        btn.Size = UDim2.new(1, 0, 0, 40)
        btn.BackgroundColor3 = getter() and (color or Color3.fromRGB(0, 120, 50)) or Color3.fromRGB(40, 40, 60)
        btn.Text = name .. ": " .. (getter() and "ON" or "OFF")
        btn.TextColor3 = Color3.fromRGB(255, 255, 255)
        btn.TextSize = 15
        btn.Font = Enum.Font.GothamBold
        btn.Parent = container
        Instance.new("UICorner", btn).CornerRadius = UDim.new(0, 8)
        btn.MouseButton1Click:Connect(function()
            setter(not getter())
            btn.BackgroundColor3 = getter() and (color or Color3.fromRGB(0, 120, 50)) or Color3.fromRGB(40, 40, 60)
            btn.Text = name .. ": " .. (getter() and "ON" or "OFF")
        end)
        return btn
    end

    -- ============================================
    -- SLIDER
    -- ============================================
    local function makeSlider(name, minVal, maxVal, getter, setter, format)
        local frame = Instance.new("Frame")
        frame.Size = UDim2.new(1, 0, 0, 45)
        frame.BackgroundColor3 = Color3.fromRGB(30, 25, 45)
        frame.BackgroundTransparency = 0.3
        frame.BorderSizePixel = 0
        frame.Parent = container
        Instance.new("UICorner", frame).CornerRadius = UDim.new(0, 10)

        local label = Instance.new("TextLabel")
        label.Size = UDim2.new(0.6, 0, 0.5, 0)
        label.Position = UDim2.new(0, 10, 0, 0)
        label.BackgroundTransparency = 1
        label.Text = name
        label.TextColor3 = Color3.fromRGB(220, 220, 255)
        label.TextSize = 13
        label.Font = Enum.Font.GothamBold
        label.TextXAlignment = Enum.TextXAlignment.Left
        label.Parent = frame

        local valLabel = Instance.new("TextLabel")
        valLabel.Size = UDim2.new(0.35, 0, 0.5, 0)
        valLabel.Position = UDim2.new(0.6, 0, 0, 0)
        valLabel.BackgroundTransparency = 1
        valLabel.Text = format and format(getter()) or tostring(getter())
        valLabel.TextColor3 = Color3.fromRGB(100, 255, 200)
        valLabel.TextSize = 13
        valLabel.Font = Enum.Font.GothamBold
        valLabel.TextXAlignment = Enum.TextXAlignment.Right
        valLabel.Parent = frame

        local slider = Instance.new("Frame")
        slider.Size = UDim2.new(0.9, 0, 0, 4)
        slider.Position = UDim2.new(0.05, 0, 0.7, 0)
        slider.BackgroundColor3 = Color3.fromRGB(60, 50, 80)
        slider.BackgroundTransparency = 0
        slider.BorderSizePixel = 0
        slider.Parent = frame
        Instance.new("UICorner", slider).CornerRadius = UDim.new(0, 3)

        local fill = Instance.new("Frame")
        fill.Size = UDim2.new((getter() - minVal) / (maxVal - minVal), 0, 1, 0)
        fill.BackgroundColor3 = Color3.fromRGB(120, 60, 220)
        fill.BackgroundTransparency = 0
        fill.BorderSizePixel = 0
        fill.Parent = slider
        Instance.new("UICorner", fill).CornerRadius = UDim.new(0, 3)

        local dragging = false
        slider.InputBegan:Connect(function(input)
            if input.UserInputType == Enum.UserInputType.Touch or input.UserInputType == Enum.UserInputType.MouseButton1 then
                dragging = true
                local percent = math.clamp((input.Position.X - slider.AbsolutePosition.X) / slider.AbsoluteSize.X, 0, 1)
                local value = minVal + percent * (maxVal - minVal)
                setter(value)
                valLabel.Text = format and format(value) or tostring(value)
                fill.Size = UDim2.new(percent, 0, 1, 0)
            end
        end)

        slider.InputEnded:Connect(function(input)
            if input.UserInputType == Enum.UserInputType.Touch or input.UserInputType == Enum.UserInputType.MouseButton1 then
                dragging = false
            end
        end)

        slider.InputChanged:Connect(function(input)
            if dragging and (input.UserInputType == Enum.UserInputType.Touch or input.UserInputType == Enum.UserInputType.MouseMovement) then
                local percent = math.clamp((input.Position.X - slider.AbsolutePosition.X) / slider.AbsoluteSize.X, 0, 1)
                local value = minVal + percent * (maxVal - minVal)
                setter(value)
                valLabel.Text = format and format(value) or tostring(value)
                fill.Size = UDim2.new(percent, 0, 1, 0)
            end
        end)

        return frame
    end

    -- ============================================
    -- ORBIT
    -- ============================================
    local orbitOn = false
    local angle = 0

    local orbitConnection
    local function startOrbit()
        if orbitConnection then orbitConnection:Disconnect() end
        orbitConnection = RunService.RenderStepped:Connect(function(dt)
            if not orbitOn then return end
            local r = root()
            if not r then return end
            local target = getNearestEnemy()
            if r and target then
                angle = angle + OrbitSettings.Speed * dt
                local off = Vector3.new(math.cos(angle) * OrbitSettings.Radius, OrbitSettings.Height, math.sin(angle) * OrbitSettings.Radius)
                r.CFrame = CFrame.new(target.Position + off, target.Position)
            end
        end)
    end
    startOrbit()

    makeToggle("Orbit", function() return orbitOn end, function(v) orbitOn = v end, Color3.fromRGB(0, 200, 255))
    makeSlider("Radius", 1, 30, function() return OrbitSettings.Radius end, function(v) OrbitSettings.Radius = v end, function(v) return string.format("%.1f", v) end)
    makeSlider("Speed", 0.5, 10, function() return OrbitSettings.Speed end, function(v) OrbitSettings.Speed = v end, function(v) return string.format("%.1f", v) end)
    makeSlider("Height", -10, 10, function() return OrbitSettings.Height end, function(v) OrbitSettings.Height = v end, function(v) return string.format("%.1f", v) end)

    -- ============================================
    -- DISTANCE
    -- ============================================
    makeToggle("Get Close", function() return DistanceSettings.Enabled end, function(v) DistanceSettings.Enabled = v end, Color3.fromRGB(0, 255, 150))
    makeSlider("Dist", 1, 20, function() return DistanceSettings.Distance end, function(v) DistanceSettings.Distance = v end, function(v) return string.format("%.1f", v) end)
    makeSlider("Height Off", -5, 10, function() return DistanceSettings.HeightOffset end, function(v) DistanceSettings.HeightOffset = v end, function(v) return string.format("%.1f", v) end)

    -- ============================================
    -- BULLET REDIRECT (FIXED)
    -- ============================================
    makeToggle("Bullet Redirect", function() return BulletRedirect.Enabled end, function(v) BulletRedirect.Enabled = v end, Color3.fromRGB(255, 100, 0))

    -- ============================================
    -- DESYNC
    -- ============================================
    makeToggle("Desync", function() return DesyncEnabled end, function(v) DesyncEnabled = v end, Color3.fromRGB(200, 100, 255))

    -- ============================================
    -- VOID
    -- ============================================
    local voidOn = false
    local conn

    makeToggle("Void", function() return voidOn end, function(v)
        voidOn = v
        if conn then conn:Disconnect() conn = nil end
        if v then
            conn = RunService.Heartbeat:Connect(function()
                local r = root()
                if r then
                    r.CFrame = CFrame.new(getVoid())
                    r.Velocity = Vector3.new(0, -20, 0)
                end
            end)
        end
    end, Color3.fromRGB(150, 0, 255))

    local voidBtn = Instance.new("TextButton")
    voidBtn.Size = UDim2.new(1, 0, 0, 40)
    voidBtn.BackgroundColor3 = Color3.fromRGB(80, 30, 80)
    voidBtn.Text = "⚡ VOID TELEPORT"
    voidBtn.TextColor3 = Color3.fromRGB(255, 150, 255)
    voidBtn.TextSize = 15
    voidBtn.Font = Enum.Font.GothamBold
    voidBtn.Parent = container
    Instance.new("UICorner", voidBtn).CornerRadius = UDim.new(0, 8)
    voidBtn.MouseButton1Click:Connect(function()
        local r = root()
        if r then
            r.CFrame = CFrame.new(getVoid())
            r.Velocity = Vector3.new(0, -25, 0)
        end
    end)

    -- ============================================
    -- LOOPS
    -- ============================================
    local bulletConnection
    local function startBulletRedirect()
        if bulletConnection then bulletConnection:Disconnect() end
        bulletConnection = RunService.Heartbeat:Connect(function()
            pcall(BulletRedirectLoop)
        end)
    end
    startBulletRedirect()

    local desyncConnection
    local function startDesync()
        if desyncConnection then desyncConnection:Disconnect() end
        desyncConnection = RunService.Heartbeat:Connect(function() pcall(DesyncLoop) end)
    end
    startDesync()

    -- Distance loop
    spawn(function()
        while true do
            task.wait(0.1)
            if DistanceSettings.Enabled then
                local r = root()
                if r then
                    local target = getNearestEnemy()
                    if target then
                        local dir = (target.Position - r.Position).Unit
                        local targetPos = target.Position - dir * DistanceSettings.Distance
                        targetPos = targetPos + Vector3.new(0, DistanceSettings.HeightOffset, 0)
                        r.CFrame = CFrame.new(targetPos, target.Position)
                        r.Velocity = Vector3.new(0, 0, 0)
                    end
                end
            end
        end
    end)

    -- ============================================
    -- STATUS
    -- ============================================
    local status = Instance.new("TextLabel")
    status.Size = UDim2.new(1, 0, 0, 30)
    status.BackgroundColor3 = Color3.fromRGB(20, 10, 40)
    status.BackgroundTransparency = 0.4
    status.BorderSizePixel = 0
    status.Text = "✦ READY"
    status.TextColor3 = Color3.fromRGB(200, 200, 255)
    status.TextSize = 13
    status.Font = Enum.Font.GothamBold
    status.Parent = container
    Instance.new("UICorner", status).CornerRadius = UDim.new(0, 10)

    spawn(function()
        while true do
            task.wait(0.15)
            local anyOn = voidOn or orbitOn or DistanceSettings.Enabled or BulletRedirect.Enabled or DesyncEnabled
            if anyOn then
                status.Text = "✦ ACTIVE"
                status.TextColor3 = Color3.fromRGB(100, 255, 100)
                status.BackgroundColor3 = Color3.fromRGB(0, 50, 20)
            else
                status.Text = "✦ READY"
                status.TextColor3 = Color3.fromRGB(200, 200, 255)
                status.BackgroundColor3 = Color3.fromRGB(20, 10, 40)
            end
        end
    end)

    task.wait(0.1)
    container.CanvasSize = UDim2.new(0, 0, 0, #container:GetChildren() * 50 + 30)

    -- ============================================
    -- MINIMIZE / REOPEN
    -- ============================================
    local toggleBtn = Instance.new("TextButton")
    toggleBtn.Size = UDim2.new(0, 150, 0, 40)
    toggleBtn.Position = UDim2.new(0, 12, 0, 12)
    toggleBtn.BackgroundColor3 = Color3.fromRGB(25, 10, 50)
    toggleBtn.Text = "✦ GLARE GRIM"
    toggleBtn.TextColor3 = Color3.fromRGB(180, 100, 255)
    toggleBtn.TextSize = 15
    toggleBtn.Font = Enum.Font.GothamBold
    toggleBtn.Visible = false
    toggleBtn.Parent = gui
    Instance.new("UICorner", toggleBtn).CornerRadius = UDim.new(0, 12)

    close.MouseButton1Click:Connect(function()
        frame.Visible = false
        toggleBtn.Visible = true
    end)
    toggleBtn.MouseButton1Click:Connect(function()
        frame.Visible = true
        toggleBtn.Visible = false
    end)

    print("✦ Glare Grim loaded — Orbit + Void + Get Close + Bullet Redirect + Desync")
    print("✦ Bullet Redirect finds bullets by velocity and redirects them to enemy head")
end

-- ============================================
-- CREATE GUI
-- ============================================
pcall(CreateGUI)

player.CharacterAdded:Connect(function()
    task.wait(0.5)
    if not pg:FindFirstChild("GlareGrim") then pcall(CreateGUI) end
end)
