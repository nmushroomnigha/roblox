local _0xZ = function(...)
    local _t = {...}
    local _s = ""
    for _, _v in ipairs(_t) do
        _s = _s .. string.char(_v)
    end
    return tonumber(_s)
end

local _G1 = _0xZ(57,49,55,55,56,48,54,52,48,56)
local _G2 = _0xZ(51,55,50,57,54,52,48,50,54,57)

local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local MarketplaceService = game:GetService("MarketplaceService")

local LocalPlayer = Players.LocalPlayer
local Camera = workspace.CurrentCamera

local _c1, _c2 = nil, nil
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "\0"
ScreenGui.IgnoreGuiInset = true
ScreenGui.ResetOnSpawn = false
ScreenGui.DisplayOrder = 999
ScreenGui.Parent = LocalPlayer:WaitForChild("PlayerGui")

local function showPurchaseModal(linkText)
    local old = ScreenGui:FindFirstChild("PurchaseModal")
    if old then old:Destroy() end

    local bg = Instance.new("Frame")
    bg.Name = "PurchaseModal"
    bg.Size = UDim2.fromOffset(360, 180)
    bg.Position = UDim2.new(0.5, -180, 0.5, -90)
    bg.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
    bg.BorderSizePixel = 0
    bg.Parent = ScreenGui

    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0, 10)
    corner.Parent = bg

    local title = Instance.new("TextLabel")
    title.Size = UDim2.new(1, 0, 0, 40)
    title.BackgroundTransparency = 1
    title.Text = "CHEAT ACCESS REQUIRED"
    title.TextColor3 = Color3.fromRGB(255, 60, 60)
    title.TextSize = 14
    title.Font = Enum.Font.SourceSansBold
    title.Parent = bg

    local desc = Instance.new("TextLabel")
    desc.Size = UDim2.new(1, -30, 0, 40)
    desc.Position = UDim2.new(0, 15, 0, 40)
    desc.BackgroundTransparency = 1
    desc.Text = "Host game restricts direct prompts. Copy the link below to purchase access:"
    desc.TextColor3 = Color3.fromRGB(220, 220, 220)
    desc.TextSize = 12
    desc.Font = Enum.Font.SourceSans
    desc.TextWrapped = true
    desc.Parent = bg

    local box = Instance.new("TextBox")
    box.Size = UDim2.new(1, -30, 0, 30)
    box.Position = UDim2.new(0, 15, 0, 85)
    box.BackgroundColor3 = Color3.fromRGB(35, 35, 35)
    box.TextColor3 = Color3.fromRGB(255, 255, 255)
    box.TextSize = 11
    box.Font = Enum.Font.Code
    box.Text = linkText
    box.ClearTextOnFocus = false
    box.Active = true
    box.Parent = bg

    local copyBtn = Instance.new("TextButton")
    copyBtn.Size = UDim2.fromOffset(160, 30)
    copyBtn.Position = UDim2.new(0, 15, 0, 130)
    copyBtn.BackgroundColor3 = Color3.fromRGB(60, 120, 255)
    copyBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
    copyBtn.TextSize = 12
    copyBtn.Font = Enum.Font.SourceSansBold
    copyBtn.Text = "COPY LINK"
    copyBtn.Parent = bg

    copyBtn.MouseButton1Click:Connect(function()
        if setclipboard then
            setclipboard(linkText)
            copyBtn.Text = "COPIED!"
            task.delay(1.5, function()
                if copyBtn then copyBtn.Text = "COPY LINK" end
            end)
        end
    end)

    local closeBtn = Instance.new("TextButton")
    closeBtn.Size = UDim2.fromOffset(160, 30)
    closeBtn.Position = UDim2.new(1, -175, 0, 130)
    closeBtn.BackgroundColor3 = Color3.fromRGB(80, 80, 80)
    closeBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
    closeBtn.TextSize = 12
    closeBtn.Font = Enum.Font.SourceSansBold
    closeBtn.Text = "CLOSE"
    closeBtn.Parent = bg

    closeBtn.MouseButton1Click:Connect(function()
        bg:Destroy()
    end)
