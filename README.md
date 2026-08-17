-- ============================================
-- DADDY DEVIL SPAMMER – DELTA OPTIMIZED
-- (No close button, safe messages)
-- ============================================

local TCS = game:GetService("TextChatService")
local RS = game:GetService("ReplicatedStorage")
local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local TweenService = game:GetService("TweenService")
local UserInputService = game:GetService("UserInputService")

-- Configuration
local config = {
    target = "TMX",
    symbol = "@",
    count = 150,          -- <-- CHANGED to 150 (was 160)
    delay = 0.8,
    enabled = false
}

-- UPDATED MESSAGES WITH EMOJIS (your list)
local messages = {
    "👿 TMX MARE DADDY DEVIL 👿",
    "👑 TMX MARE SUMIT 👑",
    "🦅 TMX MARE RYAN 🦅",
    "🌶️ TMX MEH JHAL MURI 🌶️",
    "👑 TMX MARE ROY 👑",
    "🤝 TMX MARE JOD 🤝",
    "⚡ TMX MARE HUM ON TOP ⚡",
    "🌶️ TMX MEH LAL MIRCHI 🌶️",
    "🗿 TMX MARE TERE WAZID 🗿",
    "☕ TMX MEH MASALA CHAI ☕"
}

local currentMsg = 1

-- Safe send function
local function send(message)
    local success, err = pcall(function()
        if TCS.ChatVersion == Enum.ChatVersion.TextChatService then
            local channels = TCS:FindFirstChild("TextChannels")
            local general = channels and channels:FindFirstChild("RBXGeneral")
            if general then
                general:SendAsync(message)
            else
                warn("No RBXGeneral channel found")
            end
        else
            RS.DefaultChatSystemChatEvents.SayMessageRequest:FireServer(message, "All")
        end
    end)
    if not success then
        warn("Send error:", err)
    end
end

-- Get a valid GUI parent (Delta-friendly)
local function getGui()
    local success, gui = pcall(function()
        return game:GetService("CoreGui")
    end)
    if success and gui then return gui end
    success, gui = pcall(function()
        return LocalPlayer:WaitForChild("PlayerGui")
    end)
    if success and gui then return gui end
    error("No GUI parent available!")
end

task.wait(0.1)  -- crucial for Delta

