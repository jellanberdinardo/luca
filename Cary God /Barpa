local Rayfield = loadstring(game:HttpGet('https://sirius.menu/rayfield'))()
local player = game.Players.LocalPlayer
local UIS = game:GetService("UserInputService")
local RunService = game:GetService("RunService")
local ReplicatedStorage = game:GetService("ReplicatedStorage")

local CarryReplic = ReplicatedStorage:WaitForChild("CarryReplic")
local CarryRemotes = CarryReplic:WaitForChild("CarryRemotes")
local CarryRemote = CarryRemotes:WaitForChild("CarryRemote")
local CarryChoices = CarryReplic:WaitForChild("CarryChoices")
local DEFAULT_CARRY = CarryChoices:FindFirstChildWhichIsA("ModuleScript")

local carriedPlayers = {}

-- ===============================
-- Main Window
-- ===============================
local Window = Rayfield:CreateWindow({
	Name = "BARPA Hub God Carry",
	Icon = 0,
	LoadingTitle = "BARPA Hub",
	LoadingSubtitle = "God Carry Mode",
	ShowText = "BARPA",
	Theme = "Ocean",
	ToggleUIKeybind = "N"
})

-- ===============================
-- PLAYER TAB
-- ===============================
local PlayerTab = Window:CreateTab("Player", 4483362458)

-- WalkSpeed & JumpPower
local walkSpeed, jumpPower = 16, 50
PlayerTab:CreateSlider({Name="WalkSpeed", Range={16,500}, Increment=1, Suffix="Studs", CurrentValue=16,
	Callback=function(value) walkSpeed = value end})
PlayerTab:CreateSlider({Name="JumpPower", Range={50,500}, Increment=5, Suffix="Studs", CurrentValue=50,
	Callback=function(value) jumpPower = value end})

-- Update Humanoid
spawn(function()
	while true do
		task.wait(0.1)
		local char = player.Character
		if char and char:FindFirstChild("Humanoid") then
			char.Humanoid.WalkSpeed = walkSpeed
			char.Humanoid.JumpPower = jumpPower
		end
	end
end)

-- Fly Mode
local flying, flySpeed = false, 50
local moveVector = Vector3.new()
local bodyVelocity

PlayerTab:CreateToggle({Name="Fly Mode", CurrentValue=false, Flag="FlyToggle",
	Callback=function(state)
		flying = state
		local char = player.Character
		if char and char:FindFirstChild("HumanoidRootPart") then
			if flying then
				bodyVelocity = Instance.new("BodyVelocity")
				bodyVelocity.MaxForce = Vector3.new(1e5,1e5,1e5)
				bodyVelocity.Velocity = Vector3.new()
				bodyVelocity.Parent = char.HumanoidRootPart
			else
				if bodyVelocity then bodyVelocity:Destroy() end
			end
		end
	end})

UIS.InputBegan:Connect(function(input, gp)
	if gp then return end
	if input.KeyCode == Enum.KeyCode.W then moveVector += Vector3.new(0,0,-1) end
	if input.KeyCode == Enum.KeyCode.S then moveVector += Vector3.new(0,0,1) end
	if input.KeyCode == Enum.KeyCode.A then moveVector += Vector3.new(-1,0,0) end
	if input.KeyCode == Enum.KeyCode.D then moveVector += Vector3.new(1,0,0) end
	if input.KeyCode == Enum.KeyCode.Space then moveVector += Vector3.new(0,1,0) end
	if input.KeyCode == Enum.KeyCode.LeftShift then moveVector += Vector3.new(0,-1,0) end
end)

UIS.InputEnded:Connect(function(input)
	if input.KeyCode == Enum.KeyCode.W then moveVector -= Vector3.new(0,0,-1) end
	if input.KeyCode == Enum.KeyCode.S then moveVector -= Vector3.new(0,0,1) end
	if input.KeyCode == Enum.KeyCode.A then moveVector -= Vector3.new(-1,0,0) end
	if input.KeyCode == Enum.KeyCode.D then moveVector -= Vector3.new(1,0,0) end
	if input.KeyCode == Enum.KeyCode.Space then moveVector -= Vector3.new(0,1,0) end
	if input.KeyCode == Enum.KeyCode.LeftShift then moveVector -= Vector3.new(0,-1,0) end
end)

RunService.RenderStepped:Connect(function()
	if flying and bodyVelocity and player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
		local hrp = player.Character.HumanoidRootPart
		bodyVelocity.Velocity = moveVector.Magnitude > 0 and moveVector.Unit * flySpeed or Vector3.new()

		-- make carried players follow you
		for _, plr in pairs(carriedPlayers) do
			if plr.Character and plr.Character:FindFirstChild("HumanoidRootPart") then
				plr.Character.HumanoidRootPart.CFrame = hrp.CFrame * CFrame.new(0,0,-2)
				local carryVal = plr.Character:FindFirstChild("Carryble")
				if carryVal then carryVal.Value = false -- lock them
				end
			end
		end
	end
end)

-- ===============================
-- God Carry Buttons
-- ===============================
PlayerTab:CreateButton({Name="Carry All (Only I Can Drop)", Callback=function()
	for _, plr in pairs(game.Players:GetPlayers()) do
		if plr ~= player then
			CarryRemote:FireServer({cmd="Carry", firstPlr=plr, carrychoicesss=DEFAULT_CARRY})
			table.insert(carriedPlayers, plr)
		end
	end
end})

PlayerTab:CreateButton({Name="Drop All", Callback=function()
	for _, plr in pairs(carriedPlayers) do
		CarryRemote:FireServer({cmd="Declinecarry", firstPlr=plr})
	end
	carriedPlayers = {}
end})

Rayfield:Notify({Title="BARPA Hub v2", Content="God Carry Loaded: Only you can drop!", Duration=5, Image=4483362458})