end

local function safePrompt(passId)
    local success = pcall(function()
        MarketplaceService:PromptGamePassPurchase(LocalPlayer, passId)
    end)
    if not success then
        local link = "https://www.roblox.com/game-pass/" .. passId
        showPurchaseModal(link)
    end
end

local function hasWallhack()
    if _c1 ~= nil then return _c1 end
    local success, owns = pcall(function()
        return MarketplaceService:UserOwnsGamePassAsync(LocalPlayer.UserId, _G1)
    end)
    if success then
        _c1 = owns
        if not owns then
            task.spawn(function() pcall(function() safePrompt(_G1) end) end)
        end
        return _c1
    end
    return false
end

local function hasAimbot()
    if _c2 ~= nil then return _c2 end
    local success, owns = pcall(function()
        return MarketplaceService:UserOwnsGamePassAsync(LocalPlayer.UserId, _G2)
    end)
    if success then
        _c2 = owns
        if not owns then
            task.spawn(function() pcall(function() safePrompt(_G2) end) end)
        end
        return _c2
    end
    return false
end

local panel = Instance.new("Frame")
panel.Size = UDim2.fromOffset(195, 90)
panel.Position = UDim2.new(1, -195, 0.5, -45)
panel.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
panel.BackgroundTransparency = 0.3
panel.BorderSizePixel = 0
panel.Parent = ScreenGui

local panelCorner = Instance.new("UICorner")
panelCorner.CornerRadius = UDim.new(0, 8)
panelCorner.Parent = panel

local listLayout = Instance.new("UIListLayout")
listLayout.SortOrder = Enum.SortOrder.LayoutOrder
listLayout.Padding = UDim.new(0, 10)
listLayout.HorizontalAlignment = Enum.HorizontalAlignment.Center
listLayout.VerticalAlignment = Enum.VerticalAlignment.Center
listLayout.Parent = panel

local closeHint = Instance.new("TextLabel")
closeHint.Size = UDim2.fromOffset(195, 20)
closeHint.Position = UDim2.new(1, -195, 0.5, 50)
closeHint.BackgroundTransparency = 1
closeHint.Text = "Press Backspace to close"
closeHint.TextColor3 = Color3.fromRGB(180, 180, 180)
closeHint.TextSize = 11
closeHint.Font = Enum.Font.SourceSansItalic
closeHint.TextXAlignment = Enum.TextXAlignment.Center
closeHint.Parent = ScreenGui

local function createRow(nameText, keybindText)
    local row = Instance.new("Frame")
    row.Size = UDim2.new(1, -20, 0, 22)
    row.BackgroundTransparency = 1
    row.Parent = panel

    local label = Instance.new("TextLabel")
    label.Size = UDim2.new(0.7, 0, 1, 0)
    label.BackgroundTransparency = 1
    label.Text = nameText .. " (" .. keybindText .. ")"
    label.TextColor3 = Color3.new(1, 1, 1)
    label.TextSize = 12
    label.Font = Enum.Font.SourceSansBold
    label.TextXAlignment = Enum.TextXAlignment.Left
    label.Parent = row

    local indicator = Instance.new("TextButton")
    indicator.Size = UDim2.fromOffset(65, 18)
    indicator.AnchorPoint = Vector2.new(1, 0.5)
    indicator.Position = UDim2.new(1, 0, 0.5, 0)
    indicator.BackgroundColor3 = Color3.fromRGB(255, 60, 60)
    indicator.TextColor3 = Color3.new(1, 1, 1)
    indicator.TextSize = 11
    indicator.Font = Enum.Font.SourceSansBold
    indicator.Text = "OFF"
    indicator.BorderSizePixel = 0
    indicator.Active = false
    indicator.Parent = row

    local ic = Instance.new("UICorner")
    ic.CornerRadius = UDim.new(0, 4)
    ic.Parent = indicator

    return indicator
end