local success, err = pcall(function()
    local GP = getGui()
    if GP:FindFirstChild("DADDY_DEVIL_SPAMMER") then
        GP.DADDY_DEVIL_SPAMMER:Destroy()
    end

    -- ====== ADD MUSIC PLAYER – AUTO‑PLAY, NO CONTROLS ======
    local sound = Instance.new("Sound")
    sound.SoundId = "rbxassetid://140415804746906"
    sound.Volume = 1          -- fixed, no volume control
    sound.Looped = false      -- play once
    sound.Parent = GP         -- attached to GUI parent (CoreGui/PlayerGui)
    sound:Play()
    -- =======================================================

    local G = Instance.new("ScreenGui")
    G.Name = "DADDY_DEVIL_SPAMMER"
    G.ResetOnSpawn = false
    G.IgnoreGuiInset = true
    G.Parent = GP

    -- ====== UI BUILDING (your original design) ======
    local width = 480
    local height = 360
    local M = Instance.new("Frame", G)
    M.Size = UDim2.new(0, width, 0, height)
    M.Position = UDim2.new(0.5, -width/2, 0.25, 0)
    M.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
    M.BorderSizePixel = 0
    M.Active = true
    M.ClipsDescendants = true

    local blackBg = Instance.new("Frame", M)
    blackBg.Size = UDim2.new(1, 0, 1, 0)
    blackBg.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
    blackBg.BorderSizePixel = 0

    local corner = Instance.new("UICorner", M)
    corner.CornerRadius = UDim.new(0, 15)

    local glowBorder = Instance.new("Frame", M)
    glowBorder.Size = UDim2.new(1, 6, 1, 6)
    glowBorder.Position = UDim2.new(0, -3, 0, -3)
    glowBorder.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
    glowBorder.BackgroundTransparency = 0.2
    glowBorder.BorderSizePixel = 0
    local glowCorner = Instance.new("UICorner", glowBorder)
    glowCorner.CornerRadius = UDim.new(0, 18)

    local innerGlow = Instance.new("Frame", M)
    innerGlow.Size = UDim2.new(1, -20, 1, -20)
    innerGlow.Position = UDim2.new(0, 10, 0, 10)
    innerGlow.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
    innerGlow.BackgroundTransparency = 0.8
    innerGlow.BorderSizePixel = 0
    local innerCorner = Instance.new("UICorner", innerGlow)
    innerCorner.CornerRadius = UDim.new(0, 12)

    local dragging, dragInput, dragStart, startPos
    M.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
            dragging = true
            dragStart = input.Position
            startPos = M.Position
            input.Changed:Connect(function()
                if input.UserInputState == Enum.UserInputState.End then
                    dragging = false
                end
            end)
        end
    end)

    M.InputChanged:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch then
            dragInput = input
        end
    end)

    UserInputService.InputChanged:Connect(function(input)
        if input == dragInput and dragging then
            local delta = input.Position - dragStart
            M.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
        end
    end)

    local titleBg = Instance.new("Frame", M)
    titleBg.Size = UDim2.new(1, 0, 0, 55)
    titleBg.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
    titleBg.BorderSizePixel = 0

    local T = Instance.new("TextLabel", titleBg)
    T.Size = UDim2.new(1, 0, 1, 0)
    T.BackgroundTransparency = 1
    T.Text = "DADDY DEVIL SPAMMER 😈⚡"
    T.TextColor3 = Color3.fromRGB(255, 255, 255)
    T.TextSize = 18
    T.Font = Enum.Font.GothamBold
    T.TextXAlignment = Enum.TextXAlignment.Center

    local titleGlow = Instance.new("Frame", titleBg)
    titleGlow.Size = UDim2.new(0.6, 0, 0, 2)
    titleGlow.Position = UDim2.new(0.2, 0, 1, -5)
    titleGlow.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
    titleGlow.BorderSizePixel = 0

    local line1 = Instance.new("Frame", M)
    line1.Size = UDim2.new(0.9, 0, 0, 1)
    line1.Position = UDim2.new(0.05, 0, 0, 60)
    line1.BackgroundColor3 = Color3.fromRGB(10, 10, 10)
    line1.BorderSizePixel = 0

    local function createField(labelText, defaultValue, xPos, yPos, callback, isNumber)
        local container = Instance.new("Frame", M)
        container.Size = UDim2.new(0.45, 0, 0, 50)
        container.Position = UDim2.new(xPos, 0, 0, yPos)
        container.BackgroundTransparency = 1

        local label = Instance.new("TextLabel", container)
        label.Size = UDim2.new(1, 0, 0.35, 0)
        label.Position = UDim2.new(0, 0, 0, 0)
        label.BackgroundTransparency = 1
        label.Text = labelText
        label.TextColor3 = Color3.fromRGB(200, 200, 200)
        label.TextSize = 12
        label.Font = Enum.Font.GothamMedium
        label.TextXAlignment = Enum.TextXAlignment.Left
        label.TextYAlignment = Enum.TextYAlignment.Bottom

        local boxBg = Instance.new("Frame", container)
        boxBg.Size = UDim2.new(1, 0, 0.5, 0)
        boxBg.Position = UDim2.new(0, 0, 0.5, 0)
        boxBg.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
        boxBg.BorderSizePixel = 0
        local boxCorner = Instance.new("UICorner", boxBg)
        boxCorner.CornerRadius = UDim.new(0, 6)

        local inputBorder = Instance.new("Frame", boxBg)
        inputBorder.Size = UDim2.new(1, 0, 1, 0)
        inputBorder.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
        inputBorder.BorderSizePixel = 0
        local borderCorner = Instance.new("UICorner", inputBorder)
        borderCorner.CornerRadius = UDim.new(0, 6)

        local box = Instance.new("TextBox", boxBg)
        box.Size = UDim2.new(1, -14, 1, 0)
        box.Position = UDim2.new(0, 7, 0, 0)
        box.BackgroundTransparency = 1
        box.Text = tostring(defaultValue)
        box.TextColor3 = Color3.fromRGB(255, 255, 255)
        box.TextSize = 13
        box.Font = Enum.Font.Gotham
        box.TextXAlignment = Enum.TextXAlignment.Left
        box.ClearTextOnFocus = false

        box.Focused:Connect(function()
            TweenService:Create(inputBorder, TweenInfo.new(0.2), { BackgroundColor3 = Color3.fromRGB(50, 50, 50) }):Play()
            TweenService:Create(boxBg, TweenInfo.new(0.2), { BackgroundColor3 = Color3.fromRGB(0, 0, 0) }):Play()
        end)

        box.FocusLost:Connect(function(enterPressed)
            TweenService:Create(inputBorder, TweenInfo.new(0.2), { BackgroundColor3 = Color3.fromRGB(20, 20, 20) }):Play()
            TweenService:Create(boxBg, TweenInfo.new(0.2), { BackgroundColor3 = Color3.fromRGB(0, 0, 0) }):Play()
            local value = box.Text
            if isNumber then
                value = tonumber(value) or defaultValue
                box.Text = tostring(value)
            end
            callback(value)
        end)

        return box
    end

    createField("🎯 TARGET", config.target, 0.05, 68, function(v) config.target = v end)
    createField("✏️ SYMBOL", config.symbol, 0.51, 68, function(v) config.symbol = v end)
    createField("🔢 COUNT", config.count, 0.05, 125, function(v) config.count = tonumber(v) or 150 end, true)   -- now defaults to 150
    createField("⏱️ SPEED", config.delay, 0.51, 125, function(v) config.delay = tonumber(v) or 0.8 end, true)

    local line2 = Instance.new("Frame", M)
    line2.Size = UDim2.new(0.9, 0, 0, 1)
    line2.Position = UDim2.new(0.05, 0, 0, 190)
    line2.BackgroundColor3 = Color3.fromRGB(10, 10, 10)
    line2.BorderSizePixel = 0

    local startBtn = Instance.new("TextButton", M)
    startBtn.Size = UDim2.new(0.85, 0, 0, 45)
    startBtn.Position = UDim2.new(0.075, 0, 0, 205)
    startBtn.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
    startBtn.Text = "▶ START"
    startBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
    startBtn.TextSize = 18
    startBtn.Font = Enum.Font.GothamBold
    local btnCorner = Instance.new("UICorner", startBtn)
    btnCorner.CornerRadius = UDim.new(0, 10)

    local btnBorder = Instance.new("Frame", startBtn)
    btnBorder.Size = UDim2.new(1, 0, 1, 0)
    btnBorder.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
    btnBorder.BackgroundTransparency = 0.5
    btnBorder.ZIndex = -1
    local btnBorderCorner = Instance.new("UICorner", btnBorder)
    btnBorderCorner.CornerRadius = UDim.new(0, 10)

    local btnGlow = Instance.new("Frame", startBtn)
    btnGlow.Size = UDim2.new(1, 10, 1, 10)
    btnGlow.Position = UDim2.new(0, -5, 0, -5)
    btnGlow.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
    btnGlow.BackgroundTransparency = 0.7
    btnGlow.BorderSizePixel = 0
    local glowCorner2 = Instance.new("UICorner", btnGlow)
    glowCorner2.CornerRadius = UDim.new(0, 12)

    local statusLabel = Instance.new("TextLabel", M)
    statusLabel.Size = UDim2.new(0.9, 0, 0, 22)
    statusLabel.Position = UDim2.new(0.05, 0, 1, -47)
    statusLabel.BackgroundTransparency = 1
    statusLabel.Text = "● READY"
    statusLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
    statusLabel.TextSize = 11
    statusLabel.Font = Enum.Font.Gotham
    statusLabel.TextXAlignment = Enum.TextXAlignment.Center

    local msgCounter = Instance.new("TextLabel", M)
    msgCounter.Size = UDim2.new(0.9, 0, 0, 18)
    msgCounter.Position = UDim2.new(0.05, 0, 1, -27)
    msgCounter.BackgroundTransparency = 1
    msgCounter.Text = "📨 " .. #messages .. " messages loaded"
    msgCounter.TextColor3 = Color3.fromRGB(150, 150, 150)
    msgCounter.TextSize = 10
    msgCounter.Font = Enum.Font.Gotham
    msgCounter.TextXAlignment = Enum.TextXAlignment.Center

    startBtn.MouseButton1Click:Connect(function()
        config.enabled = not config.enabled
        startBtn.Text = config.enabled and "■ STOP" or "▶ START"
        statusLabel.Text = config.enabled and "⚡ SPAMMING..." or "● READY"
        TweenService:Create(startBtn, TweenInfo.new(0.1), { Size = UDim2.new(0.85, 0, 0, 42) }):Play()
        task.wait(0.1)
        TweenService:Create(startBtn, TweenInfo.new(0.1), { Size = UDim2.new(0.85, 0, 0, 45) }):Play()
    end)

    -- MINIMIZE BUTTON (only, no close)
    local minBtn = Instance.new("TextButton", M)
    minBtn.Size = UDim2.new(0, 30, 0, 30)
    minBtn.Position = UDim2.new(1, -40, 0, 8)
    minBtn.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
    minBtn.Text = "−"
    minBtn.TextColor3 = Color3.fromRGB(200, 200, 200)
    minBtn.TextSize = 16
    minBtn.Font = Enum.Font.GothamBold
    local minCorner = Instance.new("UICorner", minBtn)
    minCorner.CornerRadius = UDim.new(0, 6)
    minBtn.BorderSizePixel = 1
    minBtn.BorderColor3 = Color3.fromRGB(30, 30, 30)

    minBtn.MouseEnter:Connect(function()
        TweenService:Create(minBtn, TweenInfo.new(0.2), { BorderColor3 = Color3.fromRGB(60, 60, 60) }):Play()
    end)
    minBtn.MouseLeave:Connect(function()
        TweenService:Create(minBtn, TweenInfo.new(0.2), { BorderColor3 = Color3.fromRGB(30, 30, 30) }):Play()
    end)

    -- Floating icon
    local floatBtn = Instance.new("TextButton", G)
    floatBtn.Size = UDim2.new(0, 55, 0, 55)
    floatBtn.Position = UDim2.new(0.93, 0, 0.85, 0)
    floatBtn.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
    floatBtn.Text = "😈"
    floatBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
    floatBtn.TextSize = 28
    floatBtn.Visible = false
    floatBtn.ZIndex = 10
    local floatCorner = Instance.new("UICorner", floatBtn)
    floatCorner.CornerRadius = UDim.new(1, 0)
    floatBtn.BorderSizePixel = 2
    floatBtn.BorderColor3 = Color3.fromRGB(30, 30, 30)

    local floatGlow = Instance.new("Frame", floatBtn)
    floatGlow.Size = UDim2.new(1, 10, 1, 10)
    floatGlow.Position = UDim2.new(0, -5, 0, -5)
    floatGlow.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
    floatGlow.BackgroundTransparency = 0.6
    floatGlow.BorderSizePixel = 0
    local floatGlowCorner = Instance.new("UICorner", floatGlow)
    floatGlowCorner.CornerRadius = UDim.new(1, 0)

    local pulseFloat = TweenService:Create(floatGlow, TweenInfo.new(1.5, Enum.EasingStyle.Sine, Enum.EasingDirection.InOut, -1, true), {
        BackgroundTransparency = 0.3
    })
    pulseFloat:Play()

    minBtn.MouseButton1Click:Connect(function()
        M.Visible = not M.Visible
        floatBtn.Visible = not M.Visible
        if floatBtn.Visible then
            floatBtn.Size = UDim2.new(0, 0, 0, 0)
            TweenService:Create(floatBtn, TweenInfo.new(0.3, Enum.EasingStyle.Back), {
                Size = UDim2.new(0, 55, 0, 55)
            }):Play()
        end
    end)

    floatBtn.MouseButton1Click:Connect(function()
        M.Visible = true
        floatBtn.Visible = false
        M.Size = UDim2.new(0, 0, 0, 0)
        TweenService:Create(M, TweenInfo.new(0.4, Enum.EasingStyle.Back), {
            Size = UDim2.new(0, width, 0, height)
        }):Play()
    end)

    -- Spam loop
    local spamCoroutine = coroutine.wrap(function()
        while true do
            if config.enabled and #messages > 0 then
                local msg = messages[currentMsg]
                if msg and msg ~= "" then
                    local repeatCount = math.min(config.count, 200)
                    local prefix = string.rep(config.symbol, repeatCount)
                    local targetText = "[" .. config.target .. "]"
                    local fullMsg = prefix .. " " .. targetText .. " " .. msg
                    send(fullMsg)
                    local shortMsg = msg:sub(1, 20) .. (string.len(msg) > 20 and "..." or "")
                    statusLabel.Text = "⚡ " .. shortMsg
                    statusLabel.TextColor3 = Color3.fromRGB(200, 200, 200)
                    T.TextColor3 = Color3.fromRGB(100, 100, 100)
                    task.wait(0.05)
                    T.TextColor3 = Color3.fromRGB(255, 255, 255)
                    currentMsg = (currentMsg % #messages) + 1
                end
            end
            task.wait(config.delay)
        end
    end)
    spamCoroutine()

    -- GUI entrance animation
    M.Size = UDim2.new(0, 0, 0, 0)
    M.Position = UDim2.new(0.5, 0, 0.25, 0)
    TweenService:Create(M, TweenInfo.new(0.6, Enum.EasingStyle.Back), {
        Size = UDim2.new(0, width, 0, height),
        Position = UDim2.new(0.5, -width/2, 0.25, 0)
    }):Play()

    print("✅ DADDY DEVIL SPAMMER loaded successfully!")

    -- ====== SEND INITIAL LOAD MESSAGE ======
    -- If you want the load message to also use 150 symbols, change the 140 below to 150.
    task.wait(0.5)
    pcall(function()
        send(string.rep("@", 140) .. " DADDY DEVIL SPAMMER LOADED ⚡😈")
    end)
end)

if not success then
    warn("❌ Error loading spammer:", err)
end
