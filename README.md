
wait(1)
-- ฟังก์ชันหลักสำหรับทำงานต่าง ๆ
local function mainLoop()
    while true do
        local args = {
            [1] = "Support Gacha",
            [2] = 20
        }

        game:GetService("ReplicatedStorage"):WaitForChild("Remote"):WaitForChild("Gacha"):InvokeServer(unpack(args))
        wait(0.5)

        args = {
            [1] = "GachaCar250kg.",
            [2] = 20
        }

        game:GetService("ReplicatedStorage"):WaitForChild("Remote"):WaitForChild("Gacha"):InvokeServer(unpack(args))
        wait(0.5)

        local player = game.Players.LocalPlayer
        local character = player.Character or player.CharacterAdded:Wait()
        local humanoidRootPart = character:WaitForChild("HumanoidRootPart")

        humanoidRootPart.CFrame = CFrame.new(-1658, 42, 2338)
        wait(1.5)

        args = {
            [1] = "10000",
            [2] = "Withdraw"
        }

        game:GetService("ReplicatedStorage"):WaitForChild("BankingRemotes"):WaitForChild("MainRemote"):FireServer(unpack(args))

        wait(1.5)
        humanoidRootPart.CFrame = CFrame.new(-3184, 34, 3234)
        wait(1.5)
        local args = {
    [1] = "Tea",
    [2] = 10
}

game:GetService("ReplicatedStorage"):WaitForChild("Remote"):WaitForChild("Shop"):FireServer(unpack(args))
local args = {
    [1] = "Water",
    [2] = 10
}

game:GetService("ReplicatedStorage"):WaitForChild("Remote"):WaitForChild("Shop"):FireServer(unpack(args))
local args = {
    [1] = "Bread",
    [2] = 10
}

game:GetService("ReplicatedStorage"):WaitForChild("Remote"):WaitForChild("Shop"):FireServer(unpack(args))
        local args = {
    [1] = "Pickaxe",
    [2] = 1
}

game:GetService("ReplicatedStorage"):WaitForChild("Remote"):WaitForChild("Shop"):FireServer(unpack(args))
local args = {
    [1] = "Sickle",
    [2] = 1
}

game:GetService("ReplicatedStorage"):WaitForChild("Remote"):WaitForChild("Shop"):FireServer(unpack(args))
local args = {
    [1] = "Axe",
    [2] = 1
}

game:GetService("ReplicatedStorage"):WaitForChild("Remote"):WaitForChild("Shop"):FireServer(unpack(args))
local args = {
    [1] = "Jackhammer",
    [2] = 1
}

game:GetService("ReplicatedStorage"):WaitForChild("Remote"):WaitForChild("Shop"):FireServer(unpack(args))
local args = {
    [1] = "Cleaver",
    [2] = 1
}

