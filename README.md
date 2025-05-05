-- Serviços
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local Camera = workspace.CurrentCamera
local LocalPlayer = Players.LocalPlayer

-- Espera o PlayerGui carregar
local PlayerGui = LocalPlayer:WaitForChild("PlayerGui")

-- Interface
local gui = Instance.new("ScreenGui")
gui.Name = "AimbotUI"
gui.ResetOnSpawn = false
gui.Parent = PlayerGui

local toggleBtn = Instance.new("TextButton")
toggleBtn.Size = UDim2.new(0, 140, 0, 40)
toggleBtn.Position = UDim2.new(0, 20, 0, 20)
toggleBtn.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
toggleBtn.TextColor3 = Color3.new(1, 1, 1)
toggleBtn.Font = Enum.Font.SourceSansBold
toggleBtn.Text = "Aimbot: OFF"
toggleBtn.TextSize = 20
toggleBtn.Parent = gui

local info = Instance.new("TextLabel")
info.Size = UDim2.new(0, 200, 0, 30)
info.Position = UDim2.new(0, 20, 0, 70)
info.BackgroundTransparency = 1
info.TextColor3 = Color3.new(1, 1, 1)
info.Font = Enum.Font.SourceSans
info.Text = "Alvo: Nenhum"
info.TextSize = 18
info.Parent = gui

-- Configuração do Aimbot
local AimbotEnabled = false
local AimRadius = 1000
local TargetPart = "Head"

local function getClosestEnemy()
	local closest, shortest = nil, AimRadius
	for _, player in pairs(Players:GetPlayers()) do
		if player ~= LocalPlayer and player.Team ~= LocalPlayer.Team then
			local char = player.Character
			local part = char and char:FindFirstChild(TargetPart)
			local root = LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart")

			if part and root then
				local dist = (part.Position - Camera.CFrame.Position).Magnitude
				if dist < shortest then
					closest = part
					shortest = dist
				end
			end
		end
	end
	return closest
end

-- Toggle botão
toggleBtn.MouseButton1Click:Connect(function()
	AimbotEnabled = not AimbotEnabled
	toggleBtn.Text = AimbotEnabled and "Aimbot: ON" or "Aimbot: OFF"
end)

-- Loop de mira
RunService.RenderStepped:Connect(function()
	if AimbotEnabled then
		local target = getClosestEnemy()
		if target then
			Camera.CFrame = CFrame.new(Camera.CFrame.Position, target.Position)
			info.Text = "Alvo: " .. target.Parent.Name
		else
			info.Text = "Alvo: Nenhum"
		end
	else
		info.Text = "Aimbot desativado"
	end
end)
# Aimbb
