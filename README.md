# Sigma-Gui-by-ermys714
Just a beginner script 
-- Sigma GUI Final Script (Scroll + Noclip + ESP + View Kamera + Calculator)
local player = game.Players.LocalPlayer
local RunService = game:GetService("RunService")

-- Buat ScreenGui utama
local gui = Instance.new("ScreenGui")
gui.Name = "sigma"
gui.ResetOnSpawn = false
gui.Parent = player:WaitForChild("PlayerGui")

-- Frame utama
local frame = Instance.new("Frame")
frame.Size = UDim2.new(0, 320, 0, 300)
frame.Position = UDim2.new(0.5, -160, 0.5, -150)
frame.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
frame.Active = true
frame.Draggable = true
frame.Parent = gui

local corner = Instance.new("UICorner")
corner.CornerRadius = UDim.new(0, 20)
corner.Parent = frame

local stroke = Instance.new("UIStroke")
stroke.Thickness = 4
stroke.Color = Color3.fromRGB(255, 255, 255)
stroke.Parent = frame

-- TopBar
local topbar = Instance.new("Frame")
topbar.Size = UDim2.new(1, 0, 0, 40)
topbar.BackgroundColor3 = Color3.fromRGB(0, 120, 255)
topbar.Parent = frame

local title = Instance.new("TextLabel")
title.Size = UDim2.new(1, 0, 1, 0)
title.BackgroundTransparency = 1
title.Text = "sigma universal"
title.TextColor3 = Color3.fromRGB(255, 255, 255)
title.Font = Enum.Font.GothamBold
title.TextScaled = true
title.TextStrokeTransparency = 0
title.TextStrokeColor3 = Color3.fromRGB(0, 0, 0)
title.Parent = topbar

-- ScrollingFrame
local scroll = Instance.new("ScrollingFrame")
scroll.Size = UDim2.new(1, -10, 1, -50)
scroll.Position = UDim2.new(0, 5, 0, 45)
scroll.CanvasSize = UDim2.new(0, 0, 0, 700)
scroll.ScrollBarThickness = 8
scroll.BackgroundTransparency = 1
scroll.Parent = frame

-- Tombol Noclip
local noclipButton = Instance.new("TextButton")
noclipButton.Size = UDim2.new(1, -20, 0, 40)
noclipButton.Position = UDim2.new(0, 10, 0, 10)
noclipButton.BackgroundColor3 = Color3.fromRGB(100, 100, 100)
noclipButton.Text = "Noclip: OFF"
noclipButton.TextColor3 = Color3.fromRGB(255, 255, 255)
noclipButton.Font = Enum.Font.GothamBold
noclipButton.TextScaled = true
noclipButton.TextStrokeTransparency = 0
noclipButton.TextStrokeColor3 = Color3.fromRGB(0, 0, 0)
noclipButton.Parent = scroll

local noclipEnabled = false
noclipButton.MouseButton1Click:Connect(function()
    noclipEnabled = not noclipEnabled
    noclipButton.Text = noclipEnabled and "Noclip: ON" or "Noclip: OFF"
end)

RunService.Stepped:Connect(function()
    if noclipEnabled and player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
        for _, part in pairs(player.Character:GetDescendants()) do
            if part:IsA("BasePart") then
                part.CanCollide = false
            end
        end
    end
end)

-- Tombol ESP Global
local espButton = noclipButton:Clone()
espButton.Position = UDim2.new(0, 10, 0, 60)
espButton.Text = "ESP: OFF"
espButton.Parent = scroll

local espEnabled = false
espButton.MouseButton1Click:Connect(function()
    espEnabled = not espEnabled
    espButton.Text = espEnabled and "ESP: ON" or "ESP: OFF"
end)