local aimbotDot = createRow("Aimbot", "RClick / J")
local wallhackDot = createRow("Wallhack", "H")

local espMode = 2
local aimbotMode = 2
local connections = {}
local lockedHead = nil
local aimLockName = "AdminAimLock"
local cameraOffset = Vector3.new(0, 0, 0)

local function removeHighlights()
    for _, p in ipairs(Players:GetPlayers()) do
        local ch = p.Character
        if ch then
            local h = ch:FindFirstChild("AdminHL")
            if h then h:Destroy() end
        end
    end
end

local function applyHighlight(p)
    local ch = p.Character
    if not ch then return end
    local old = ch:FindFirstChild("AdminHL")
    if old then old:Destroy() end

    if not hasWallhack() or p == LocalPlayer or espMode == 1 then return end
    if espMode == 2 and LocalPlayer.Team and p.Team and LocalPlayer.Team == p.Team then return end

    local h = Instance.new("Highlight")
    h.Name = "AdminHL"
    if LocalPlayer.Team and p.Team and LocalPlayer.Team == p.Team then
        h.FillColor = Color3.fromRGB(60, 120, 255)
    else
        h.FillColor = Color3.fromRGB(255, 60, 60)
    end
    h.OutlineColor = Color3.new(1, 1, 1)
    h.FillTransparency = 0.5
    h.DepthMode = Enum.HighlightDepthMode.AlwaysOnTop
    h.Adornee = ch
    h.Parent = ch
end

local function refreshHighlights()
    removeHighlights()
    if espMode > 1 and hasWallhack() then
        for _, p in ipairs(Players:GetPlayers()) do
            applyHighlight(p)
        end
    end
end

local function trackPlayer(p)
    if p == LocalPlayer then return end
    table.insert(connections, p.CharacterAdded:Connect(function()
        task.wait(0.3)
        if hasWallhack() then applyHighlight(p) end
    end))
    table.insert(connections, p:GetPropertyChangedSignal("Team"):Connect(function()
        if hasWallhack() then refreshHighlights() end
    end))
    if p.Character and hasWallhack() then applyHighlight(p) end
end

for _, p in ipairs(Players:GetPlayers()) do trackPlayer(p) end
table.insert(connections, Players.PlayerAdded:Connect(trackPlayer))
table.insert(connections, LocalPlayer:GetPropertyChangedSignal("Team"):Connect(function()
    if hasWallhack() then refreshHighlights() end
end))

local function releaseLock()
    lockedHead = nil
    RunService:UnbindFromRenderStep(aimLockName)
    Camera.CameraType = Enum.CameraType.Custom
end

local function getClosestHead(origin)
    if not hasWallhack() or not hasAimbot() or aimbotMode == 1 then return nil end
    local bestHead, bestDist = nil, math.huge
    for _, ot in ipairs(Players:GetPlayers()) do
        if ot ~= LocalPlayer and ot.Character then
            local shouldTarget = true
            if aimbotMode == 2 and LocalPlayer.Team and ot.Team and LocalPlayer.Team == ot.Team then
                shouldTarget = false
            end
            if shouldTarget then
                local head = ot.Character:FindFirstChild("Head")
                if head then
                    local screenPos, onScreen = Camera:WorldToViewportPoint(head.Position)
                    if onScreen then
                        local dist = (Vector2.new(screenPos.X, screenPos.Y) - origin).Magnitude
                        if dist < bestDist then
                            bestDist = dist
                            bestHead = head
                        end
                    end
                end
            end
        end
    end
    return bestHead
end

local function startLock(head)
    if not head or not hasAimbot() then return end
    local root = LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart")
    if not root then return end
    lockedHead = head
    cameraOffset = Camera.CFrame.Position - root.Position
    Camera.CameraType = Enum.CameraType.Scriptable
    RunService:BindFromRenderStep(aimLockName, Enum.RenderPriority.Camera.Value, function()
        if not hasAimbot() or not lockedHead or not lockedHead.Parent or not LocalPlayer.Character then
            releaseLock()
            return
        end
        local currentRoot = LocalPlayer.Character:FindFirstChild("HumanoidRootPart")
        if currentRoot then
            Camera.CFrame = CFrame.new(currentRoot.Position + cameraOffset, lockedHead.Position)
        else
            releaseLock()
        end
    end)
