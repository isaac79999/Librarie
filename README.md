-- MINI HUB LIBRARY - Completa e Corrigida
local Library = {}

local Players = game:GetService("Players")
local UserInputService = game:GetService("UserInputService")
local RunService = game:GetService("RunService")

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

-- Title Bar
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

-- Controls
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

-- Container
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

local content = Instance.new("ScrollingFrame")
content.Size = UDim2.new(1, -70, 1, 0)
content.Position = UDim2.new(0, 70, 0, 0)
content.BackgroundColor3 = Color3.fromRGB(26, 26, 26)
content.ScrollBarThickness = 5
content.Parent = container

local contentPadding = Instance.new("UIPadding")
contentPadding.PaddingLeft = UDim.new(0, 12)
contentPadding.PaddingRight = UDim.new(0, 12)
contentPadding.PaddingTop = UDim.new(0, 12)
contentPadding.Parent = content

-- Drag do painel
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

-- Bolinha Arrastável
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

-- Minimizar
minimizeBtn.MouseButton1Click:Connect(function()
    mainFrame.Visible = false
    orb.Visible = true
end)

-- Drag da bolinha
local orbDragging = false
local orbDragStart, orbStartPos

orb.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
        orbDragging = true
        orbDragStart = input.Position
        orbStartPos = orb.Position
    end
end)

UserInputService.InputChanged:Connect(function(input)
    if orbDragging and (input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch) then
        local delta = input.Position - orbDragStart
        orb.Position = UDim2.new(orbStartPos.X.Scale, orbStartPos.X.Offset + delta.X, orbStartPos.Y.Scale, orbStartPos.Y.Offset + delta.Y)
    end
end)

UserInputService.InputEnded:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then orbDragging = false end
end)

-- Restaurar
orb.MouseButton1Click:Connect(function()
    mainFrame.Visible = true
    orb.Visible = false
    mainFrame.Position = UDim2.new(0.5, -290, 0.5, -210)
end)

closeBtn.MouseButton1Click:Connect(function()
    screenGui:Destroy()
end)

-- ==================== API ====================
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

    local iconLabel = Instance.new("TextLabel")
    iconLabel.Size = UDim2.new(1, 0, 0.55, 0)
    iconLabel.BackgroundTransparency = 1
    iconLabel.Text = icon
    iconLabel.TextScaled = true
    iconLabel.Font = Enum.Font.GothamBold
    iconLabel.TextColor3 = Color3.fromRGB(255, 200, 100)
    iconLabel.Parent = tabBtn

    local nameLabel = Instance.new("TextLabel")
    nameLabel.Size = UDim2.new(1, 0, 0.45, 0)
    nameLabel.Position = UDim2.new(0, 0, 0.55, 0)
    nameLabel.BackgroundTransparency = 1
    nameLabel.Text = name
    nameLabel.TextScaled = true
    nameLabel.Font = Enum.Font.GothamSemibold
    nameLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
    nameLabel.Parent = tabBtn

    local tabContent = Instance.new("ScrollingFrame")
    tabContent.Size = UDim2.new(1, 0, 1, 0)
    tabContent.BackgroundTransparency = 1
    tabContent.ScrollBarThickness = 5
    tabContent.Visible = false
    tabContent.Parent = content

    local layout = Instance.new("UIListLayout")
    layout.SortOrder = Enum.SortOrder.LayoutOrder
    layout.Padding = UDim.new(0, 10)
    layout.Parent = tabContent

    layout:GetPropertyChangedSignal("AbsoluteContentSize"):Connect(function()
        tabContent.CanvasSize = UDim2.new(0, 0, 0, layout.AbsoluteContentSize.Y + 30)
    end)

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

    sidebar.CanvasSize = UDim2.new(0, 0, 0, sidebarLayout.AbsoluteContentSize.Y + 20)

    return {
        CreateButton = function(self, btnConfig)
            local b = Instance.new("TextButton")
            b.Size = UDim2.new(1, 0, 0, 46)
            b.BackgroundColor3 = Color3.fromRGB(55, 55, 55)
            b.Text = btnConfig.Name
            b.TextColor3 = Color3.new(1,1,1)
            b.TextScaled = true
            b.Font = Enum.Font.GothamSemibold
            b.Parent = tabContent
            Instance.new("UICorner", b).CornerRadius = UDim.new(0, 8)
            b.MouseButton1Click:Connect(btnConfig.Callback or function() end)
        end
    }
end

return Library