RunService.RenderStepped:Connect(function()
    if espEnabled then
        for _, plr in pairs(game.Players:GetPlayers()) do
            if plr ~= player and plr.Character and plr.Character:FindFirstChild("HumanoidRootPart") then
                local char = plr.Character
                if not char:FindFirstChild("SigmaESP") then
                    local highlight = Instance.new("Highlight")
                    highlight.Name = "SigmaESP"
                    highlight.FillColor = Color3.fromRGB(0, 255, 0)
                    highlight.OutlineColor = Color3.fromRGB(255, 255, 255)
                    highlight.Adornee = char
                    highlight.Parent = char
                end
                if not char:FindFirstChild("SigmaName") then
                    local billboard = Instance.new("BillboardGui")
                    billboard.Name = "SigmaName"
                    billboard.Size = UDim2.new(0, 120, 0, 20)
                    billboard.Adornee = char:FindFirstChild("Head")
                    billboard.AlwaysOnTop = true
                    billboard.StudsOffset = Vector3.new(0, 3, 0)
                    billboard.Parent = char

                    local nameLabel = Instance.new("TextLabel")
                    nameLabel.Size = UDim2.new(1, 0, 1, 0)
                    nameLabel.BackgroundTransparency = 1
                    nameLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
                    nameLabel.Font = Enum.Font.GothamBold
                    nameLabel.TextSize = 14
                    nameLabel.TextStrokeTransparency = 0
                    nameLabel.TextStrokeColor3 = Color3.fromRGB(0, 0, 0)
                    nameLabel.Parent = billboard

                    RunService.RenderStepped:Connect(function()
                        if espEnabled and char:FindFirstChild("HumanoidRootPart") and player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
                            local dist = (char.HumanoidRootPart.Position - player.Character.HumanoidRootPart.Position).Magnitude
                            nameLabel.Text = plr.Name .. " [" .. math.floor(dist) .. " studs]"
                        end
                    end)
                end
            end
        end
    else
        for _, plr in pairs(game.Players:GetPlayers()) do
            if plr.Character then
                if plr.Character:FindFirstChild("SigmaName") then plr.Character.SigmaName:Destroy() end
                if plr.Character:FindFirstChild("SigmaESP") then plr.Character.SigmaESP:Destroy() end
            end
        end
    end
end)

-- TextBox untuk username target
local userBox = Instance.new("TextBox")
userBox.Size = UDim2.new(1, -20, 0, 35)
userBox.Position = UDim2.new(0, 10, 0, 110)
userBox.BackgroundColor3 = Color3.fromRGB(200, 200, 200)
userBox.Text = ""
userBox.PlaceholderText = "Masukkan username..."
userBox.TextColor3 = Color3.fromRGB(0, 0, 0)
userBox.Font = Enum.Font.GothamBold
userBox.TextScaled = true
userBox.Parent = scroll

-- Tombol View/Unview Kamera
local viewButton = noclipButton:Clone()
viewButton.Position = UDim2.new(0, 10, 0, 160)
viewButton.Text = "View: OFF"
viewButton.Parent = scroll

local viewEnabled = false
local targetPlayer = nil

