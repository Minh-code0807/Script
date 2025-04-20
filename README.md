local ScreenGui = Instance.new("ScreenGui")
local TeleportButton = Instance.new("TextButton")

ScreenGui.Parent = game.Players.LocalPlayer:WaitForChild("PlayerGui")
ScreenGui.Name = "TeleportGui"

TeleportButton.Parent = ScreenGui
TeleportButton.Text = "Tp đến 10000m"
TeleportButton.BackgroundColor3 = Color3.fromRGB(0, 170, 255)
TeleportButton.TextColor3 = Color3.fromRGB(255, 255, 255)
TeleportButton.Font = Enum.Font.SourceSans
TeleportButton.TextSize = 20
TeleportButton.Size = UDim2.new(0, 150, 0, 50)
TeleportButton.Position = UDim2.new(0, 20, 0, 100)
local AnchorPoint = Vector2.new(0, 0)

TeleportButton.MouseButton1Click:Connect(function()
    local player = game.Players.LocalPlayer
    if player and player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
        local humanoidRootPart = player.Character.HumanoidRootPart
        local direction = humanoidRootPart.CFrame.LookVector
        humanoidRootPart.CFrame = humanoidRootPart.CFrame + (direction * 10000)
        print("Đã dịch chuyển khoảng cách 10,000 mét tới phía trước!")
    else
        print("Không thể thực hiện teleport vì nhân vật chưa sẵn sàng!")
    end
end)
