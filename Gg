local Players = game:GetService("Players")
local ReplicatedStorage = game:GetService("ReplicatedStorage")

-- ใส่ชื่อผู้เล่นที่จะให้สิทธิ์ใช้คำสั่งได้ตรงนี้
local ADMINS = {"YourRobloxUsername"} 

-- ฟังก์ชันตรวจสอบสิทธิ์ Admin
local function isAdmin(player)
	for _, adminName in ipairs(ADMINS) do
		if player.Name:lower() == adminName:lower() then
			return true
		end
	end
	return false
end

-- สร้างระบบเงิน (Leaderstats)
Players.PlayerAdded:Connect(function(player)
	local leaderstats = Instance.new("Folder")
	leaderstats.Name = "leaderstats"
	leaderstats.Parent = player

	local coins = Instance.new("IntValue")
	coins.Name = "Coins"
	coins.Value = 1000
	coins.Parent = leaderstats

	-- ฟังเสียงพิมพ์ในช่องแชท
	player.Chatted:Connect(function(message)
		if not isAdmin(player) then return end

		local args = string.split(message, " ")
		local command = args[1]:lower()

		--------------------------------------------------
		-- 1. คำสั่งเสกเงิน: !cash [จำนวน]
		-- ตัวอย่าง: !cash 999999
		--------------------------------------------------
		if command == "!cash" or command == "!money" then
			local amount = tonumber(args[2]) or 1000000
			coins.Value = coins.Value + amount
			print(player.Name .. " เสกเงินจำนวน: " .. amount)

		--------------------------------------------------
		-- 2. คำสั่งเสกของไม่จำกัด: !item [ชื่อของ]
		-- ตัวอย่าง: !item Sword
		--------------------------------------------------
		elseif command == "!item" or command == "!give" then
			local itemName = args[2]
			if not itemName then return end

			local toolsFolder = ReplicatedStorage:FindFirstChild("Tools")
			if toolsFolder then
				local itemTemplate = toolsFolder:FindFirstChild(itemName)
				if itemTemplate then
					local newItem = itemTemplate:Clone()
					newItem.Parent = player.Backpack -- เสกใส่กระเป๋าของผู้เล่น
					print("เสกไอเทม " .. itemName .. " ให้ " .. player.Name)
				else
					warn("ไม่พบไอเทมชื่อ " .. itemName .. " ใน ReplicatedStorage.Tools")
				end
			end

		--------------------------------------------------
		-- 3. คำสั่งเสกสัตว์เลี้ยง: !pet [ชื่อสัตว์]
		-- ตัวอย่าง: !pet Dog
		--------------------------------------------------
		elseif command == "!pet" or command == "!spawnpet" then
			local petName = args[2]
			if not petName then return end

			local petsFolder = ReplicatedStorage:FindFirstChild("Pets")
			if petsFolder then
				local petTemplate = petsFolder:FindFirstChild(petName)
				if petTemplate and player.Character then
					local newPet = petTemplate:Clone()
					-- วางสัตว์เลี้ยงไว้ข้างตัวผู้เล่น
					local hrp = player.Character:FindFirstChild("HumanoidRootPart")
					if hrp then
						newPet:PivotTo(hrp.CFrame * CFrame.new(3, 0, 0))
						newPet.Parent = workspace
						print("เสกสัตว์เลี้ยง " .. petName .. " สำเร็จ!")
					end
				else
					warn("ไม่พบสัตว์เลี้ยงชื่อ " .. petName .. " ใน ReplicatedStorage.Pets")
				end
			end
		end
	end)
end)