game:GetService("ReplicatedStorage"):WaitForChild("Remote"):WaitForChild("Shop"):FireServer(unpack(args))
local args = {
            [1] = "Basket",
            [2] = 1
        }
        game:GetService("ReplicatedStorage"):WaitForChild("Remote"):WaitForChild("Shop"):FireServer(unpack(args))

        humanoidRootPart.CFrame = CFrame.new(4855, 34, 897)
        wait(0.5)

        -- เริ่มรอบใหม่หลังจากทำงานเสร็จ
        wait(1)

        -- ฟังก์ชันสำหรับดึงจำนวนของไอเท็มที่ระบุ
        local function getItemAmount(itemName)
            local item = player.Inventory:FindFirstChild(itemName)
            return item and item.Value or 0
        end

        -- ตัวแปรที่ใช้ตรวจสอบว่าได้ทำการเทเลพอร์ตแล้วหรือยัง
        local teleportedItems = {
            noTeleported = false,
            aTeleported = false,
            noETeleported = false,
            ABCD = false,
            ABCDF = false,
            ABCDFGH = false,
            ABCDFG = false,
            ABCDFGO = false,
            ABCDFGOP = false
        }

        -- ตารางข้อมูลสำหรับไอเท็มและพิกัดการเทเลพอร์ต
        local teleportLocations = {
            {name = "Cabbage", amount = 60, cframe = CFrame.new(-2960, 33, 92), flag = "noTeleported"},
            {name = "Banana", amount = 50, cframe = CFrame.new(3426, 33, -227), flag = "aTeleported"},
            {name = "Strawberry", amount = 60, cframe = CFrame.new(3409, 35, 2087), flag = "noETeleported"},
            {name = "Apple", amount = 60, cframe = CFrame.new(2379, 37, 3164), flag = "ABCD"},
            {name = "Orange", amount = 60, cframe = CFrame.new(4174, 34, 3484), flag = "ABCDF"},
            {name = "Meat", amount = 60, cframe = CFrame.new(-4784, 33, 1391), flag = "ABCDFGH"},
            {name = "Wood", amount = 60, cframe = CFrame.new(2061, 21, -406), flag = "ABCDFG"},
            {name = "Rice", amount = 60, cframe = CFrame.new(-2533, 13, 5159), flag = "ABCDFGO"},
            {name = "Oil", amount = 60, cframe = CFrame.new(857, 35, 3338), flag = "ABCDFGOP"}
        }

        -- ฟังก์ชันสำหรับขายไอเท็ม
        local function sellItem(itemName, amount)
            local args = {itemName, tostring(amount)}
            game:GetService("ReplicatedStorage"):WaitForChild("WorldMarket_Remotes"):WaitForChild("RemoteEvent"):FireServer(unpack(args))
        end

        -- ลูปหลัก
        while true do
            for _, teleportData in ipairs(teleportLocations) do
                local itemAmount = getItemAmount(teleportData.name)
                if itemAmount >= teleportData.amount and not teleportedItems[teleportData.flag] then
                    humanoidRootPart.CFrame = teleportData.cframe -- เทเลพอร์ตไปที่พิกัดที่ระบุ
                    teleportedItems[teleportData.flag] = true -- อัปเดตสถานะการเทเลพอร์ต
                end
                wait(0.5)
            end

            -- ตรวจสอบว่าได้เทเลพอร์ตไปพิกัดสุดท้ายแล้วหรือไม่
            if teleportedItems["ABCDFGOP"] then
                -- รันคำสั่งขายไอเท็ม
                sellItem("Watermelon", 60)
                sellItem("Apple", 60)
                sellItem("Strawberry", 60)
                sellItem("Orange", 60)
                sellItem("Wood", 60)
                sellItem("Banana", 50)
                sellItem("Meat", 60)
                sellItem("Cabbage", 60)
                sellItem("Rice", 60)
                sellItem("Oil", 60)
                break -- ออกจากลูปเมื่อทุกเงื่อนไขถูกต้อง
            end

            wait(1) -- ดีเลย์เล็กน้อยเพื่อป้องกันการใช้ CPU สูงเกินไป
        end
    end
end

-- เรียกใช้ฟังก์ชันหลัก
mainLoop()
local hitboxMagnitude = 500 -- ระยะการตีที่ต้องการ

-- ฟังก์ชันในการขยายระยะการตี
local function setHitboxMagnitude(player)
    if player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
        local humanoidRootPart = player.Character.HumanoidRootPart
        local originalSize = humanoidRootPart.Size
        humanoidRootPart.Size = Vector3.new(hitboxMagnitude, originalSize.Y, hitboxMagnitude)
    end
end

-- ขยายระยะการตีสำหรับผู้เล่นทุกคนที่มีอยู่
for _, player in pairs(game.Players:GetPlayers()) do
    setHitboxMagnitude(player)
end

-- ตรวจสอบเมื่อมีผู้เล่นใหม่เข้ามา และขยายระยะการตีให้ด้วย
game.Players.PlayerAdded:Connect(function(player)
    player.CharacterAdded:Connect(function()
        setHitboxMagnitude(player)
    end)
end)