local function findClosestUsername(input)
    local bestMatch, bestScore = nil, math.huge
    for _, plr in pairs(game.Players:GetPlayers()) do
        if string.find(string.lower(plr.Name), string.lower(input)) then
            local dist = math.abs(#plr.Name - #input)
            if dist < bestScore then
                bestScore = dist
                bestMatch = plr
            end
        end
    end
    return bestMatch
end

viewButton.MouseButton1Click:Connect(function()
    if viewEnabled then
        viewEnabled = false
        viewButton.Text = "View: OFF"
        workspace.CurrentCamera.CameraSubject = player.Character:FindFirstChild("Humanoid")
        targetPlayer = nil
    else
        local match = findClosestUsername(userBox.Text)
        if match and match.Character and match.Character:FindFirstChild("Humanoid") then
            viewEnabled = true
            viewButton.Text = "View: ON"
            targetPlayer = match
            workspace.CurrentCamera.CameraSubject = match.Character:FindFirstChild("Humanoid")
        end
    end
end)

-- Tombol Calculator
local calcButton = noclipButton:Clone()
calcButton.Position = UDim2.new(0, 10, 0, 210)
calcButton.Text = "Calculator"
calcButton.Parent = scroll

-- GUI Kalkulator
local calcGui = Instance.new("Frame")
calcGui.Size = UDim2.new(0, 250, 0, 300)
calcGui.Position = UDim2.new(0.5, -125, 0.5, -150)
calcGui.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
calcGui.Visible = false
calcGui.Parent = gui

local calcCorner = Instance.new("UICorner")
calcCorner.CornerRadius = UDim.new(0, 15)
calcCorner.Parent = calcGui

local calcTitle = Instance.new("TextLabel")
calcTitle.Size = UDim2.new(1, 0, 0, 40)
calcTitle.BackgroundColor3 = Color3.fromRGB(0, 120, 255)
calcTitle.Text = "Calculator"
calcTitle.TextColor3 = Color3.fromRGB(255, 255, 255)
calcTitle.Font = Enum.Font.GothamBold
calcTitle.TextScaled = true
calcTitle.Parent = calcGui

local inputBox = Instance.new("TextBox")
inputBox.Size = UDim2.new(1, -20, 0, 40)
inputBox.Position = UDim2.new(0, 10, 0, 60)
inputBox.BackgroundColor3 = Color3.fromRGB(200, 200, 200)
inputBox.TextColor3 = Color3.fromRGB(0, 0, 0)
inputBox.PlaceholderText = "Masukkan ekspresi (contoh: 2+3*5)"
inputBox.Font = Enum.Font.Gotham
inputBox.TextScaled = true
inputBox.Parent = calcGui

local resultLabel = Instance.new("TextLabel")
resultLabel.Size = UDim2.new(1, -20, 0, 40)
resultLabel.Position = UDim2.new(0, 10, 0, 110)
resultLabel.BackgroundColor3 = Color3.fromRGB(80, 80, 80)
resultLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
resultLabel.Text = "Hasil: "
resultLabel.Font = Enum.Font.GothamBold
resultLabel.TextScaled = true
resultLabel.Parent = calcGui

local evalButton = Instance.new("TextButton")
evalButton.Size = UDim2.new(1, -20, 0, 40)
evalButton.Position = UDim2.new(0, 10, 0, 160)
evalButton.BackgroundColor3 = Color3.fromRGB(100, 100, 100)
evalButton.Text = "Hitung"
evalButton.TextColor3 = Color3.fromRGB(255, 255, 255)
evalButton.Font = Enum.Font.GothamBold
evalButton.TextScaled = true
evalButton.Parent = calcGui

-- Kalkulator logic
evalButton.MouseButton1Click:Connect(function()
    local expr = inputBox.Text
    local ok, result = pcall(function()
        return loadstring("return " .. expr)()
    end)
    if ok and result ~= nil then
        resultLabel.Text = "Hasil: " .. tostring(result)
    else
        resultLabel.Text = "Error!"
    end
end)

-- Toggle kalkulator GUI
calcButton.MouseButton1Click:Connect(function()
    calcGui.Visible = not calcGui.Visible
end)-- Tombol Teleport
local tpButton = Instance.new("TextButton")
tpButton.Size = UDim2.new(1, -20, 0, 40)
tpButton.Position = UDim2.new(0, 10, 0, 250) -- di bawah tombol View
tpButton.BackgroundColor3 = Color3.fromRGB(100, 100, 100)
tpButton.Text = "Teleport"
tpButton.TextColor3 = Color3.fromRGB(255, 255, 255)
tpButton.Font = Enum.Font.GothamBold
tpButton.TextScaled = true
tpButton.TextStrokeTransparency = 0
tpButton.TextStrokeColor3 = Color3.fromRGB(0, 0, 0)
tpButton.Parent = scroll

-- Fungsi Teleport
tpButton.MouseButton1Click:Connect(function()
    local match = findClosestUsername(userBox.Text)
    if match and match.Character and match.Character:FindFirstChild("HumanoidRootPart") then
        local targetPos = match.Character.HumanoidRootPart.Position
        if player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
            player.Character.HumanoidRootPart.CFrame = CFrame.new(targetPos + Vector3.new(0, 5, 0))
        end
    end
end)-- Tombol AntiKick
local antikickButton = Instance.new("TextButton")
antikickButton.Size = UDim2.new(1, -20, 0, 40)
antikickButton.Position = UDim2.new(0, 10, 0, 300) -- letakkan di bawah tombol Teleport
antikickButton.BackgroundColor3 = Color3.fromRGB(100, 100, 100)
antikickButton.Text = "AntiKick: OFF"
antikickButton.TextColor3 = Color3.fromRGB(255, 255, 255)
antikickButton.Font = Enum.Font.GothamBold
antikickButton.TextScaled = true
antikickButton.TextStrokeTransparency = 0
antikickButton.TextStrokeColor3 = Color3.fromRGB(0, 0, 0)
antikickButton.Parent = scroll

local antikickEnabled = false
local oldKick = player.Kick -- simpan fungsi asli

antikickButton.MouseButton1Click:Connect(function()
    antikickEnabled = not antikickEnabled
    antikickButton.Text = antikickEnabled and "AntiKick: ON" or "AntiKick: OFF"

    if antikickEnabled then
        -- Override fungsi Kick
        player.Kick = function() 
            warn("Kick attempt blocked by AntiKick!") 
        end
    else
        -- Kembalikan fungsi Kick asli
        player.Kick = oldKick
    end
end)
