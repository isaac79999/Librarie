
-- ==================== BOLINHA MOBILE ====================
local player = game.Players.LocalPlayer
local gui = gethui() or player:WaitForChild("PlayerGui")

local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "SkeetMobileToggle"
ScreenGui.ResetOnSpawn = false
ScreenGui.Parent = gui

local Button = Instance.new("TextButton")
Button.Size = UDim2.new(0, 50, 0, 50)
Button.Position = UDim2.new(0.9, 0, 0.5, 0)
Button.BackgroundColor3 = Color3.fromRGB(255, 0, 100)
Button.Text = "S"
Button.TextColor3 = Color3.new(1,1,1)
Button.TextScaled = true
Button.Font = Enum.Font.GothamBold
Button.BorderSizePixel = 0
Button.Parent = ScreenGui

local UICorner = Instance.new("UICorner")
UICorner.CornerRadius = UDim.new(1, 0)
UICorner.Parent = Button

-- Draggable
local dragging = false
local dragInput
local dragStart
local startPos

local function update(input)
    local delta = input.Position - dragStart
    Button.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
end

Button.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
        dragging = true
        dragStart = input.Position
        startPos = Button.Position
        
        input.Changed:Connect(function()
            if input.UserInputState == Enum.UserInputState.End then
                dragging = false
            end
        end)
    end
end)

Button.InputChanged:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch then
        dragInput = input
    end
end)

game:GetService("UserInputService").InputChanged:Connect(function(input)
    if dragging and input == dragInput then
        update(input)
    end
end)

local VIM = game:GetService("VirtualInputManager")

Button.MouseButton1Click:Connect(function()
    VIM:SendKeyEvent(true, Enum.KeyCode.K, false, game)
    task.wait(0.05)
    VIM:SendKeyEvent(false, Enum.KeyCode.K, false, game)
end)
Library:Notify({Description = "Bolinha mobile ativada! Arraste ela e clica pra toggle.", Duration = 5})