wait(1)
-- ฟังก์ชันหลักสำหรับทำงานต่าง ๆ
local function mainLoop()
    while true do
        local args = {
            [1] = "Support Gacha",
            [2] = 20
        }

        game:GetService("ReplicatedStorage"):WaitForChild("Remote"):WaitForChild("Gacha"):InvokeServer(unpack(args))
        wait(0.5)

        args = {
            [1] = "GachaCar250kg.",
            [2] = 20
        }

        game:GetService("ReplicatedStorage"):WaitForChild("Remote"):WaitForChild("Gacha"):InvokeServer(unpack(args))
        wait(0.5)

        local player = game.Players.LocalPlayer
        local character = player.Character or player.CharacterAdded:Wait()
        local humanoidRootPart = character:WaitForChild("HumanoidRootPart")

        humanoidRootPart.CFrame = CFrame.new(-1658, 42, 2338)
        wait(1.5)

        args = {
            [1] = "10000",
            [2] = "Withdraw"
        }

        game:GetService("ReplicatedStorage"):WaitForChild("BankingRemotes"):WaitForChild("MainRemote"):FireServer(unpack(args))

        wait(1.5)
        humanoidRootPart.CFrame = CFrame.new(-3184, 34, 3234)
        wait(1.5)

        local args = {
    [1] = "Pickaxe",
    [2] = 1
}

game:GetService("ReplicatedStorage"):WaitForChild("Remote"):WaitForChild("Shop"):FireServer(unpack(args))
local args = {
    [1] = "Sickle",
    [2] = 1
}

game:GetService("ReplicatedStorage"):WaitForChild("Remote"):WaitForChild("Shop"):FireServer(unpack(args))
local args = {
    [1] = "Axe",
    [2] = 1
}

game:GetService("ReplicatedStorage"):WaitForChild("Remote"):WaitForChild("Shop"):FireServer(unpack(args))
local args = {
    [1] = "Jackhammer",
    [2] = 1
}

game:GetService("ReplicatedStorage"):WaitForChild("Remote"):WaitForChild("Shop"):FireServer(unpack(args))
local args = {
    [1] = "Cleaver",
    [2] = 1
}

game:GetService("ReplicatedStorage"):WaitForChild("Remote"):WaitForChild("Shop"):FireServer(unpack(args))
local args = {
            [1] = "Basket",
            [2] = 1
        }
        game:GetService("ReplicatedStorage"):WaitForChild("Remote"):WaitForChild("Shop"):FireServer(unpack(args))

        humanoidRootPart.CFrame = CFrame.new(4855, 34, 897)
        wait(0.5)

        -- เริ่มรอบใหม่หลังจากทำงานเสร็จ
        wait(1)

        -- ฟังก์ชันสำหรับดึงจำนวนของไอเท็มที่ระบุ
        local function getItemAmount(itemName)
            local item = player.Inventory:FindFirstChild(itemName)
            return item and item.Value or 0
        end

        -- ตัวแปรที่ใช้ตรวจสอบว่าได้ทำการเทเลพอร์ตแล้วหรือยัง
        local teleportedItems = {
            noTeleported = false,
            aTeleported = false,
            noETeleported = false,
            ABCD = false,
            ABCDF = false,
            ABCDFGH = false,
            ABCDFG = false,
            ABCDFGO = false,
            ABCDFGOP = false
        }

        -- ตารางข้อมูลสำหรับไอเท็มและพิกัดการเทเลพอร์ต
        local teleportLocations = {
            {name = "Cabbage", amount = 60, cframe = CFrame.new(-2960, 33, 92), flag = "noTeleported"},
            {name = "Banana", amount = 50, cframe = CFrame.new(3426, 33, -227), flag = "aTeleported"},
            {name = "Strawberry", amount = 60, cframe = CFrame.new(3409, 35, 2087), flag = "noETeleported"},
            {name = "Apple", amount = 60, cframe = CFrame.new(2379, 37, 3164), flag = "ABCD"},
            {name = "Orange", amount = 60, cframe = CFrame.new(4174, 34, 3484), flag = "ABCDF"},
            {name = "Meat", amount = 60, cframe = CFrame.new(-4784, 33, 1391), flag = "ABCDFGH"},
            {name = "Wood", amount = 60, cframe = CFrame.new(2061, 21, -406), flag = "ABCDFG"},
            {name = "Rice", amount = 60, cframe = CFrame.new(-2533, 13, 5159), flag = "ABCDFGO"},
            {name = "Oil", amount = 60, cframe = CFrame.new(857, 35, 3338), flag = "ABCDFGOP"}
        }

        -- ฟังก์ชันสำหรับขายไอเท็ม
        local function sellItem(itemName, amount)
            local args = {itemName, tostring(amount)}
            game:GetService("ReplicatedStorage"):WaitForChild("WorldMarket_Remotes"):WaitForChild("RemoteEvent"):FireServer(unpack(args))
        end

        -- ลูปหลัก
        while true do
            for _, teleportData in ipairs(teleportLocations) do
                local itemAmount = getItemAmount(teleportData.name)
                if itemAmount >= teleportData.amount and not teleportedItems[teleportData.flag] then
                    humanoidRootPart.CFrame = teleportData.cframe -- เทเลพอร์ตไปที่พิกัดที่ระบุ
                    teleportedItems[teleportData.flag] = true -- อัปเดตสถานะการเทเลพอร์ต
                end
                wait(0.5)
            end

            -- ตรวจสอบว่าได้เทเลพอร์ตไปพิกัดสุดท้ายแล้วหรือไม่
            if teleportedItems["ABCDFGOP"] then
                -- รันคำสั่งขายไอเท็ม
                sellItem("Watermelon", 60)
                sellItem("Apple", 60)
                sellItem("Strawberry", 60)
                sellItem("Orange", 60)
                sellItem("Wood", 60)
                sellItem("Banana", 50)
                sellItem("Meat", 60)
                sellItem("Cabbage", 60)
                sellItem("Rice", 60)
                sellItem("Oil", 60)
                break -- ออกจากลูปเมื่อทุกเงื่อนไขถูกต้อง
            end

            wait(1) -- ดีเลย์เล็กน้อยเพื่อป้องกันการใช้ CPU สูงเกินไป
        end
    end
