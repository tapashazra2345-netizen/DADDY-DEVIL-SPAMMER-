-- ============================================
-- DADDY DEVIL SPAMMER – DELTA OPTIMIZED
-- + ENHANCED LOADING SCREEN (Devil Red Neon)
-- + MULTI TARGET (10 SLOTS, 5x2 GRID)
-- + RX CHAT (auto-execute)
-- + MESSAGE EDITOR
-- Default symbol: ~
-- ============================================

local TCS = game:GetService("TextChatService")
local RS = game:GetService("ReplicatedStorage")
local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local TweenService = game:GetService("TweenService")
local UserInputService = game:GetService("UserInputService")
local HttpService = game:GetService("HttpService")
local SoundService = game:GetService("SoundService")

local MESSAGE_FILE = "daddy_devil_messages.txt"

local config = {
    target = "TMX",
    symbol = "~",
    count = 150,
    delay = 0.8,
    enabled = false
}

local defaultMessages = {
    "👿 TMX MARE DADDY DEVIL 👿",
    "👑 TMX MARE SUMIT 👑",
    "🦅 TMX MARE RYAN 🦅",
    "🌶️ TMX MEH JHAL MURI 🌶️",
    "😹 TMX MEH GOTE 😹",
    "🤝 TMX MARE JOD 🤝",
    "⚡ TMX MARE HUM ON TOP ⚡",
    "🌶️ TMX MEH LAL MIRCHI 🌶️",
    "☕ TMX MEH MASALA CHAI ☕"
}

local function loadMessages()
    local ok, data = pcall(readfile, MESSAGE_FILE)
    if ok and data and data ~= "" then
        local dOk, decoded = pcall(function() return HttpService:JSONDecode(data) end)
        if dOk and type(decoded) == "table" and #decoded > 0 then return decoded end
    end
    return table.clone(defaultMessages)
end

local function saveMessages(msgs)
    local ok, enc = pcall(function() return HttpService:JSONEncode(msgs) end)
    if ok and enc then pcall(writefile, MESSAGE_FILE, enc) end
end

local messages = loadMessages()
local currentMsg = 1

-- Multi target state
local TOTAL_SLOTS = 10
local multiTargets = {}
local multiMode = false

local function send(message)
    pcall(function()
        if TCS.ChatVersion == Enum.ChatVersion.TextChatService then
            local ch = TCS:FindFirstChild("TextChannels")
            local gen = ch and ch:FindFirstChild("RBXGeneral")
            if gen then gen:SendAsync(message) end
        else
            RS.DefaultChatSystemChatEvents.SayMessageRequest:FireServer(message, "All")
        end
    end)
end

local function getGui()
    local ok, gui = pcall(function() return game:GetService("CoreGui") end)
    if ok and gui then return gui end
    ok, gui = pcall(function() return LocalPlayer:WaitForChild("PlayerGui") end)
    if ok and gui then return gui end
    error("No GUI parent available!")
end

task.wait(0.1)

