-- MINI HUB LIBRARY - API Melhorada
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

local content = Instance.new("ScrollingFrame")
content.Size = UDim2.new(1, -70, 1, 0)
content.Position = UDim2.new(0, 70, 0, 0)
content.BackgroundColor3 = Color3.fromRGB(26, 26, 26)
content.ScrollBarThickness = 5
content.Parent = container

-- (Mantive o drag, bolinha, etc do código anterior - se precisar eu reposto completo)

local tabs = {}

function Library:CreateWindow(config)
    titleLabel.Text = config.Title or "MINI HUB"
    return Library
end

function Library:CreateTab(config)
    local name = config.Name or "Tab"

    local tabBtn = Instance.new("TextButton")
    tabBtn.Size = UDim2.new(1, -10, 0, 52)
    tabBtn.Position = UDim2.new(0, 5, 0, #tabs * 62)
    tabBtn.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
    tabBtn.Text = name
    tabBtn.TextColor3 = Color3.fromRGB(255, 220, 120)
    tabBtn.TextScaled = true
    tabBtn.Parent = sidebar
    Instance.new("UICorner", tabBtn).CornerRadius = UDim.new(0, 8)

    local tabContent = Instance.new("ScrollingFrame")
    tabContent.Size = UDim2.new(1, 0, 1, 0)
    tabContent.BackgroundTransparency = 1
    tabContent.ScrollBarThickness = 5
    tabContent.Visible = false
    tabContent.Parent = content
    Instance.new("UIListLayout", tabContent).Padding = UDim.new(0, 10)

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
        CreateButton = function(self, btnConfig)
            local b = Instance.new("TextButton")
            b.Size = UDim2.new(1, -20, 0, 46)
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

-- Drag, bolinha e resto do código (mantém o que já tinha)

return Library