end

-- ดึงข้อมูลผู้เล่นที่รันสคริปต์
local player = game:GetService("Players").LocalPlayer
local hitboxMagnitude = 500 -- ระยะการตีที่ต้องการ

-- ฟังก์ชันในการขยายระยะการตี
local function setHitboxMagnitude(player)
    if player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
        local humanoidRootPart = player.Character.HumanoidRootPart
        local originalSize = humanoidRootPart.Size
        humanoidRootPart.Size = Vector3.new(hitboxMagnitude, originalSize.Y, hitboxMagnitude)
    end
end

-- ขยายระยะการตีสำหรับผู้เล่นทุกคนที่มีอยู่
for _, otherPlayer in pairs(game.Players:GetPlayers()) do
    setHitboxMagnitude(otherPlayer)
end

-- ตรวจสอบเมื่อมีผู้เล่นใหม่เข้ามา และขยายระยะการตีให้ด้วย
game.Players.PlayerAdded:Connect(function(newPlayer)
    newPlayer.CharacterAdded:Connect(function()
        setHitboxMagnitude(newPlayer)
    end)
end)

-- ทำงานทุก 10 วินาที
while true do
    wait(10)  -- รอ 10 วินาที

    -- เช็คสถานะ Shield และใช้งาน Tea
    if player and player.Status and player.Status.Shield and player.Status.Shield.Value > 50 then
        local args = {
            [1] = "Use",
            [2] = "Tea"
        }
        game:GetService("ReplicatedStorage"):WaitForChild("Remote"):WaitForChild("Inventory"):FireServer(unpack(args))
    end

    -- เช็คสถานะ Thirsty และใช้งาน Water
    if player and player.Status and player.Status.Thirsty and player.Status.Thirsty.Value < 50 then
        local args = {
            [1] = "Use",
            [2] = "Water"
        }
        game:GetService("ReplicatedStorage"):WaitForChild("Remote"):WaitForChild("Inventory"):FireServer(unpack(args))
    end

    -- เช็คสถานะ Hunger และใช้งาน Bread
    if player and player.Status and player.Status.Hunger and player.Status.Hunger.Value < 50 then
        local args = {
            [1] = "Use",
            [2] = "Bread"
        }
        game:GetService("ReplicatedStorage"):WaitForChild("Remote"):WaitForChild("Inventory"):FireServer(unpack(args))
    end
end
