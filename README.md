local Players = game:GetService("Players")
local player = Players.LocalPlayer
local isScriptActivated = false

local function setNoclip(enabled)
    local character = player.Character
    if not character then return end
    for _, part in ipairs(character:GetDescendants()) do
        if part:IsA("BasePart") and part.CanCollide ~= nil then
            part.CanCollide = not enabled
        end
    end
end

local function teleportForward(distance)
    local character = player.Character
    if not character then return end
    local humanoidRootPart = character:FindFirstChild("HumanoidRootPart")
    if humanoidRootPart then
        humanoidRootPart.CFrame = humanoidRootPart.CFrame * CFrame.new(0, 0, -distance)
    end
end

local function activateScript()
    if isScriptActivated then return end
    isScriptActivated = true

    local ScreenGui = Instance.new("ScreenGui")
    local Frame = Instance.new("Frame")
    local EmptyFrame = Instance.new("Frame")
    local ButtonsFrame = Instance.new("Frame")
    local TeleportButton = Instance.new("TextButton")

    ScreenGui.Parent = player:WaitForChild("PlayerGui")
    ScreenGui.Name = "TeleportAndNoclipGui"

    Frame.Parent = ScreenGui
    Frame.Size = UDim2.new(0, 400, 0, 200)
    Frame.Position = UDim2.new(0.5, -200, 0.5, -100)
    Frame.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
    Frame.BackgroundTransparency = 0.2
    Frame.Active = true
    Frame.Draggable = true

    EmptyFrame.Parent = Frame
    EmptyFrame.Size = UDim2.new(0.5, 0, 1, 0)
    EmptyFrame.Position = UDim2.new(0, 0, 0, 0)
    EmptyFrame.BackgroundColor3 = Color3.fromRGB(70, 70, 70)
    EmptyFrame.BackgroundTransparency = 0.5

    ButtonsFrame.Parent = Frame
    ButtonsFrame.Size = UDim2.new(0.5, 0, 1, 0)
    ButtonsFrame.Position = UDim2.new(0.5, 0, 0, 0)
    ButtonsFrame.BackgroundColor3 = Color3.fromRGB(100, 100, 100)
    ButtonsFrame.BackgroundTransparency = 0.3

    TeleportButton.Parent = ButtonsFrame
    TeleportButton.Size = UDim2.new(0.8, 0, 0.2, 0)
    TeleportButton.Position = UDim2.new(0.1, 0, 0.4, 0)
    TeleportButton.Text = "TP +10k & Noclip"
    TeleportButton.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
    TeleportButton.TextColor3 = Color3.fromRGB(0, 0, 0)
    TeleportButton.Font = Enum.Font.SourceSans
    TeleportButton.TextScaled = true

    TeleportButton.MouseButton1Click:Connect(function()
        setNoclip(true)
        task.wait(0.2)
        teleportForward(10000)
        task.wait(0.2)
        setNoclip(false)
    end)
end

Players.PlayerChatted:Connect(function(playerWhoChatted, message)
    if string.lower(message) == "all" then
        if playerWhoChatted == player then
            activateScript()
        end
    end
end)
