local player = game.Players.LocalPlayer
local gui = Instance.new("ScreenGui")
gui.Name = "MinhHubDraggableGui"
gui.Parent = player:WaitForChild("PlayerGui")

local frame = Instance.new("Frame")
frame.Size = UDim2.new(0, 420, 0, 220)
frame.Position = UDim2.new(0.5, -210, 0.3, 0)
frame.BackgroundColor3 = Color3.fromRGB(33, 35, 42)
frame.BorderSizePixel = 0
frame.Active = true
frame.Draggable = true
frame.Parent = gui

local title = Instance.new("TextLabel")
title.Size = UDim2.new(1, 0, 0, 48)
title.Position = UDim2.new(0, 0, 0, 0)
title.BackgroundTransparency = 1
title.Font = Enum.Font.GothamBold
title.Text = "by Minh Hub"
title.TextSize = 34
title.TextColor3 = Color3.fromRGB(255, 255, 255)
title.Parent = frame

local buttonTable = Instance.new("Frame")
buttonTable.Size = UDim2.new(1, -48, 0, 100)
buttonTable.Position = UDim2.new(0, 24, 0, 75)
buttonTable.BackgroundTransparency = 1
buttonTable.Parent = frame

local uiList = Instance.new("UIListLayout")
uiList.FillDirection = Enum.FillDirection.Horizontal
uiList.HorizontalAlignment = Enum.HorizontalAlignment.Center
uiList.VerticalAlignment = Enum.VerticalAlignment.Center
uiList.Padding = UDim.new(0, 28)
uiList.Parent = buttonTable

-- TIMER BEGIN
local timerLabel = Instance.new("TextLabel")
timerLabel.Size = UDim2.new(0, 150, 0, 40)
timerLabel.Position = UDim2.new(0, 14, 1, -50)
timerLabel.BackgroundTransparency = 0.3
timerLabel.BackgroundColor3 = Color3.fromRGB(50, 50, 60)
timerLabel.BorderSizePixel = 0
timerLabel.Font = Enum.Font.GothamBold
timerLabel.TextSize = 28
timerLabel.TextColor3 = Color3.fromRGB(255,255,255)
timerLabel.TextXAlignment = Enum.TextXAlignment.Left
timerLabel.Text = "10:00"
timerLabel.Parent = frame

local seconds = 10 * 60
spawn(function()
	while seconds >= 0 do
		local mins = math.floor(seconds / 60)
		local secs = seconds % 60
		timerLabel.Text = string.format("%02d:%02d", mins, secs)
		wait(1)
		seconds = seconds - 1
	end
	timerLabel.Text = "00:00"
end)
-- TIMER END

-- Thu nhỏ / Phóng to GUI
local minimized = false

local minimizeBtn = Instance.new("TextButton")
minimizeBtn.Size = UDim2.new(0, 40, 0, 40)
minimizeBtn.Position = UDim2.new(1, -50, 0, 8)
minimizeBtn.BackgroundColor3 = Color3.fromRGB(60, 60, 90)
minimizeBtn.Text = "-"
minimizeBtn.TextColor3 = Color3.fromRGB(255,255,255)
minimizeBtn.Font = Enum.Font.GothamBold
minimizeBtn.TextSize = 30
minimizeBtn.BorderSizePixel = 0
minimizeBtn.Parent = frame

local smallBtn = Instance.new("TextButton")
smallBtn.Size = UDim2.new(0, 40, 0, 40)
smallBtn.Position = UDim2.new(0.5, -20, 0, 20)
smallBtn.AnchorPoint = Vector2.new(0.5, 0)
smallBtn.BackgroundColor3 = Color3.fromRGB(60, 60, 90)
smallBtn.Text = "+"
smallBtn.TextColor3 = Color3.fromRGB(255,255,255)
smallBtn.Font = Enum.Font.GothamBold
smallBtn.TextSize = 30
smallBtn.BorderSizePixel = 0
smallBtn.Visible = false
smallBtn.Active = true
smallBtn.Draggable = true
smallBtn.Parent = gui

