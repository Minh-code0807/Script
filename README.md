-- Nút Tp end - có khóa và thử lại liên tục
createButton("Tp end", function()
	local targetCFrame = CFrame.new(-428.75, 23, -49040.91)
	local char = player.Character or player.CharacterAdded:Wait()
	local hrp = char:WaitForChild("HumanoidRootPart")
	local humanoid = char:FindFirstChildWhichIsA("Humanoid")
	if not hrp or not humanoid then return end

	local locked, success = true, false

	-- Khóa điều khiển
	humanoid.WalkSpeed = 0
	humanoid.JumpPower = 0
	humanoid.AutoRotate = false

	-- Giao diện mờ
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

		local msg = success and "Dịch chuyển thành công!" or "Dịch chuyển bị hủy."
		game.StarterGui:SetCore("SendNotification", {
			Title = "Tp end";
			Text = msg;
			Duration = 2;
		})
	end)
end)

-- Nút NPC Lock
local oldMinZoom, oldMaxZoom = nil, nil
local npcLockEnabled = false
createButton("NPC Lock", function()
	npcLockEnabled = not npcLockEnabled
	if npcLockEnabled then
		oldMinZoom = player.CameraMinZoomDistance
		oldMaxZoom = player.CameraMaxZoomDistance
		player.CameraMinZoomDistance = 0.5
		player.CameraMaxZoomDistance = 50
		loadstring(game:HttpGet("https://raw.githubusercontent.com/huybuda1/Anh-Con-PHD-Troll/refs/heads/main/AnhCon_TheNao_LaiManh.lua"))()
		game.StarterGui:SetCore("SendNotification", {
			Title = "NPC Lock";
			Text = "Đã bật NPC Lock và tự do zoom camera!";
			Duration = 2;
		})
	else
		if oldMinZoom and oldMaxZoom then
			player.CameraMinZoomDistance = oldMinZoom
			player.CameraMaxZoomDistance = oldMaxZoom
		end
		game.StarterGui:SetCore("SendNotification", {
			Title = "NPC Lock";
			Text = "Đã tắt NPC Lock và trả về zoom mặc định!";
			Duration = 2;
		})
	end
end)

-- Nút noclip
local noclipEnabled = false
local noclipConnection = nil
createButton("noclip", function()
	noclipEnabled = not noclipEnabled
	local function Noclip()
		local char = player.Character
		if char then
			for _, part in pairs(char:GetDescendants()) do
				if part:IsA("BasePart") and part.CanCollide == true then
					part.CanCollide = false
				end
			end
		end
	end
	if noclipEnabled then
		noclipConnection = game:GetService("RunService").Stepped:Connect(Noclip)
		game.StarterGui:SetCore("SendNotification", {
			Title = "noclip";
			Text = "Đã bật xuyên tường!";
			Duration = 2;
		})
	else
		if noclipConnection then
			noclipConnection:Disconnect()
			noclipConnection = nil
		end
		local char = player.Character
		if char then
			for _, part in pairs(char:GetDescendants()) do
				if part:IsA("BasePart") then
					part.CanCollide = true
				end
			end
		end
		game.StarterGui:SetCore("SendNotification", {
			Title = "noclip";
			Text = "Đã tắt xuyên tường!";
			Duration = 2;
		})
	end
end)
