-- MINI HUB LIBRARY - Versão Final Corrigida (Icon + Elementos garantidos)
local Library = {}

local Players = game:GetService("Players")
local UserInputService = game:GetService("UserInputService")

local player = Players.LocalPlayer
local gethui = gethui or function() return player:WaitForChild("PlayerGui") end

local screenGui = Instance.new("ScreenGui")
screenGui.Name = "MiniHubLibrary"
screenGui.ResetOnSpawn = false
screenGui.Parent = gethui()

local mainFrame = Instance.new("Frame")
mainFrame.Name = "MainFrame"
mainFrame.BackgroundColor3 = Color3.fromRGB(26, 26, 26)
mainFrame.Size = UDim2.new(0, 580, 0, 420)
mainFrame.Position = UDim2.new(0.5, -290, 0.5, -210)
mainFrame.Parent = screenGui
Instance.new("UICorner", mainFrame).CornerRadius = UDim.new(0, 14)

local titleBar = Instance.new("Frame")
titleBar.Size = UDim2.new(1, 0, 0, 50)
titleBar.BackgroundColor3 = Color3.fromRGB(255, 140, 0)
titleBar.Parent = mainFrame
Instance.new("UICorner", titleBar).CornerRadius = UDim.new(0, 14)

local titleLabel = Instance.new("TextLabel")
titleLabel.Size = UDim2.new(1, -100, 1, 0)
titleLabel.BackgroundTransparency = 1
titleLabel.Text = "MINI HUB"
titleLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
titleLabel.TextScaled = true
titleLabel.Font = Enum.Font.GothamBold
titleLabel.Parent = titleBar

local minimizeBtn = Instance.new("TextButton")
minimizeBtn.Size = UDim2.new(0, 35, 0, 35)
minimizeBtn.Position = UDim2.new(1, -80, 0.5, -17.5)
minimizeBtn.BackgroundTransparency = 1
minimizeBtn.Text = "−"
minimizeBtn.TextColor3 = Color3.fromRGB(0,0,0)
minimizeBtn.TextScaled = true
minimizeBtn.Parent = titleBar

local closeBtn = Instance.new("TextButton")
closeBtn.Size = UDim2.new(0, 35, 0, 35)
closeBtn.Position = UDim2.new(1, -42, 0.5, -17.5)
closeBtn.BackgroundTransparency = 1
closeBtn.Text = "×"
closeBtn.TextColor3 = Color3.fromRGB(0,0,0)
closeBtn.TextScaled = true
closeBtn.Parent = titleBar

local container = Instance.new("Frame")
container.Size = UDim2.new(1, 0, 1, -50)
container.Position = UDim2.new(0, 0, 0, 50)
container.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
container.Parent = mainFrame

local sidebar = Instance.new("ScrollingFrame")
sidebar.Size = UDim2.new(0, 70, 1, 0)
sidebar.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
sidebar.ScrollBarThickness = 4
sidebar.Parent = container

local sidebarLayout = Instance.new("UIListLayout")
sidebarLayout.SortOrder = Enum.SortOrder.LayoutOrder
sidebarLayout.Padding = UDim.new(0, 8)
sidebarLayout.Parent = sidebar

local content = Instance.new("Frame")
content.Size = UDim2.new(1, -70, 1, 0)
content.Position = UDim2.new(0, 70, 0, 0)
content.BackgroundColor3 = Color3.fromRGB(26, 26, 26)
content.Parent = container

local contentPadding = Instance.new("UIPadding")
contentPadding.PaddingLeft = UDim.new(0, 12)
contentPadding.PaddingRight = UDim.new(0, 12)
contentPadding.PaddingTop = UDim.new(0, 12)
contentPadding.Parent = content

-- Drag
local dragging = false
local dragStart, startPos
titleBar.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
        dragging = true
        dragStart = input.Position
        startPos = mainFrame.Position
    end
end)
UserInputService.InputChanged:Connect(function(input)
    if dragging and (input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch) then
        local delta = input.Position - dragStart
        mainFrame.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
    end
end)
UserInputService.InputEnded:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then dragging = false end
end)

-- Bolinha
local orb = Instance.new("TextButton")
orb.Size = UDim2.new(0, 60, 0, 60)
orb.Position = UDim2.new(0.5, -30, 0.85, 0)
orb.BackgroundColor3 = Color3.fromRGB(255, 140, 0)
orb.Text = "M"
orb.TextColor3 = Color3.fromRGB(0,0,0)
orb.TextScaled = true
orb.Font = Enum.Font.GothamBold
orb.Visible = false
orb.Parent = screenGui
Instance.new("UICorner", orb).CornerRadius = UDim.new(1, 0)