minimizeBtn.MouseButton1Click:Connect(function()
	frame.Visible = false
	smallBtn.Visible = true
end)

smallBtn.MouseButton1Click:Connect(function()
	frame.Visible = true
	smallBtn.Visible = false
end)

local function createButton(name, callback)
	local btn = Instance.new("TextButton")
	btn.Size = UDim2.new(0, 96, 0, 50)
	btn.BackgroundColor3 = Color3.fromRGB(114, 137, 218)
	btn.Text = name
	btn.TextColor3 = Color3.fromRGB(255,255,255)
	btn.Font = Enum.Font.GothamBold
	btn.TextSize = 24
	btn.BorderSizePixel = 0
	btn.AutoButtonColor = true
	btn.Parent = buttonTable
	btn.MouseButton1Click:Connect(callback)
end

-- Nút Tp end - cập nhật mới
createButton("Tp end", function()
	local targetCFrame = CFrame.new(-428.75, 23, -49040.91)
	local char = player.Character or player.CharacterAdded:Wait()
	local hrp = char:WaitForChild("HumanoidRootPart")
	local humanoid = char:FindFirstChildWhichIsA("Humanoid")
	
	if not hrp or not humanoid then return end

	local locked = true
	local success = false

	humanoid.WalkSpeed = 0
	humanoid.JumpPower = 0
	humanoid.AutoRotate = false

	local blurGui = Instance.new("Frame")
	blurGui.Size = UDim2.new(1, 0, 1, 0)
	blurGui.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
	blurGui.BackgroundTransparency = 0.5
	blurGui.BorderSizePixel = 0
	blurGui.ZIndex = 10
	blurGui.Parent = gui

	local popup = Instance.new("TextLabel")
	popup.Size = UDim2.new(0, 300, 0, 60)
	popup.Position = UDim2.new(0.5, -150, 0.5, -30)
	popup.BackgroundTransparency = 1
	popup.Text = "Đang dịch chuyển..."
	popup.Font = Enum.Font.GothamBold
	popup.TextSize = 28
	popup.TextColor3 = Color3.fromRGB(255, 255, 255)
	popup.ZIndex = 11
	popup.Parent = blurGui

	local userInput = game:GetService("UserInputService")
	local jumpConn
	jumpConn = userInput.InputBegan:Connect(function(input, gameProcessed)
		if not gameProcessed and input.KeyCode == Enum.KeyCode.Space then
			locked = false
			game.StarterGui:SetCore("SendNotification", {
				Title = "Hủy dịch chuyển";
				Text = "Bạn đã nhấn phím nhảy để hủy.";
				Duration = 2;
			})
		end
	end)

	spawn(function()
		while locked and not success do
			if (hrp.Position - targetCFrame.Position).Magnitude > 5 then
				hrp.CFrame = targetCFrame
			else
				success = true
			end
			wait(0.25)
		end

		humanoid.WalkSpeed = 16
		humanoid.JumpPower = 50
		humanoid.AutoRotate = true
		if jumpConn then jumpConn:Disconnect() end
		if blurGui then blurGui:Destroy() end

		if success then
			game.StarterGui:SetCore("SendNotification", {
				Title = "Tp end";
				Text = "Dịch chuyển thành công!";
				Duration = 2;
			})
		else
			game.StarterGui:SetCore("SendNotification", {
				Title = "Tp end";
				Text = "Dịch chuyển bị hủy.";
				Duration = 2;
			})
		end
	end)
end)

-- Các nút còn lại giữ nguyên (NPC Lock, noclip...)

-- (Giữ nguyên NPC Lock và noclip như bạn có trước đó)

-- Auto teleport if Y < 2.5 (teleport to Y = 5)
spawn(function()
	while true do
		local char = player.Character
		if char then
			local hrp = char:FindFirstChild("HumanoidRootPart")
			if hrp and hrp.Position.Y < 2.5 then
				hrp.CFrame = CFrame.new(hrp.Position.X, 5, hrp.Position.Z)
			end
		end
		wait(0.15)
	end
end)