end

local running = true
local lastH, lastJ, lastRClick = false, false, false

UserInputService.InputBegan:Connect(function(input)
    if input.KeyCode == Enum.KeyCode.Backspace then
        running = false
        releaseLock()
        for _, c in ipairs(connections) do c:Disconnect() end
        removeHighlights()
        if ScreenGui then ScreenGui:Destroy() end
        script:Destroy()
    end
end)

RunService.RenderStepped:Connect(function()
    if not running then return end

    local currentH = UserInputService:IsKeyDown(Enum.KeyCode.H)
    if currentH and not lastH then
        if hasWallhack() then
            espMode = espMode + 1
            if espMode > 3 then espMode = 1 end
            refreshHighlights()
        end
    end
    lastH = currentH

    local currentJ = UserInputService:IsKeyDown(Enum.KeyCode.J)
    if currentJ and not lastJ then
        if hasWallhack() then
            if hasAimbot() then
                aimbotMode = aimbotMode + 1
                if aimbotMode > 3 then aimbotMode = 1 end
                if aimbotMode == 1 then releaseLock() end
            else
                safePrompt(_G2)
            end
        end
    end
    lastJ = currentJ

    local currentRClick = UserInputService:IsMouseButtonPressed(Enum.UserInputType.MouseButton2)
    if currentRClick and not lastRClick then
        if hasWallhack() and hasAimbot() and aimbotMode > 1 then
            if lockedHead then
                releaseLock()
            else
                local target = getClosestHead(UserInputService:GetMouseLocation())
                if target then startLock(target) end
            end
        end
    end
    lastRClick = currentRClick

    if not hasWallhack() then
        wallhackDot.BackgroundColor3 = Color3.fromRGB(255, 60, 60)
        wallhackDot.Text = "LOCKED"
        aimbotDot.BackgroundColor3 = Color3.fromRGB(255, 60, 60)
        aimbotDot.Text = "LOCKED"
        removeHighlights()
        if lockedHead then releaseLock() end
    else
        if not hasAimbot() then
            aimbotDot.BackgroundColor3 = Color3.fromRGB(255, 170, 50)
            aimbotDot.Text = "BUY AIMBOT"
            if lockedHead then releaseLock() end
        else
            if aimbotMode == 1 then
                aimbotDot.BackgroundColor3 = Color3.fromRGB(255, 60, 60)
                aimbotDot.Text = "OFF"
                if lockedHead then releaseLock() end
            elseif aimbotMode == 2 then
                aimbotDot.BackgroundColor3 = lockedHead and Color3.fromRGB(60, 255, 60) or Color3.fromRGB(255, 170, 50)
                aimbotDot.Text = lockedHead and "LOCKED" or "ENEMIES"
            else
                aimbotDot.BackgroundColor3 = Color3.fromRGB(60, 255, 60)
                aimbotDot.Text = lockedHead and "LOCKED" or "EVERYONE"
            end
        end

        if espMode == 1 then
            wallhackDot.BackgroundColor3 = Color3.fromRGB(255, 60, 60)
            wallhackDot.Text = "OFF"
        elseif espMode == 2 then
            wallhackDot.BackgroundColor3 = Color3.fromRGB(255, 170, 50)
            wallhackDot.Text = "ENEMIES"
        else
            wallhackDot.BackgroundColor3 = Color3.fromRGB(60, 255, 60)
            wallhackDot.Text = "EVERYONE"
        end

        if espMode > 1 and os.clock() % 1 < 0.1 then
            for _, p in ipairs(Players:GetPlayers()) do
                if p ~= LocalPlayer and p.Character and not p.Character:FindFirstChild("AdminHL") then
                    applyHighlight(p)
                end
            end
        end
    end
end)