minimizeBtn.MouseButton1Click:Connect(function() mainFrame.Visible = false; orb.Visible = true end)
orb.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
        local orbDragging = true
        local orbDragStart = input.Position
        local orbStartPos = orb.Position
        local conn
        conn = UserInputService.InputChanged:Connect(function(inp)
            if orbDragging and (inp.UserInputType == Enum.UserInputType.MouseMovement or inp.UserInputType == Enum.UserInputType.Touch) then
                local delta = inp.Position - orbDragStart
                orb.Position = UDim2.new(orbStartPos.X.Scale, orbStartPos.X.Offset + delta.X, orbStartPos.Y.Scale, orbStartPos.Y.Offset + delta.Y)
            end
        end)
        UserInputService.InputEnded:Connect(function() orbDragging = false; conn:Disconnect() end)
    end
end)
orb.MouseButton1Click:Connect(function() mainFrame.Visible = true; orb.Visible = false end)
closeBtn.MouseButton1Click:Connect(function() screenGui:Destroy() end)

local tabs = {}

function Library:CreateWindow(config)
    titleLabel.Text = config.Title or "MINI HUB"
    return Library
end

function Library:CreateTab(config)
    local name = config.Name or "Tab"
    local icon = config.Icon or "📄"

    local tabBtn = Instance.new("TextButton")
    tabBtn.Size = UDim2.new(1, -10, 0, 70)
    tabBtn.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
    tabBtn.Text = ""
    tabBtn.Parent = sidebar
    Instance.new("UICorner", tabBtn).CornerRadius = UDim.new(0, 8)

    -- ÍCONE REFORÇADO
    local iconObj
    if string.match(icon, "^https?://") or string.find(icon, "rbxassetid") then
        iconObj = Instance.new("ImageLabel")
        iconObj.Image = icon
        iconObj.ScaleType = Enum.ScaleType.Fit
        iconObj.ResampleMode = Enum.ResamplerMode.Pixelated
        iconObj.BackgroundTransparency = 1
    else
        iconObj = Instance.new("TextLabel")
        iconObj.Text = icon
        iconObj.TextScaled = true
        iconObj.Font = Enum.Font.GothamBold
        iconObj.BackgroundTransparency = 1
    end
    iconObj.Size = UDim2.new(1, 0, 0.6, 0)
    iconObj.Position = UDim2.new(0, 0, 0, 0)
    iconObj.Parent = tabBtn

    local nameLabel = Instance.new("TextLabel")
    nameLabel.Size = UDim2.new(1, 0, 0.4, 0)
    nameLabel.Position = UDim2.new(0, 0, 0.6, 0)
    nameLabel.BackgroundTransparency = 1
    nameLabel.Text = name
    nameLabel.TextScaled = true
    nameLabel.Font = Enum.Font.GothamSemibold
    nameLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
    nameLabel.Parent = tabBtn

    local tabContent = Instance.new("ScrollingFrame")
    tabContent.Name = "TabContent_"..name
    tabContent.Size = UDim2.new(1, 0, 1, 0)
    tabContent.BackgroundTransparency = 1
    tabContent.ScrollBarThickness = 6
    tabContent.Visible = false
    tabContent.AutomaticCanvasSize = Enum.AutomaticSize.Y
    tabContent.ScrollingDirection = Enum.ScrollingDirection.Y
    tabContent.ClipsDescendants = true
    tabContent.Parent = content

    local layout = Instance.new("UIListLayout")
    layout.SortOrder = Enum.SortOrder.LayoutOrder
    layout.Padding = UDim.new(0, 10)
    layout.Parent = tabContent

    table.insert(tabs, {btn = tabBtn, content = tabContent})

    tabBtn.MouseButton1Click:Connect(function()
        for _, t in ipairs(tabs) do
            t.content.Visible = false
            t.btn.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
        end
        tabContent.Visible = true
        tabBtn.BackgroundColor3 = Color3.fromRGB(255, 140, 0)
    end)

    if #tabs == 1 then
        tabContent.Visible = true
        tabBtn.BackgroundColor3 = Color3.fromRGB(255, 140, 0)
    end

    return {
        CreateButton = function(self, cfg)
            local b = Instance.new("TextButton")
            b.Size = UDim2.new(1, 0, 0, 46)
            b.BackgroundColor3 = Color3.fromRGB(55, 55, 55)
            b.Text = cfg.Name
            b.TextColor3 = Color3.new(1,1,1)
            b.TextScaled = true
            b.Font = Enum.Font.GothamSemibold
            b.Parent = tabContent
            Instance.new("UICorner", b).CornerRadius = UDim.new(0, 8)
            b.MouseButton1Click:Connect(cfg.Callback or function() end)
            task.wait() -- força update
        end,

        CreateToggle = function(self, cfg)
            local state = cfg.CurrentValue or false
            local toggle = Instance.new("TextButton")
            toggle.Size = UDim2.new(1, 0, 0, 46)
            toggle.BackgroundColor3 = Color3.fromRGB(55,55,55)
            toggle.TextColor3 = Color3.new(1,1,1)
            toggle.TextScaled = true
            toggle.Font = Enum.Font.GothamSemibold
            toggle.Parent = tabContent
            Instance.new("UICorner", toggle).CornerRadius = UDim.new(0,8)

            local function Update()
                if state then
                    toggle.Text = cfg.Name .. "  [ON]"
                    toggle.BackgroundColor3 = Color3.fromRGB(0,170,80)
                else
                    toggle.Text = cfg.Name .. "  [OFF]"
                    toggle.BackgroundColor3 = Color3.fromRGB(55,55,55)
                end
            end
            Update()

            toggle.MouseButton1Click:Connect(function()
                state = not state
                Update()
                if cfg.Callback then cfg.Callback(state) end
            end)
        end,

        CreateSlider = function(self, cfg)
            local min = cfg.Min or 0
            local max = cfg.Max or 100
            local value = cfg.Default or min
            local callback = cfg.Callback or function() end

            local sliderFrame = Instance.new("Frame")
            sliderFrame.Size = UDim2.new(1, 0, 0, 60)
            sliderFrame.BackgroundTransparency = 1
            sliderFrame.Parent = tabContent

            local nameLabel = Instance.new("TextLabel")
            nameLabel.Size = UDim2.new(1, 0, 0, 20)
            nameLabel.BackgroundTransparency = 1
            nameLabel.Text = cfg.Name .. ": " .. math.floor(value)
            nameLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
            nameLabel.TextScaled = true
            nameLabel.Font = Enum.Font.GothamSemibold
            nameLabel.Parent = sliderFrame

            local bar = Instance.new("Frame")
            bar.Size = UDim2.new(1, 0, 0, 8)
            bar.Position = UDim2.new(0, 0, 0, 30)
            bar.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
            bar.Parent = sliderFrame
            Instance.new("UICorner", bar).CornerRadius = UDim.new(0, 4)

            local fill = Instance.new("Frame")
            fill.Size = UDim2.new(0, 0, 1, 0)
            fill.BackgroundColor3 = Color3.fromRGB(255, 140, 0)
            fill.Parent = bar
            Instance.new("UICorner", fill).CornerRadius = UDim.new(0, 4)

            local knob = Instance.new("TextButton")
            knob.Size = UDim2.new(0, 20, 0, 20)
            knob.Position = UDim2.new(0, 0, 0.5, -10)
            knob.BackgroundColor3 = Color3.fromRGB(255, 200, 100)
            knob.Text = ""
            knob.Parent = bar
            Instance.new("UICorner", knob).CornerRadius = UDim.new(1, 0)

            local dragging = false

            local function updateSlider()
                local percent = math.clamp((value - min) / (max - min), 0, 1)
                fill.Size = UDim2.new(percent, 0, 1, 0)
                knob.Position = UDim2.new(percent, -10, 0.5, -10)
                nameLabel.Text = cfg.Name .. ": " .. math.floor(value)
            end

            local function setValue(newValue)
                value = math.clamp(newValue, min, max)
                updateSlider()
                callback(value)
            end

            local function onInputBegan(input)
                if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
                    dragging = true
                    local mousePos = input.Position
                    local barPos = bar.AbsolutePosition
                    local barSize = bar.AbsoluteSize
                    local percent = math.clamp((mousePos.X - barPos.X) / barSize.X, 0, 1)
                    setValue(min + (max - min) * percent)
                end
            end

            knob.InputBegan:Connect(onInputBegan)
            bar.InputBegan:Connect(onInputBegan)
            UserInputService.InputChanged:Connect(function(input)
                if dragging and (input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch) then
                    local mousePos = input.Position
                    local barPos = bar.AbsolutePosition
                    local barSize = bar.AbsoluteSize
                    local percent = math.clamp((mousePos.X - barPos.X) / barSize.X, 0, 1)
                    setValue(min + (max - min) * percent)
                end
            end)

            UserInputService.InputEnded:Connect(function(input)
                if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then dragging = false end
            end)

            updateSlider()
        end
    }
end

sidebarLayout:GetPropertyChangedSignal("AbsoluteContentSize"):Connect(function()
    sidebar.CanvasSize = UDim2.new(0, 0, 0, sidebarLayout.AbsoluteContentSize.Y + 20)
end)

print("MINI HUB - Corrigido e blindado!")
return Library
