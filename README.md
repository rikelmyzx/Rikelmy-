-- ⚠️ ESP + NoClip + Teleporte para executores (Synapse, Fluxus, etc.)

-- Destruir GUI antiga, se existir
pcall(function() game.CoreGui:FindFirstChild("BrainrotESPUI"):Destroy() end)

-- Criar GUI principal
local gui = Instance.new("ScreenGui", game.CoreGui)
gui.Name = "BrainrotESPUI"

local frame = Instance.new("Frame", gui)
frame.Size = UDim2.new(0, 240, 0, 250)
frame.Position = UDim2.new(0.05, 0, 0.4, 0)
frame.BackgroundColor3 = Color3.fromRGB(15, 20, 45)
frame.BorderSizePixel = 0
Instance.new("UICorner", frame).CornerRadius = UDim.new(0, 8)

local layout = Instance.new("UIListLayout", frame)
layout.Padding = UDim.new(0, 8)
layout.HorizontalAlignment = Enum.HorizontalAlignment.Center
layout.VerticalAlignment = Enum.VerticalAlignment.Center

local function createBtn(text, callback)
	local b = Instance.new("TextButton", frame)
	b.Size = UDim2.new(0, 200, 0, 35)
	b.Text = text
	b.Font = Enum.Font.GothamBold
	b.TextSize = 14
	b.TextColor3 = Color3.fromRGB(255, 255, 255)
	b.BackgroundColor3 = Color3.fromRGB(30, 35, 70)
	Instance.new("UICorner", b).CornerRadius = UDim.new(0, 6)
	b.MouseButton1Click:Connect(callback)
end

----------------------------
-- 🟢 ESP FUNCTION
----------------------------
createBtn("👁️ Ativar ESP", function()
	for _, v in pairs(game.Players:GetPlayers()) do
		if v ~= game.Players.LocalPlayer then
			local billboard = Instance.new("BillboardGui")
			billboard.Adornee = v.Character:WaitForChild("Head")
			billboard.AlwaysOnTop = true
			billboard.Size = UDim2.new(0, 100, 0, 40)
			billboard.StudsOffset = Vector3.new(0, 2, 0)
			billboard.Name = "ESP"

			local label = Instance.new("TextLabel", billboard)
			label.Text = v.Name
			label.TextColor3 = Color3.fromRGB(0, 255, 255)
			label.TextSize = 14
			label.BackgroundTransparency = 1
			label.Size = UDim2.new(1, 0, 1, 0)

			billboard.Parent = v.Character
		end
	end

	game.Players.PlayerAdded:Connect(function(player)
		player.CharacterAdded:Connect(function(char)
			wait(1)
			local head = char:WaitForChild("Head")
			local billboard = Instance.new("BillboardGui", char)
			billboard.Adornee = head
			billboard.AlwaysOnTop = true
			billboard.Size = UDim2.new(0, 100, 0, 40)
			billboard.StudsOffset = Vector3.new(0, 2, 0)
			billboard.Name = "ESP"

			local label = Instance.new("TextLabel", billboard)
			label.Text = player.Name
			label.TextColor3 = Color3.fromRGB(0, 255, 255)
			label.TextSize = 14
			label.BackgroundTransparency = 1
			label.Size = UDim2.new(1, 0, 1, 0)
		end)
	end)
end)

----------------------------
-- 🟣 NOCLIP FUNCTION
----------------------------
local noclip = false
createBtn("🚪 Ativar NoClip", function()
	noclip = not noclip
	local player = game.Players.LocalPlayer
	local char = player.Character

	game:GetService("RunService").Stepped:Connect(function()
		if noclip and char then
			for _, part in pairs(char:GetDescendants()) do
				if part:IsA("BasePart") then
					part.CanCollide = false
				end
			end
		end
	end)
end)

----------------------------
-- 🔵 TELEPORT FUNCTION
----------------------------
createBtn("📍 Teleportar p/ Jogador", function()
	local lp = game.Players.LocalPlayer
	local list = {}

	for _, p in pairs(game.Players:GetPlayers()) do
		if p ~= lp then
			table.insert(list, p.Name)
		end
	end

	local selected = list[1]
	local dialog = Instance.new("TextButton", frame)
	dialog.Text = "Clique aqui para teleportar até: " .. selected
	dialog.Size = UDim2.new(0, 200, 0, 35)
	dialog.BackgroundColor3 = Color3.fromRGB(60, 65, 120)
	dialog.TextColor3 = Color3.fromRGB(255, 255, 255)
	dialog.Font = Enum.Font.Gotham
	dialog.TextSize = 13
	Instance.new("UICorner", dialog)

	dialog.MouseButton1Click:Connect(function()
		for _, p in pairs(game.Players:GetPlayers()) do
			if p.Name == selected then
				local char = lp.Character or lp.CharacterAdded:Wait()
				local target = p.Character:FindFirstChild("HumanoidRootPart")
				if target then
					char:MoveTo(target.Position + Vector3.new(0, 3, 0))
				end
			end
		end
	end)
end)