local success, err = pcall(function()
    local GP = getGui()
    if GP:FindFirstChild("DADDY_DEVIL_SPAMMER") then
        GP.DADDY_DEVIL_SPAMMER:Destroy()
    end

    local G = Instance.new("ScreenGui")
    G.Name = "DADDY_DEVIL_SPAMMER"
    G.ResetOnSpawn = false
    G.IgnoreGuiInset = true
    G.Parent = GP

    -- ===== ENHANCED LOADING SCREEN – DEVIL RED NEON =====
    local loading = Instance.new("Frame", G)
    loading.Size = UDim2.fromScale(1, 1)
    loading.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
    loading.ZIndex = 100

    -- Ambient red glow behind title
    local glowBg = Instance.new("Frame", loading)
    glowBg.Size = UDim2.new(0.7, 0, 0, 200)
    glowBg.Position = UDim2.new(0.15, 0, 0.32, 0)
    glowBg.BackgroundColor3 = Color3.fromRGB(60, 0, 0)
    glowBg.BackgroundTransparency = 0.7
    glowBg.BorderSizePixel = 0
    glowBg.ZIndex = 100
    Instance.new("UICorner", glowBg).CornerRadius = UDim.new(0, 30)

    local pulseGlow = TweenService:Create(glowBg,
        TweenInfo.new(1.2, Enum.EasingStyle.Sine, Enum.EasingDirection.InOut, -1, true),
        { BackgroundTransparency = 0.4 })
    pulseGlow:Play()

    -- Title
    local loadingTitle = Instance.new("TextLabel", loading)
    loadingTitle.Size = UDim2.new(0.85, 0, 0, 80)
    loadingTitle.Position = UDim2.new(0.075, 0, 0.34, 0)
    loadingTitle.BackgroundTransparency = 1
    loadingTitle.Text = "DADDY DEVIL SPAMMER 😈⚡"
    loadingTitle.TextColor3 = Color3.fromRGB(255, 60, 60)
    loadingTitle.TextScaled = true
    loadingTitle.Font = Enum.Font.GothamBold
    loadingTitle.ZIndex = 101

    -- Title glow pulse
    local titlePulse = TweenService:Create(loadingTitle,
        TweenInfo.new(0.8, Enum.EasingStyle.Sine, Enum.EasingDirection.InOut, -1, true),
        { TextColor3 = Color3.fromRGB(255, 200, 200) })
    titlePulse:Play()

    -- Subtitle
    local loadingSub = Instance.new("TextLabel", loading)
    loadingSub.Size = UDim2.new(0.85, 0, 0, 25)
    loadingSub.Position = UDim2.new(0.075, 0, 0.44, 0)
    loadingSub.BackgroundTransparency = 1
    loadingSub.Text = "😈 INITIALIZING DEVIL MODE 😈"
    loadingSub.TextColor3 = Color3.fromRGB(200, 100, 100)
    loadingSub.TextSize = 14
    loadingSub.Font = Enum.Font.Gotham
    loadingSub.ZIndex = 101

    -- Status text
    local loadingStatus = Instance.new("TextLabel", loading)
    loadingStatus.Size = UDim2.new(0.8, 0, 0, 30)
    loadingStatus.Position = UDim2.new(0.1, 0, 0.52, 0)
    loadingStatus.BackgroundTransparency = 1
    loadingStatus.Text = "⚡ LOADING 0%"
    loadingStatus.TextColor3 = Color3.fromRGB(255, 255, 255)
    loadingStatus.TextSize = 20
    loadingStatus.Font = Enum.Font.GothamBold
    loadingStatus.ZIndex = 101

    -- Progress bar container
    local progressBack = Instance.new("Frame", loading)
    progressBack.Size = UDim2.new(0.6, 0, 0, 14)
    progressBack.Position = UDim2.new(0.2, 0, 0.62, 0)
    progressBack.BackgroundColor3 = Color3.fromRGB(30, 0, 0)
    progressBack.ZIndex = 101
    progressBack.BorderSizePixel = 1
    progressBack.BorderColor3 = Color3.fromRGB(255, 50, 50)
    Instance.new("UICorner", progressBack).CornerRadius = UDim.new(1, 0)

    -- Progress fill
    local progress = Instance.new("Frame", progressBack)
    progress.Size = UDim2.new(0, 0, 1, 0)
    progress.BackgroundColor3 = Color3.fromRGB(255, 50, 50)
    progress.ZIndex = 102
    progress.BorderSizePixel = 0
    Instance.new("UICorner", progress).CornerRadius = UDim.new(1, 0)

    -- Progress glow
    local progressGlow = Instance.new("Frame", progress)
    progressGlow.Size = UDim2.new(1, 8, 1, 8)
    progressGlow.Position = UDim2.new(0, -4, 0, -4)
    progressGlow.BackgroundColor3 = Color3.fromRGB(255, 100, 100)
    progressGlow.BackgroundTransparency = 0.6
    progressGlow.BorderSizePixel = 0
    progressGlow.ZIndex = 101
    Instance.new("UICorner", progressGlow).CornerRadius = UDim.new(1, 0)

    -- Footer
    local tipText = Instance.new("TextLabel", loading)
    tipText.Size = UDim2.new(0.8, 0, 0, 20)
    tipText.Position = UDim2.new(0.1, 0, 0.67, 0)
    tipText.BackgroundTransparency = 1
    tipText.Text = "🔥 POWERED BY SUMIT 🔥"
    tipText.TextColor3 = Color3.fromRGB(255, 50, 50)
    tipText.TextSize = 12
    tipText.Font = Enum.Font.GothamBold
    tipText.ZIndex = 101

    -- Rotating loading tips
    local loadingTips = {
        "⚡ LOADING DEVIL ARSENAL...",
        "😈 SUMMONING DARK POWERS...",
        "🔥 PREPARING SPAM ENGINE...",
        "👿 CHARGING UP DADDY MODE...",
        "💀 ALMOST READY..."
    }

    for i = 1, 100 do
        progress.Size = UDim2.new(i / 100, 0, 1, 0)
        loadingStatus.Text = "⚡ LOADING " .. i .. "%"
        local tipIndex = math.min(math.floor((i - 1) / 20) + 1, #loadingTips)
        loadingSub.Text = loadingTips[tipIndex]
        task.wait(0.03)
    end

    -- Fade out loading screen
    local fadeOut = TweenService:Create(loading,
        TweenInfo.new(0.4, Enum.EasingStyle.Quad, Enum.EasingDirection.In),
        { BackgroundTransparency = 1 })
    fadeOut:Play()
    task.wait(0.4)
    loading:Destroy()

    -- ===== MUSIC =====
    local sound = Instance.new("Sound")
    sound.SoundId = "rbxassetid://140415804746906"
    sound.Volume = 1
    sound.Looped = false
    sound.Parent = GP
    sound:Play()

    -- ===== MAIN WINDOW =====
    local width, height = 480, 360
    local M = Instance.new("Frame", G)
    M.Size = UDim2.new(0, width, 0, height)
    M.Position = UDim2.new(0.5, -width/2, 0.25, 0)
    M.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
    M.BorderSizePixel = 0
    M.Active = true
    M.ClipsDescendants = true
    Instance.new("UICorner", M).CornerRadius = UDim.new(0, 15)

    local glowBorder = Instance.new("Frame", M)
    glowBorder.Size = UDim2.new(1, 6, 1, 6)
    glowBorder.Position = UDim2.new(0, -3, 0, -3)
    glowBorder.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
    glowBorder.BackgroundTransparency = 0.2
    glowBorder.BorderSizePixel = 0
    Instance.new("UICorner", glowBorder).CornerRadius = UDim.new(0, 18)

    -- Dragging
    local dragging, dragInput, dragStart, startPos
    M.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
            dragging = true
            dragStart = input.Position
            startPos = M.Position
            input.Changed:Connect(function()
                if input.UserInputState == Enum.UserInputState.End then dragging = false end
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

    -- Title bar
    local titleBg = Instance.new("Frame", M)
    titleBg.Size = UDim2.new(1, 0, 0, 55)
    titleBg.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
    titleBg.BorderSizePixel = 0

    local T = Instance.new("TextLabel", titleBg)
    T.Size = UDim2.new(1, 0, 1, 0)
    T.BackgroundTransparency = 1
    T.Text = "DADDY DEVIL SPAMMER 😈⚡"
    T.TextColor3 = Color3.new(1, 1, 1)
    T.TextSize = 18
    T.Font = Enum.Font.GothamBold

    local line1 = Instance.new("Frame", M)
    line1.Size = UDim2.new(0.9, 0, 0, 1)
    line1.Position = UDim2.new(0.05, 0, 0, 60)
    line1.BackgroundColor3 = Color3.fromRGB(10, 10, 10)
    line1.BorderSizePixel = 0

    -- ===== FIELD CREATOR =====
    local function createField(labelText, defaultValue, xPos, yPos, callback, isNumber)
        local container = Instance.new("Frame", M)
        container.Size = UDim2.new(0.45, 0, 0, 50)
        container.Position = UDim2.new(xPos, 0, 0, yPos)
        container.BackgroundTransparency = 1

        local label = Instance.new("TextLabel", container)
        label.Size = UDim2.new(1, 0, 0.35, 0)
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
        Instance.new("UICorner", boxBg).CornerRadius = UDim.new(0, 6)

        local inputBorder = Instance.new("Frame", boxBg)
        inputBorder.Size = UDim2.new(1, 0, 1, 0)
        inputBorder.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
        inputBorder.BorderSizePixel = 0
        Instance.new("UICorner", inputBorder).CornerRadius = UDim.new(0, 6)

        local box = Instance.new("TextBox", boxBg)
        box.Size = UDim2.new(1, -14, 1, 0)
        box.Position = UDim2.new(0, 7, 0, 0)
        box.BackgroundTransparency = 1
        box.Text = tostring(defaultValue)
        box.TextColor3 = Color3.new(1, 1, 1)
        box.TextSize = 13
        box.Font = Enum.Font.Gotham
        box.TextXAlignment = Enum.TextXAlignment.Left
        box.ClearTextOnFocus = false

        box.Focused:Connect(function()
            TweenService:Create(inputBorder, TweenInfo.new(0.2), { BackgroundColor3 = Color3.fromRGB(50, 50, 50) }):Play()
        end)
        box.FocusLost:Connect(function()
            TweenService:Create(inputBorder, TweenInfo.new(0.2), { BackgroundColor3 = Color3.fromRGB(20, 20, 20) }):Play()
            local value = box.Text
            if isNumber then
                value = tonumber(value) or defaultValue
                box.Text = tostring(value)
            end
            callback(value)
        end)
        return box
    end

    local targetBox = createField("🎯 TARGET", config.target, 0.05, 68, function(v) config.target = v end)
    createField("✏️ SYMBOL", config.symbol, 0.51, 68, function(v) config.symbol = v end)
    createField("🔢 COUNT", config.count, 0.05, 125, function(v) config.count = tonumber(v) or 150 end, true)
    createField("⏱️ SPEED", config.delay, 0.51, 125, function(v) config.delay = tonumber(v) or 0.8 end, true)

    local line2 = Instance.new("Frame", M)
    line2.Size = UDim2.new(0.9, 0, 0, 1)
    line2.Position = UDim2.new(0.05, 0, 0, 190)
    line2.BackgroundColor3 = Color3.fromRGB(10, 10, 10)
    line2.BorderSizePixel = 0

    -- ===== START BUTTON =====
    local startBtn = Instance.new("TextButton", M)
    startBtn.Size = UDim2.new(0.85, 0, 0, 42)
    startBtn.Position = UDim2.new(0.075, 0, 0, 200)
    startBtn.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
    startBtn.Text = "▶ START"
    startBtn.TextColor3 = Color3.new(1, 1, 1)
    startBtn.TextSize = 18
    startBtn.Font = Enum.Font.GothamBold
    Instance.new("UICorner", startBtn).CornerRadius = UDim.new(0, 10)

    startBtn.MouseButton1Click:Connect(function()
        config.enabled = not config.enabled
        startBtn.Text = config.enabled and "■ STOP" or "▶ START"
        statusLabel.Text = config.enabled and "⚡ SPAMMING..." or "● READY"
    end)

    -- ===== EDIT / MULTI / RX CHAT ROW =====
    local editBtn = Instance.new("TextButton", M)
    editBtn.Size = UDim2.new(0.28, 0, 0, 30)
    editBtn.Position = UDim2.new(0.05, 0, 0, 252)
    editBtn.BackgroundColor3 = Color3.fromRGB(20, 20, 25)
    editBtn.Text = "📝 EDIT"
    editBtn.TextColor3 = Color3.fromRGB(255, 180, 100)
    editBtn.TextSize = 12
    editBtn.Font = Enum.Font.GothamBold
    Instance.new("UICorner", editBtn).CornerRadius = UDim.new(0, 8)

    local multiBtn = Instance.new("TextButton", M)
    multiBtn.Size = UDim2.new(0.30, 0, 0, 30)
    multiBtn.Position = UDim2.new(0.35, 0, 0, 252)
    multiBtn.BackgroundColor3 = Color3.fromRGB(20, 20, 25)
    multiBtn.Text = "🎯 MULTI (10)"
    multiBtn.TextColor3 = Color3.fromRGB(255, 200, 100)
    multiBtn.TextSize = 12
    multiBtn.Font = Enum.Font.GothamBold
    Instance.new("UICorner", multiBtn).CornerRadius = UDim.new(0, 8)

    local rxBtn = Instance.new("TextButton", M)
    rxBtn.Size = UDim2.new(0.28, 0, 0, 30)
    rxBtn.Position = UDim2.new(0.67, 0, 0, 252)
    rxBtn.BackgroundColor3 = Color3.fromRGB(20, 20, 25)
    rxBtn.Text = "💬 RX CHAT"
    rxBtn.TextColor3 = Color3.fromRGB(0, 212, 255)
    rxBtn.TextSize = 12
    rxBtn.Font = Enum.Font.GothamBold
    Instance.new("UICorner", rxBtn).CornerRadius = UDim.new(0, 8)

    -- ===== STATUS LABELS =====
    local statusLabel = Instance.new("TextLabel", M)
    statusLabel.Size = UDim2.new(0.9, 0, 0, 22)
    statusLabel.Position = UDim2.new(0.05, 0, 1, -47)
    statusLabel.BackgroundTransparency = 1
    statusLabel.Text = "● READY"
    statusLabel.TextColor3 = Color3.new(1, 1, 1)
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

    -- ===== MINIMIZE =====
    local minBtn = Instance.new("TextButton", M)
    minBtn.Size = UDim2.new(0, 30, 0, 30)
    minBtn.Position = UDim2.new(1, -40, 0, 8)
    minBtn.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
    minBtn.Text = "−"
    minBtn.TextColor3 = Color3.fromRGB(200, 200, 200)
    minBtn.TextSize = 16
    minBtn.Font = Enum.Font.GothamBold
    Instance.new("UICorner", minBtn).CornerRadius = UDim.new(0, 6)
    minBtn.BorderSizePixel = 1
    minBtn.BorderColor3 = Color3.fromRGB(30, 30, 30)

    local floatBtn = Instance.new("TextButton", G)
    floatBtn.Size = UDim2.new(0, 55, 0, 55)
    floatBtn.Position = UDim2.new(0.93, 0, 0.85, 0)
    floatBtn.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
    floatBtn.Text = "😈"
    floatBtn.TextColor3 = Color3.new(1, 1, 1)
    floatBtn.TextSize = 28
    floatBtn.Visible = false
    floatBtn.ZIndex = 10
    Instance.new("UICorner", floatBtn).CornerRadius = UDim.new(1, 0)
    floatBtn.BorderSizePixel = 2
    floatBtn.BorderColor3 = Color3.fromRGB(30, 30, 30)

    minBtn.MouseButton1Click:Connect(function()
        M.Visible = not M.Visible
        floatBtn.Visible = not M.Visible
    end)
    floatBtn.MouseButton1Click:Connect(function()
        M.Visible = true
        floatBtn.Visible = false
    end)

    -- ============================================
    -- MULTI TARGET PANEL (10 SLOTS, 5x2)
    -- ============================================
    local MPanel = Instance.new("Frame", G)
    MPanel.Name = "DevilMultiTarget"
    MPanel.Size = UDim2.new(0, 430, 0, 262)
    MPanel.Position = UDim2.new(0.5, -215, 0.55, -131)
    MPanel.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
    MPanel.BorderSizePixel = 2
    MPanel.BorderColor3 = Color3.fromRGB(255, 50, 50)
    MPanel.Active = true
    MPanel.Draggable = true
    MPanel.Visible = false
    MPanel.ZIndex = 30
    Instance.new("UICorner", MPanel).CornerRadius = UDim.new(0, 10)

    local MTitle = Instance.new("TextLabel", MPanel)
    MTitle.Size = UDim2.new(1, -35, 0, 32)
    MTitle.Position = UDim2.new(0, 0, 0, 0)
    MTitle.BackgroundColor3 = Color3.fromRGB(15, 0, 0)
    MTitle.BorderSizePixel = 0
    MTitle.Text = "  🎯 DEVIL MULTI TARGET (10)"
    MTitle.TextColor3 = Color3.fromRGB(255, 80, 80)
    MTitle.TextSize = 14
    MTitle.Font = Enum.Font.GothamBold
    MTitle.TextXAlignment = Enum.TextXAlignment.Left
    MTitle.ZIndex = 31
    Instance.new("UICorner", MTitle).CornerRadius = UDim.new(0, 10)

    local MClose = Instance.new("TextButton", MPanel)
    MClose.Size = UDim2.new(0, 35, 0, 32)
    MClose.Position = UDim2.new(1, -35, 0, 0)
    MClose.BackgroundColor3 = Color3.fromRGB(30, 0, 0)
    MClose.Text = "X"
    MClose.TextColor3 = Color3.new(1, 1, 1)
    MClose.TextSize = 14
    MClose.Font = Enum.Font.GothamBold
    MClose.BorderSizePixel = 0
    MClose.ZIndex = 32
    MClose.MouseButton1Click:Connect(function()
        MPanel.Visible = false
    end)

    local MHint = Instance.new("TextLabel", MPanel)
    MHint.Size = UDim2.new(1, -30, 0, 16)
    MHint.Position = UDim2.new(0, 15, 0, 34)
    MHint.BackgroundTransparency = 1
    MHint.Text = "Fill any slot to auto-enable MULTI. Blanks skipped. Cycles only filled targets."
    MHint.TextColor3 = Color3.fromRGB(200, 160, 160)
    MHint.TextSize = 10
    MHint.Font = Enum.Font.Gotham
    MHint.TextXAlignment = Enum.TextXAlignment.Left
    MHint.ZIndex = 31

    local COL_X = {15, 96, 177, 258, 339}
    local COL_W = 73
    local ROW_Y = {70, 134}
    local LBL_Y = {54, 118}

    local function createSlotBox(parent, size, position, placeholder)
        local tb = Instance.new("TextBox")
        tb.Size = size
        tb.Position = position
        tb.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
        tb.TextColor3 = Color3.new(1, 1, 1)
        tb.PlaceholderText = placeholder
        tb.PlaceholderColor3 = Color3.fromRGB(120, 120, 120)
        tb.Text = ""
        tb.TextSize = 10
        tb.Font = Enum.Font.Gotham
        tb.ClearTextOnFocus = false
        tb.BorderSizePixel = 1
        tb.BorderColor3 = Color3.fromRGB(80, 30, 30)
        tb.ZIndex = 32
        tb.Parent = parent
        Instance.new("UICorner", tb).CornerRadius = UDim.new(0, 5)
        return tb
    end

    for i = 1, TOTAL_SLOTS do
        local col = ((i - 1) % 5) + 1
        local row = math.floor((i - 1) / 5) + 1

        local lbl = Instance.new("TextLabel", MPanel)
        lbl.Size = UDim2.new(0, COL_W, 0, 14)
        lbl.Position = UDim2.new(0, COL_X[col], 0, LBL_Y[row])
        lbl.BackgroundTransparency = 1
        lbl.Text = "TARGET " .. i
        lbl.TextColor3 = Color3.fromRGB(200, 160, 160)
        lbl.TextSize = 9
        lbl.Font = Enum.Font.GothamBold
        lbl.ZIndex = 31

        local box = createSlotBox(MPanel, UDim2.new(0, COL_W, 0, 34), UDim2.new(0, COL_X[col], 0, ROW_Y[row]), "T" .. i .. " name")
        multiTargets[i] = box
    end

    local multiToggleBtn = Instance.new("TextButton", MPanel)
    multiToggleBtn.Size = UDim2.new(0, 200, 0, 38)
    multiToggleBtn.Position = UDim2.new(0, 15, 0, 178)
    multiToggleBtn.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
    multiToggleBtn.Text = "AUTO MULTI: OFF"
    multiToggleBtn.TextColor3 = Color3.new(1, 1, 1)
    multiToggleBtn.TextSize = 12
    multiToggleBtn.Font = Enum.Font.GothamBold
    multiToggleBtn.AutoButtonColor = false
    multiToggleBtn.BorderSizePixel = 0
    multiToggleBtn.ZIndex = 32
    Instance.new("UICorner", multiToggleBtn).CornerRadius = UDim.new(0, 8)

    local clearAllBtn = Instance.new("TextButton", MPanel)
    clearAllBtn.Size = UDim2.new(0, 100, 0, 38)
    clearAllBtn.Position = UDim2.new(0, 225, 0, 178)
    clearAllBtn.BackgroundColor3 = Color3.fromRGB(60, 20, 20)
    clearAllBtn.Text = "CLEAR ALL"
    clearAllBtn.TextColor3 = Color3.new(1, 1, 1)
    clearAllBtn.TextSize = 12
    clearAllBtn.Font = Enum.Font.GothamBold
    clearAllBtn.BorderSizePixel = 0
    clearAllBtn.ZIndex = 32
    Instance.new("UICorner", clearAllBtn).CornerRadius = UDim.new(0, 8)

    local MStatus = Instance.new("TextLabel", MPanel)
    MStatus.Size = UDim2.new(1, -30, 0, 18)
    MStatus.Position = UDim2.new(0, 15, 0, 224)
    MStatus.BackgroundTransparency = 1
    MStatus.Text = "Mode: SINGLE (uses Target box)"
    MStatus.TextColor3 = Color3.fromRGB(200, 160, 160)
    MStatus.TextSize = 11
    MStatus.Font = Enum.Font.Gotham
    MStatus.TextXAlignment = Enum.TextXAlignment.Left
    MStatus.ZIndex = 31

    local function getFilledTargets()
        local filled = {}
        for i = 1, TOTAL_SLOTS do
            local box = multiTargets[i]
            if box then
                local txt = box.Text
                if txt and txt:gsub("%s", "") ~= "" then
                    table.insert(filled, txt)
                end
            end
        end
        return filled
    end

    local function refreshAutoMulti()
        local n = #getFilledTargets()
        if n > 0 then
            multiMode = true
            multiToggleBtn.BackgroundColor3 = Color3.fromRGB(0, 170, 0)
            multiToggleBtn.Text = "AUTO MULTI: ON"
            MStatus.Text = "Mode: MULTI (auto, cycling " .. tostring(n) .. " filled target" .. (n == 1 and "" or "s") .. ")"
            MStatus.TextColor3 = Color3.fromRGB(0, 220, 100)

            targetBox.TextEditable = false
            targetBox.TextColor3 = Color3.fromRGB(120, 120, 120)
            targetBox.PlaceholderText = "🔒"
        else
            multiMode = false
            multiToggleBtn.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
            multiToggleBtn.Text = "AUTO MULTI: OFF"
            MStatus.Text = "Mode: SINGLE (uses Target box)"
            MStatus.TextColor3 = Color3.fromRGB(200, 160, 160)

            targetBox.TextEditable = true
            targetBox.TextColor3 = Color3.new(1, 1, 1)
            targetBox.PlaceholderText = ""
        end
    end

    for _, box in ipairs(multiTargets) do
        box:GetPropertyChangedSignal("Text"):Connect(refreshAutoMulti)
        box.FocusLost:Connect(function()
            task.wait()
            refreshAutoMulti()
        end)
    end

    clearAllBtn.MouseButton1Click:Connect(function()
        for _, b in ipairs(multiTargets) do
            b.Text = ""
        end
    end)

    multiBtn.MouseButton1Click:Connect(function()
        MPanel.Visible = not MPanel.Visible
    end)

    refreshAutoMulti()

    -- ============================================
    -- MESSAGE EDITOR
    -- ============================================
    local editor = Instance.new("Frame", G)
    editor.Size = UDim2.fromOffset(430, 360)
    editor.Position = UDim2.new(0.5, -215, 0.28, 0)
    editor.BackgroundColor3 = Color3.fromRGB(10, 10, 12)
    editor.BorderColor3 = Color3.fromRGB(255, 50, 50)
    editor.BorderSizePixel = 2
    editor.Visible = false
    editor.Active = true
    editor.ZIndex = 20
    Instance.new("UICorner", editor).CornerRadius = UDim.new(0, 12)

    local editorTitle = Instance.new("TextLabel", editor)
    editorTitle.Size = UDim2.new(1, -45, 0, 42)
    editorTitle.BackgroundColor3 = Color3.fromRGB(255, 50, 50)
    editorTitle.Text = "📝 EDIT MESSAGES • DRAG HERE"
    editorTitle.TextColor3 = Color3.new(0, 0, 0)
    editorTitle.TextSize = 15
    editorTitle.Font = Enum.Font.GothamBold
    editorTitle.ZIndex = 21

    -- Editor drag
    local eDragging, eInput, eStart, ePos
    editorTitle.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
            eDragging = true
            eStart = input.Position
            ePos = editor.Position
            input.Changed:Connect(function()
                if input.UserInputState == Enum.UserInputState.End then eDragging = false end
            end)
        end
    end)
    editorTitle.InputChanged:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch then
            eInput = input
        end
    end)
    UserInputService.InputChanged:Connect(function(input)
        if input == eInput and eDragging then
            local delta = input.Position - eStart
            editor.Position = UDim2.new(ePos.X.Scale, ePos.X.Offset + delta.X, ePos.Y.Scale, ePos.Y.Offset + delta.Y)
        end
    end)

    local closeEditor = Instance.new("TextButton", editor)
    closeEditor.Size = UDim2.fromOffset(32, 30)
    closeEditor.Position = UDim2.new(1, -38, 0, 6)
    closeEditor.BackgroundColor3 = Color3.fromRGB(190, 45, 45)
    closeEditor.Text = "X"
    closeEditor.TextColor3 = Color3.new(1, 1, 1)
    closeEditor.TextSize = 15
    closeEditor.Font = Enum.Font.GothamBold
    closeEditor.ZIndex = 23
    Instance.new("UICorner", closeEditor).CornerRadius = UDim.new(0, 6)

    local scroll = Instance.new("ScrollingFrame", editor)
    scroll.Size = UDim2.new(1, -20, 1, -96)
    scroll.Position = UDim2.fromOffset(10, 48)
    scroll.BackgroundColor3 = Color3.fromRGB(10, 10, 12)
    scroll.BorderSizePixel = 0
    scroll.ScrollBarThickness = 8
    scroll.ScrollBarImageColor3 = Color3.fromRGB(255, 50, 50)
    scroll.CanvasSize = UDim2.fromOffset(0, 0)
    scroll.ZIndex = 21
    Instance.new("UICorner", scroll).CornerRadius = UDim.new(0, 6)

    local addButton = Instance.new("TextButton", editor)
    addButton.Size = UDim2.fromOffset(125, 32)
    addButton.Position = UDim2.new(0, 14, 1, -42)
    addButton.BackgroundColor3 = Color3.fromRGB(0, 200, 80)
    addButton.Text = "➕ ADD"
    addButton.TextColor3 = Color3.new(0, 0, 0)
    addButton.TextSize = 14
    addButton.Font = Enum.Font.GothamBold
    addButton.ZIndex = 22
    Instance.new("UICorner", addButton).CornerRadius = UDim.new(0, 6)

    local saveButton = Instance.new("TextButton", editor)
    saveButton.Size = UDim2.fromOffset(125, 32)
    saveButton.Position = UDim2.new(1, -139, 1, -42)
    saveButton.BackgroundColor3 = Color3.fromRGB(255, 50, 50)
    saveButton.Text = "💾 SAVE"
    saveButton.TextColor3 = Color3.new(0, 0, 0)
    saveButton.TextSize = 14
    saveButton.Font = Enum.Font.GothamBold
    saveButton.ZIndex = 22
    Instance.new("UICorner", saveButton).CornerRadius = UDim.new(0, 6)

    local function rebuildEditor()
        for _, child in ipairs(scroll:GetChildren()) do
            child:Destroy()
        end

        local y = 6

        for index, message in ipairs(messages) do
            local row = Instance.new("Frame", scroll)
            row.Name = "MessageRow_" .. index
            row.Size = UDim2.new(1, -12, 0, 40)
            row.Position = UDim2.fromOffset(6, y)
            row.BackgroundColor3 = Color3.fromRGB(20, 20, 25)
            row.BorderSizePixel = 0
            row.ZIndex = 22
            Instance.new("UICorner", row).CornerRadius = UDim.new(0, 5)

            local box = Instance.new("TextBox", row)
            box.Name = "MessageText"
            box.Size = UDim2.new(1, -54, 1, 0)
            box.Position = UDim2.fromOffset(7, 0)
            box.BackgroundTransparency = 1
            box.Text = tostring(message)
            box.PlaceholderText = "Type message here..."
            box.TextColor3 = Color3.new(1, 1, 1)
            box.PlaceholderColor3 = Color3.fromRGB(140, 140, 150)
            box.TextSize = 13
            box.Font = Enum.Font.Gotham
            box.TextXAlignment = Enum.TextXAlignment.Left
            box.ClearTextOnFocus = false
            box.ZIndex = 23

            box.FocusLost:Connect(function()
                messages[index] = box.Text
                row.BackgroundColor3 = Color3.fromRGB(20, 20, 25)
            end)
            box.Focused:Connect(function()
                row.BackgroundColor3 = Color3.fromRGB(40, 30, 30)
            end)

            local delBtn = Instance.new("TextButton", row)
            delBtn.Size = UDim2.fromOffset(32, 30)
            delBtn.Position = UDim2.new(1, -38, 0.5, -15)
            delBtn.BackgroundColor3 = Color3.fromRGB(195, 50, 50)
            delBtn.Text = "✕"
            delBtn.TextColor3 = Color3.new(1, 1, 1)
            delBtn.TextSize = 14
            delBtn.Font = Enum.Font.GothamBold
            delBtn.BorderSizePixel = 0
            delBtn.ZIndex = 24
            Instance.new("UICorner", delBtn).CornerRadius = UDim.new(0, 5)

            delBtn.MouseButton1Click:Connect(function()
                table.remove(messages, index)
                rebuildEditor()
                msgCounter.Text = "📨 " .. #messages .. " messages loaded"
            end)

            y += 45
        end

        if #messages == 0 then
            local empty = Instance.new("TextLabel", scroll)
            empty.Size = UDim2.new(1, 0, 0, 32)
            empty.Position = UDim2.fromOffset(0, 8)
            empty.BackgroundTransparency = 1
            empty.Text = "No messages. Click ADD."
            empty.TextColor3 = Color3.fromRGB(220, 220, 220)
            empty.TextSize = 14
            empty.Font = Enum.Font.Gotham
            empty.ZIndex = 23
            y = 48
        end

        scroll.CanvasSize = UDim2.fromOffset(0, y + 8)
    end

    local editorOpen = false
    editBtn.MouseButton1Click:Connect(function()
        editorOpen = not editorOpen
        editor.Visible = editorOpen
        if editorOpen then rebuildEditor() end
    end)
    closeEditor.MouseButton1Click:Connect(function()
        editor.Visible = false
        editorOpen = false
    end)
    addButton.MouseButton1Click:Connect(function()
        table.insert(messages, "New Message " .. (#messages + 1))
        rebuildEditor()
        msgCounter.Text = "📨 " .. #messages .. " messages loaded"
    end)
    saveButton.MouseButton1Click:Connect(function()
        for _, row in ipairs(scroll:GetChildren()) do
            if row:IsA("Frame") then
                local box = row:FindFirstChild("MessageText")
                local idx = tonumber(row.Name:match("MessageRow_(%d+)"))
                if box and idx then messages[idx] = box.Text end
            end
        end
        saveMessages(messages)
        msgCounter.Text = "📨 " .. #messages .. " messages loaded"
        editor.Visible = false
        editorOpen = false
    end)

    -- ============================================
    -- RX CHAT AUTO-EXECUTE
    -- ============================================
    local RX_CHAT_URL = "https://raw.githubusercontent.com/tapashazra2345-netizen/RX-CHAT-V2/refs/heads/main/SCRIPT"

    rxBtn.MouseButton1Click:Connect(function()
        rxBtn.Text = "⏳ LOADING..."
        rxBtn.TextColor3 = Color3.fromRGB(255, 220, 120)
        statusLabel.Text = "💬 Executing RX Chat..."
        statusLabel.TextColor3 = Color3.fromRGB(0, 212, 255)

        task.spawn(function()
            local execStatus = "❌ Execute failed"
            local ok, e = pcall(function()
                local fetchOk, content = pcall(game.HttpGet, game, RX_CHAT_URL)
                if fetchOk and content then
                    local fn, loadErr = loadstring(content)
                    if fn then
                        fn()
                        execStatus = "✅ RX Chat executed!"
                    else
                        execStatus = "❌ Load error: " .. tostring(loadErr)
                    end
                else
                    execStatus = "❌ Fetch failed"
                end
            end)

            if not ok then execStatus = "❌ Error: " .. tostring(e) end

            statusLabel.Text = execStatus
            statusLabel.TextColor3 = (execStatus:find("✅") and Color3.fromRGB(100, 255, 100)) or Color3.fromRGB(255, 100, 100)

            if execStatus:find("✅") then
                rxBtn.Text = "💬 RX CHAT"
                rxBtn.TextColor3 = Color3.fromRGB(0, 212, 255)
            else
                rxBtn.Text = "💬 RETRY"
                rxBtn.TextColor3 = Color3.fromRGB(255, 100, 100)
            end

            task.delay(4, function()
                statusLabel.Text = config.enabled and "⚡ SPAMMING..." or "● READY"
                statusLabel.TextColor3 = Color3.new(1, 1, 1)
            end)
        end)
    end)

    -- ============================================
    -- SPAM LOOP (multi-target cycling)
    -- ============================================
    task.spawn(function()
        while G.Parent do
            if config.enabled and #messages > 0 then
                local msg = messages[currentMsg]
                if msg and msg ~= "" then
                    local repeatCount = math.min(config.count, 200)
                    local prefix = string.rep(config.symbol, repeatCount)

                    local targetName = config.target
                    if multiMode then
                        local list = getFilledTargets()
                        if #list > 0 then
                            local tIdx = ((currentMsg - 1) % #list) + 1
                            targetName = list[tIdx]
                        end
                    end

                    local fullMsg = prefix .. " [" .. targetName .. "] " .. msg
                    send(fullMsg)
                    statusLabel.Text = "⚡ " .. msg:sub(1, 24)
                    statusLabel.TextColor3 = Color3.fromRGB(200, 200, 200)
                    currentMsg = (currentMsg % #messages) + 1
                end
            end
            task.wait(config.delay)
        end
    end)

    -- Entrance animation
    M.Size = UDim2.new(0, 0, 0, 0)
    M.Position = UDim2.new(0.5, 0, 0.25, 0)
    TweenService:Create(M, TweenInfo.new(0.6, Enum.EasingStyle.Back), {
        Size = UDim2.new(0, width, 0, height),
        Position = UDim2.new(0.5, -width/2, 0.25, 0)
    }):Play()

    task.wait(0.5)
    pcall(function()
        send(string.rep("~", 150) .. " DADDY DEVIL SPAMMER LOADED 😈⚡")
    end)

    print("✅ DADDY DEVIL SPAMMER ready (loading + multi + RX + editor).")
end)

if not success then
    warn("❌ Error loading spammer:", err)
end