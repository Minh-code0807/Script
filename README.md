-- Tạo ScreenGUI
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "ButtonGUI"
screenGui.Parent = game.Players.LocalPlayer:WaitForChild("PlayerGui")

-- Hàm tạo nút
local function createButton(name, text, position, parent)
    local button = Instance.new("TextButton")
    button.Name = name
    button.Text = text
    button.Size = UDim2.new(0, 150, 0, 50) -- Kích thước: 150x50
    button.Position = position -- Vị trí trên màn hình
    button.BackgroundColor3 = Color3.fromRGB(0, 0, 0) -- Màu nền đen
    button.TextColor3 = Color3.fromRGB(255, 255, 255) -- Chữ trắng
    button.Font = Enum.Font.SourceSans
    button.TextSize = 20
    button.Parent = parent
    return button
end

-- Tạo các nút
local button1 = createButton("Button1", "TP Sterling Town", UDim2.new(0, 20, 0, 20), screenGui)
local button2 = createButton("Button2", "Tesla TP", UDim2.new(0, 200, 0, 20), screenGui)
local button3 = createButton("Button3", "Castle TP", UDim2.new(0, 380, 0, 20), screenGui)
local button4 = createButton("Button4", "Fort TP", UDim2.new(0, 560, 0, 20), screenGui)
local button5 = createButton("Button5", "Fort ConstitutionConstitution", UDim2.new(0, 740, 0, 20), screenGui)
local minimizeButton = createButton("MinimizeButton", "Minimize", UDim2.new(0, 920, 0, 20), screenGui)

-- Gắn chức năng cho từng nút
button1.MouseButton1Click:Connect(function()
    print("Executing TP Sterling Town script...")
    local success, err = pcall(function()
        loadstring(game:HttpGet('https://raw.githubusercontent.com/ringtaa/sterlingnotifcation.github.io/refs/heads/main/Sterling.lua'))()
    end)
    if not success then
        warn("Error executing TP Sterling Town script: " .. err)
    end
end)

button2.MouseButton1Click:Connect(function()
    print("Executing Tesla TP script...")
    local success, err = pcall(function()
        loadstring(game:HttpGet('https://raw.githubusercontent.com/ringtaa/tptotesla.github.io/refs/heads/main/Tptotesla.lua'))()
    end)
    if not success then
        warn("Error executing Tesla TP script: " .. err)
    end
end)

button3.MouseButton1Click:Connect(function()
    print("Executing Castle TP script...")
    local success, err = pcall(function()
        loadstring(game:HttpGet("https://raw.githubusercontent.com/ringtaa/castletpfast.github.io/refs/heads/main/FASTCASTLE.lua"))()
    end)
    if not success then
        warn("Error executing Castle TP script: " .. err)
    end
end)

button4.MouseButton1Click:Connect(function()
    print("Executing Fort TP script...")
    local success, err = pcall(function()
        loadstring(game:HttpGet("https://raw.githubusercontent.com/ringtaa/Tpfort.github.io/refs/heads/main/Tpfort.lua"))()
    end)
    if not success then
        warn("Error executing Fort TP script: " .. err)
    end
end)

button5.MouseButton1Click:Connect(function()
    print("Executing Fort ConstitutionConstitution script...")

