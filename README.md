# ZallBeta_v3.6
-- // ZallBeta v3.6 FINAL FIX | by Zall x ChatGPT
-- ✅ Features: Fast Attack, Auto Level, Chest Farm, Weapon Config, Redz Style Tween

local OrionLib = loadstring(game:HttpGet("https://raw.githubusercontent.com/jensonhirst/Orion/main/source"))()
local Player = game.Players.LocalPlayer
local Character = Player.Character or Player.CharacterAdded:Wait()
local Backpack = Player:WaitForChild("Backpack")

getgenv().ZallFastAttack = false
getgenv().ZallAutoLevel = false
getgenv().ZallChestFarm = false
getgenv().Zall_Weapon = "Melee"

local Window = OrionLib:MakeWindow({
	Name = "⚔️ ZallBeta v3.6 | Blox Fruits",
	HidePremium = false,
	SaveConfig = true,
	ConfigFolder = "ZallBetaConfig",
	IntroEnabled = true,
	IntroText = "ZallBeta by Zall x ChatGPT",
})

local Main = Window:MakeTab({Name = "Main", Icon = "rbxassetid://4483345998", PremiumOnly = false})
Main:AddSection({Name = "Zall Main Controls"})

Main:AddToggle({
	Name = "Fast Attack",
	Default = false,
	Save = true,
	Callback = function(Value)
		getgenv().ZallFastAttack = Value
	end
})

Main:AddToggle({
	Name = "Auto Level Up (All Sea)",
	Default = false,
	Save = true,
	Callback = function(Value)
		getgenv().ZallAutoLevel = Value
		getgenv().ZallFastAttack = Value
	end
})

Main:AddToggle({
	Name = "Auto Chest Farm (Tween)",
	Default = false,
	Save = true,
	Callback = function(Value)
		getgenv().ZallChestFarm = Value
	end
})

Main:AddDropdown({
	Name = "Weapon Type",
	Default = "Melee",
	Options = {"Melee", "Sword", "Blox Fruit"},
	Callback = function(Value)
		getgenv().Zall_Weapon = Value
	end
})

Main:AddButton({
	Name = "❌ Close GUI",
	Callback = function()
		OrionLib:Destroy()
	end
})

function EquipWeapon()
	for _, v in pairs(Backpack:GetChildren()) do
		if v:IsA("Tool") and v.ToolTip == getgenv().Zall_Weapon then
			Character.Humanoid:EquipTool(v)
		end
	end
end

task.spawn(function()
	while task.wait(0.05) do
		if getgenv().ZallFastAttack then
			pcall(function()
				local CF = require(Player.PlayerScripts.CombatFramework)
				local ctrl = CF.activeController
				if ctrl and ctrl.equipped then
					ctrl.timeToNextAttack = 0
					ctrl.increment = 3
					ctrl.blocking = false
					ctrl.attacking = false
					ctrl:attack()
				end
			end)
		end
	end
end)local TweenService = game:GetService("TweenService")

function TweenTo(pos)
	local HRP = Character:WaitForChild("HumanoidRootPart")
	local dist = (HRP.Position - pos).Magnitude

	if dist > 500 then
		local steps = math.ceil(dist / 500)
		local dir = (pos - HRP.Position).Unit
		for i = 1, steps do
			local stepPos = HRP.Position + dir * math.min(500, (pos - HRP.Position).Magnitude)
			local tween = TweenService:Create(HRP, TweenInfo.new(1, Enum.EasingStyle.Linear), {CFrame = CFrame.new(stepPos)})
			tween:Play()
			tween.Completed:Wait()
			task.wait(0.1)
		end
	else
		local tween = TweenService:Create(HRP, TweenInfo.new(1, Enum.EasingStyle.Linear), {CFrame = CFrame.new(pos)})
		tween:Play()
		tween.Completed:Wait()
	end
end

function GetChests()
	local chests = {}
	for _, v in pairs(workspace:GetDescendants()) do
		if v:IsA("MeshPart") and v.Name:lower():find("chest") and v.Transparency == 0 then
			table.insert(chests, v)
		end
	end
	return chests
end

task.spawn(function()
	while task.wait(1) do
		if getgenv().ZallChestFarm then
			local chests = GetChests()
			table.sort(chests, function(a, b)
				return (Character.HumanoidRootPart.Position - a.Position).Magnitude < (Character.HumanoidRootPart.Position - b.Position).Magnitude
			end)
			for _, chest in ipairs(chests) do
				if getgenv().ZallChestFarm and chest and chest:IsDescendantOf(workspace) then
					TweenTo(chest.Position + Vector3.new(0, 2, 0))
					task.wait(0.2)
				end
			end
		end
	end
end)
local TweenService = game:GetService("TweenService")

