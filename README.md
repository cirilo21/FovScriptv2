local Players = game:GetService("Players")
local player = Players.LocalPlayer
local camera = workspace.CurrentCamera
local UIS = game:GetService("UserInputService")

local minFOV = 10
local maxFOV = 120
local currentFOV = 70
camera.FieldOfView = currentFOV

local gui = script.Parent

-- 🔘 Botão flutuante (abrir menu)
local openButton = Instance.new("TextButton")
openButton.Size = UDim2.new(0, 35, 0, 35)
openButton.Position = UDim2.new(0, 20, 0, 200)
openButton.BackgroundColor3 = Color3.fromRGB(44, 42, 42)
openButton.Text = "+"
openButton.TextSize = 28
openButton.TextColor3 = Color3.fromRGB(255, 255, 255)
openButton.Font = Enum.Font.SourceSansBold
openButton.Name = "OpenMenuButton"
openButton.Parent = gui
openButton.Active = true
openButton.Draggable = true

-- 🪟 Menu principal (inicialmente invisível)
local frame = Instance.new("Frame")
frame.Size = UDim2.new(0, 300, 0, 200)
frame.Position = UDim2.new(0, 100, 0, 100)
frame.BackgroundColor3 = Color3.fromRGB(35, 35, 35)
frame.BorderSizePixel = 0
frame.Active = true
frame.Draggable = true
frame.Visible = false
frame.Name = "FOVMenu"
frame.Parent = gui

-- 🔹 Título
local titleBar = Instance.new("TextLabel")
titleBar.Size = UDim2.new(1, 0, 0, 30)
titleBar.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
titleBar.Text = " FovScript "
titleBar.TextColor3 = Color3.fromRGB(255, 255, 255)
titleBar.Font = Enum.Font.SourceSansBold
titleBar.TextSize = 12
titleBar.Parent = frame


-- 📦 Conteúdo
local content = Instance.new("Frame")
content.Size = UDim2.new(1, 0, 1, -30)
content.Position = UDim2.new(0, 0, 0, 30)
content.BackgroundColor3 = Color3.fromRGB(45, 45, 45)
content.BorderSizePixel = 0
content.Name = "Content"
content.Parent = frame

-- 📋 Label do FOV
local fovLabel = Instance.new("TextLabel")
fovLabel.Size = UDim2.new(1, 0, 0, 30)
fovLabel.Position = UDim2.new(0, 0, 0, 10)
fovLabel.BackgroundTransparency = 1
fovLabel.Text = "FOV: " .. currentFOV
fovLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
fovLabel.Font = Enum.Font.SourceSans
fovLabel.TextSize = 20
fovLabel.Parent = content

-- ➖ Botão de diminuir FOV
local minusButton = Instance.new("TextButton")
minusButton.Size = UDim2.new(0, 50, 0, 30)
minusButton.Position = UDim2.new(0.25, -25, 0, 60)
minusButton.Text = "-"
minusButton.BackgroundColor3 = Color3.fromRGB(200, 60, 60)
minusButton.TextColor3 = Color3.fromRGB(255, 255, 255)
minusButton.Font = Enum.Font.SourceSansBold
minusButton.TextSize = 24
minusButton.Parent = content

-- ➕ Botão de aumentar FOV
local plusButton = Instance.new("TextButton")
plusButton.Size = UDim2.new(0, 50, 0, 30)
plusButton.Position = UDim2.new(0.75, -25, 0, 60)
plusButton.Text = "+"
plusButton.BackgroundColor3 = Color3.fromRGB(60, 200, 60)
plusButton.TextColor3 = Color3.fromRGB(255, 255, 255)
plusButton.Font = Enum.Font.SourceSansBold
plusButton.TextSize = 24
plusButton.Parent = content

-- 🧮 Caixa de texto para digitar o FOV
local fovInput = Instance.new("TextBox")
fovInput.Size = UDim2.new(0, 100, 0, 30)
fovInput.Position = UDim2.new(0.5, -50, 0, 100)
fovInput.PlaceholderText = "FovScript"
fovInput.Text = ""
fovInput.BackgroundColor3 = Color3.fromRGB(70, 70, 70)
fovInput.TextColor3 = Color3.fromRGB(255, 255, 255)
fovInput.Font = Enum.Font.SourceSans
fovInput.TextSize = 18
fovInput.ClearTextOnFocus = false
fovInput.Parent = content

-- 📈 Função de atualizar FOV
local function updateFOV(newFOV)
	currentFOV = math.clamp(newFOV, minFOV, maxFOV)
	camera.FieldOfView = currentFOV
	fovLabel.Text = "FOV: " .. currentFOV
end

-- Clique nos botões + e -
plusButton.MouseButton1Click:Connect(function()
	updateFOV(currentFOV + 1)
end)

minusButton.MouseButton1Click:Connect(function()
	updateFOV(currentFOV - 1)
end)

-- Digitar FOV manualmente
fovInput.FocusLost:Connect(function(enterPressed)
	if enterPressed then
		local num = tonumber(fovInput.Text)
		if num then
			updateFOV(num)
			fovInput.Text = ""
		else
			fovInput.Text = ""
		end
	end
end)

-- Clique para abrir/fechar o menu
openButton.MouseButton1Click:Connect(function()
	frame.Visible = not frame.Visible
end)

-- Minimizar o menu
local minimized = false
minimizeBtn.MouseButton1Click:Connect(function()
	minimized = not minimized
	if minimized then
		content.Visible = false
		frame.Size = UDim2.new(0, 300, 0, 30)
		minimizeBtn.Text = "⏵"
	else
		content.Visible = true
		frame.Size = UDim2.new(0, 300, 0, 200)
		minimizeBtn.Text = "⏷"
	end
end)
