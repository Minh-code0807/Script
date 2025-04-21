local player = game.Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local humanoid = character:WaitForChild("Humanoid")
local sitting = false
local teleportDistance = 10000

-- Hàm để cập nhật nhân vật khi nhân vật thay đổi (respawn)
local function updateCharacter(newCharacter)
    character = newCharacter
    humanoid = character:WaitForChild("Humanoid")
end

player.CharacterAdded:Connect(updateCharacter) -- Lắng nghe sự kiện nhân vật thay đổi

local function teleportCharacter()
    if not character.PrimaryPart then
        warn("Character does not have a PrimaryPart.")
        return
    end

    local currentPosition = character.PrimaryPart.Position
    local newPosition = currentPosition + Vector3.new(teleportDistance, 0, 0)

    -- Đảm bảo PrimaryPart tồn tại trước khi thay đổi vị trí
    character:SetPrimaryPartCFrame(CFrame.new(newPosition))
    humanoid.Sit = true
    sitting = true
end

player.Chatted:Connect(function(message)
    if message:lower() == "tp" then
        teleportCharacter()
    end
end)

game:GetService("UserInputService").InputBegan:Connect(function(input, gameProcessed)
    if gameProcessed then
        return -- Bỏ qua nếu hành động đã được xử lý bởi Roblox
    end

    if sitting and (input.KeyCode == Enum.KeyCode.T or input.UserInputType == Enum.UserInputType.Jump) then
        humanoid.Sit = false
        sitting = false
    end
end)