function TweenTo(pos)
	local HRP = Character:WaitForChild("HumanoidRootPart")
	local dist = (HRP.Position - pos).Magnitude

	if dist > 500 then
		local steps = math.ceil(dist / 500)
		local dir = (pos - HRP.Position).Unit
		for i = 1, steps do
			local stepPos = HRP.Position + dir * math.min(500, (pos - HRP.Position).Magnitude)
			local tween = TweenService:Create(HRP, TweenInfo.new(1, Enum.EasingStyle.Linear), {CFrame = CFrame.new(stepPos)})
			tween:Play()
			tween.Completed:Wait()
			task.wait(0.1)
		end
	else
		local tween = TweenService:Create(HRP, TweenInfo.new(1, Enum.EasingStyle.Linear), {CFrame = CFrame.new(pos)})
		tween:Play()
		tween.Completed:Wait()
	end
end

function GetChests()
	local chests = {}
	for _, v in pairs(workspace:GetDescendants()) do
		if v:IsA("MeshPart") and v.Name:lower():find("chest") and v.Transparency == 0 then
			table.insert(chests, v)
		end
	end
	return chests
end

task.spawn(function()
	while task.wait(1) do
		if getgenv().ZallChestFarm then
			local chests = GetChests()
			table.sort(chests, function(a, b)
				return (Character.HumanoidRootPart.Position - a.Position).Magnitude < (Character.HumanoidRootPart.Position - b.Position).Magnitude
			end)
			for _, chest in ipairs(chests) do
				if getgenv().ZallChestFarm and chest and chest:IsDescendantOf(workspace) then
					TweenTo(chest.Position + Vector3.new(0, 2, 0))
					task.wait(0.2)
				end
			end
		end
	end
end)
local function getSea()
	local placeId = game.PlaceId
	if placeId == 2753915549 then
		return 1
	elseif placeId == 4442272183 then
		return 2
	elseif placeId == 7449423635 then
		return 3
	end
	return 1
end

local function getNearestMob()
	local mobs = {}
	for _, v in pairs(workspace.Enemies:GetChildren()) do
		if v:FindFirstChild("HumanoidRootPart") and v:FindFirstChild("Humanoid") and v.Humanoid.Health > 0 then
			table.insert(mobs, v)
		end
	end
	table.sort(mobs, function(a, b)
		return (Character.HumanoidRootPart.Position - a.HumanoidRootPart.Position).Magnitude <
		       (Character.HumanoidRootPart.Position - b.HumanoidRootPart.Position).Magnitude
	end)
	return mobs[1]
end

local function getQuestData()
	local level = Player.Data.Level.Value
	local sea = getSea()

	if sea == 1 then
		if level < 10 then
			return {Name = "BanditQuest1", Level = 1, Pos = Vector3.new(1060, 17, 1547), Enemy = "Bandit"}
		elseif level < 30 then
			return {Name = "Quest2", Level = 2, Pos = Vector3.new(-1145, 17, 4350), Enemy = "Monkey"}
		elseif level < 60 then
			return {Name = "Quest3", Level = 3, Pos = Vector3.new(-1602, 36, 145), Enemy = "Gorilla"}
		end
	elseif sea == 2 then
		if level < 750 then
			return {Name = "FishmanQuest", Level = 1, Pos = Vector3.new(-10563, 331, -8756), Enemy = "Fishman Warrior"}
		end
	elseif sea == 3 then
		if level < 1575 then
			return {Name = "PirateQuest", Level = 1, Pos = Vector3.new(-290, 44, 5583), Enemy = "Pirate"}
		end
	end
	return nil
end

function startQuest(questData)
	if not questData then return end
	local args = {
		[1] = "StartQuest",
		[2] = questData.Name,
		[3] = questData.Level
	}
	game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer(unpack(args))
end

task.spawn(function()
	while task.wait(1) do
		if getgenv().ZallAutoLevel then
			local quest = getQuestData()
			if quest then
				startQuest(quest)
				EquipWeapon()
				local mob = getNearestMob()
				if mob then
					repeat
						if not mob:FindFirstChild("Humanoid") or mob.Humanoid.Health <= 0 then break end
						Character.HumanoidRootPart.CFrame = mob.HumanoidRootPart.CFrame * CFrame.new(0, 15, 0)
						task.wait(0.2)
					until not mob.Parent or mob.Humanoid.Health <= 0 or not getgenv().ZallAutoLevel
				end
			end
		end
	end
end)
-- 📢 Notification
OrionLib:MakeNotification({
	Name = "ZallBeta v3.6 Final",
	Content = "Script loaded! All features ready ✅",
	Image = "rbxassetid://7733954760",
	Time = 5
})

-- 🧼 Finalizer (required)
OrionLib:Init()
