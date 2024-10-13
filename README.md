local Fluent = loadstring(game:HttpGet("https://github.com/dawid-scripts/Fluent/releases/latest/download/main.lua"))()
local SaveManager = loadstring(game:HttpGet("https://raw.githubusercontent.com/dawid-scripts/Fluent/master/Addons/SaveManager.lua"))()
local InterfaceManager = loadstring(game:HttpGet("https://raw.githubusercontent.com/dawid-scripts/Fluent/master/Addons/InterfaceManager.lua"))()

local Window = Fluent:CreateWindow({
    Title = "Fluent " .. Fluent.Version,
    SubTitle = "by dawid",
    TabWidth = 160,
    Size = UDim2.fromOffset(580, 460),
    Acrylic = true, -- The blur may be detectable, setting this to false disables blur entirely
    Theme = "Dark",
    MinimizeKey = Enum.KeyCode.LeftControl -- Used when theres no MinimizeKeybind
})

--Fluent provides Lucide Icons https://lucide.dev/icons/ for the tabs, icons are optional
local Tabs = {
    Vibranium = Window:AddTab({ Title = "Vibranium", Icon = "" }),
    Main = Window:AddTab({ Title = "Main", Icon = "" }),
    AirDrop = Window:AddTab({ Title = "AirDrop", Icon = "" }),
    groblins = Window:AddTab({ Title = "groblins", Icon = "" }),
    Teleport = Window:AddTab({ Title = "Teleport", Icon = "" }),
    Settings = Window:AddTab({ Title = "Settings", Icon = "settings" })
}

local Options = Fluent.Options

do
    Fluent:Notify({
        Title = "Water hub",
        Content = "open Water hub",
        SubContent = "Water hub", -- Optional
        Duration = 5 -- Set to nil to make the notification not disappear
    })

local Toggle = Tabs.Vibranium:AddToggle("MyToggle", {Title = "check", Default = false})

Toggle:OnChanged(function(state)
    local player = game.Players.LocalPlayer
    local alreadyTeleported = false -- ใช้ตัวแปรนี้เพื่อป้องกันการวาบซ้ำ

    -- ฟังก์ชันในการเช็คและวาบ
    local function checkAndTeleport()
        -- ตรวจสอบว่า Inventory และ Stone มีอยู่จริง
        if player:FindFirstChild("Inventory") and player.Inventory:FindFirstChild("Stone") then
            local inventory = player.Inventory
            local stoneAmount = inventory.Stone.Value -- เช็คค่าของ Stone
            
            if stoneAmount >= 60 and not alreadyTeleported then
                -- วาบไปที่ตำแหน่งที่กำหนดครั้งแรก
                player.Character.HumanoidRootPart.CFrame = CFrame.new(2898, 40, -843)
                alreadyTeleported = true -- วาบแล้วจะไม่วาบซ้ำ
                print("วาบครั้งแรก")
            end
            
            -- ถ้า Stone ลดลงให้รีเซ็ตสถานะการวาบใหม่
            if stoneAmount < 60 then
                alreadyTeleported = false -- รีเซ็ตเพื่อให้วาบใหม่เมื่อ Stone ถึง 60
                print("จำนวน Stone ไม่พอ, รอการวาบใหม่")
            end
        else
            print("ไม่พบ Inventory หรือ Stone ในผู้เล่น")
        end
    end

    -- ถ้า Toggle ถูกเปิดใช้งานให้ทำการเช็คต่อเนื่อง
    if state then
        while state do
            checkAndTeleport() -- เรียกฟังก์ชันเช็คและวาบ
            wait(2) -- รอ 2 วินาทีแล้วเช็คใหม่
        end
    end
end)

local Toggle = Tabs.Vibranium:AddToggle("MyToggle", {Title = "check stone v2", Default = false })

-- ตัวแปรสำหรับเช็คว่าวาบไปแล้วหรือยัง
local hasTeleported = false 
local running = false -- ตัวแปรสำหรับควบคุมการทำงานของลูป

Toggle:OnChanged(function(state)
    if state then
        running = true -- เริ่มทำงานเมื่อ Toggle ถูกเปิด
        while running do
            local player = game:GetService("Players").LocalPlayer
            
            if player then
                local stoneCount = player.Inventory:FindFirstChild("Stone") and player.Inventory.Stone.Value or 0

                if stoneCount == 0 and not hasTeleported then
                    -- วาบไปที่ตำแหน่ง -4539, 38, -227
                    player.Character.HumanoidRootPart.CFrame = CFrame.new(-4445, 13, -222)
                    hasTeleported = true -- ตั้งค่าตัวแปรว่าได้วาบไปแล้ว
                elseif stoneCount > 0 then
                    hasTeleported = false -- ถ้ามี Stone ให้ตั้งค่าตัวแปรกลับ
                end
            end
            
            wait(0.5) -- ตรวจสอบทุก 0.5 วินาที
        end
    else
        running = false -- หยุดลูปเมื่อ Toggle ถูกปิด
    end
end)
local Toggle = Tabs.Vibranium:AddToggle("MyToggle", {Title = "Auto E", Default = false })

local VirtualInputManager = game:GetService("VirtualInputManager")
local isRunning = false -- ตัวแปรเพื่อบันทึกสถานะการทำงาน

Toggle:OnChanged(function(state)
    if state then
        isRunning = true
        while isRunning do
            -- จำลองการกดปุ่ม E
            VirtualInputManager:SendKeyEvent(true, Enum.KeyCode.E, false, game)
            wait(5) -- หน่วงเวลาสั้นๆ ก่อนปล่อยปุ่ม E
            -- จำลองการปล่อยปุ่ม E
            VirtualInputManager:SendKeyEvent(false, Enum.KeyCode.E, false, game)
            wait(1) -- รอ 5 วินาทีก่อนทำซ้ำ
        end
    else
        isRunning = false -- หยุดการทำงานเมื่อ Toggle ปิด
    end
end)
end
local Toggle = Tabs.Vibranium:AddToggle("MyToggle", { Title = "eat", Default = false })

Toggle:OnChanged(function()
    -- เช็คสถานะของ Toggle
    if Toggle.Value then
        while true do
            local player = game.Players.LocalPlayer
            local character = player.Character or player.CharacterAdded:Wait()
            local humanoidRootPart = character:WaitForChild("HumanoidRootPart")

            -- วาบไปยังตำแหน่งที่กำหนด
            humanoidRootPart.CFrame = CFrame.new(-3182, 34, 3242)

            -- ฟังก์ชันซื้อสินค้า
            local function buy(item, quantity)
                local args = { [1] = item, [2] = quantity }
                game:GetService("ReplicatedStorage"):WaitForChild("Remote"):WaitForChild("Shop"):FireServer(unpack(args))
            end

            -- ซื้อสินค้า
            buy("Tea", 10)
            buy("Water", 10)
            buy("Bread", 10)

            -- รอ 1 วินาที
            wait(1)

            -- วาบไปยังพิกัดใหม่
            humanoidRootPart.CFrame = CFrame.new(-4539, 38, -227)

            -- รอ 500 วินาที
            wait(250)
        end
    end
end)

local Toggle = Tabs.Vibranium:AddToggle("MyToggle", {Title = "pull food", Default = false })

Toggle:OnChanged(function(state)
    -- ถ้าเปิดการทำงาน Toggle
    while state do
        -- รายการไอเท็มที่ต้องการรับ
        local items = {"Water", "Tea", "Water", "Bread"}

        -- ลูปเพื่อส่งคำสั่งรับไอเท็มแต่ละชนิด
        for _, item in pairs(items) do
            local args = {
                [1] = "Get",
                [2] = item,
                [3] = 10
            }
            game:GetService("ReplicatedStorage"):WaitForChild("Remote"):WaitForChild("Vault"):FireServer(unpack(args))
        end

        -- รอ 500 วินาทีก่อนรันอีกครั้ง
        wait(500)
    end
end)
local Toggle = Tabs.Vibranium:AddToggle("MyToggle", {Title = "collect", Default = false})

local running = false  -- ตัวแปรสำหรับตรวจสอบสถานะการทำงาน

Toggle:OnChanged(function(state)
    if state then
        running = true
        while running do
            local materials = {
                {name = "Vibranium", amount = 10},
                {name = "Diamond", amount = 10},
                {name = "Gold", amount = 10},
                {name = "SteelBar", amount = 10} -- เพิ่ม Steel ที่นี่
            }

            for _, material in ipairs(materials) do
                local args = {
                    [1] = "Store",
                    [2] = material.name,
                    [3] = material.amount
                }

                game:GetService("ReplicatedStorage"):WaitForChild("Remote"):WaitForChild("Vault"):FireServer(unpack(args))
                wait(10)  -- รอ 10 วินาทีก่อนจะเก็บวัสดุถัดไป
            end
        end
    else
        running = false  -- เมื่อปิด Toggle จะหยุดการทำงาน
    end
end)
local Toggle = Tabs.Vibranium:AddToggle("MyToggle", {Title = "sell", Default = false })
local selling = false  -- ตัวแปรเพื่อตรวจสอบสถานะของ Toggle

local itemsToSell = {"Diamond", "Gold", "Vibranium", "Steel"}

local function sellItem(itemName)
    local args = {
        [1] = "Sell",
        [2] = itemName,
        [3] = "1"
    }
    game:GetService("ReplicatedStorage"):WaitForChild("Remotes"):WaitForChild("Economy"):FireServer(unpack(args))
end

Toggle:OnChanged(function(state)
    selling = state  -- อัปเดตสถานะ selling ตาม Toggle
    if selling then
        coroutine.wrap(function()
            while selling do
                for _, item in ipairs(itemsToSell) do
                    sellItem(item)
                end
                wait(5)
            end
        end)()  -- เริ่ม coroutine เพื่อรันการขายในแบ็คกราวด์
    end
end)

local Toggle = Tabs.Vibranium:AddToggle("MyToggle", {Title = "Craft SteelBar", Default = false })

local running = false -- ตัวแปรเพื่อตรวจสอบสถานะการทำงาน

Toggle:OnChanged(function(state)
    running = state -- ตั้งค่าให้ running เป็น true หรือ false ขึ้นอยู่กับสถานะของ toggle

    if running then
        while running do
            local args = {
                [1] = "Material",
                [2] = "SteelBar",
                [3] = 1
            }

            game:GetService("ReplicatedStorage"):WaitForChild("Remote"):WaitForChild("Craft"):FireServer(unpack(args))

            wait(5) -- รอ 5 วินาทีก่อนที่จะรันอีกครั้ง
        end
    end
end)

local Toggle = Tabs.Vibranium:AddToggle("MyToggle", {Title = "20 minutes", Default = false })

-- เก็บสถานะของการรันลูป
local running = false

Toggle:OnChanged(function(state)
    -- อัปเดตสถานะของการรันลูปตามค่า Toggle
    running = state
    
    -- ถ้า Toggle ถูกเปิดใช้งาน
    if running then
        -- สร้างการทำงานซ้ำด้วย RunService
        local RunService = game:GetService("RunService")
        local VirtualUser = game:GetService("VirtualUser")
        
        local connection
        connection = RunService.Heartbeat:Connect(function()
            if not running then
                connection:Disconnect()  -- ถ้า Toggle ถูกปิดให้หยุดการทำงาน
                return
            end
            -- จำลองการกดปุ่มเมาส์ขวา
            VirtualUser:CaptureController()
            VirtualUser:ClickButton2(Vector2.new())
            wait(5)  -- รอ 5 วินาทีก่อนรันครั้งถัดไป
        end)
    end
end)

local Toggle = Tabs.Main:AddToggle("MyToggle", {Title = "check", Default = false})

Toggle:OnChanged(function(state)
    local player = game.Players.LocalPlayer
    local alreadyTeleported = false -- ใช้ตัวแปรนี้เพื่อป้องกันการวาบซ้ำ

    -- ฟังก์ชันในการเช็คและวาบ
    local function checkAndTeleport()
        -- ตรวจสอบว่า Inventory และ Stone มีอยู่จริง
        if player:FindFirstChild("Inventory") and player.Inventory:FindFirstChild("Stone") then
            local inventory = player.Inventory
            local stoneAmount = inventory.Stone.Value -- เช็คค่าของ Stone
            
            if stoneAmount >= 60 and not alreadyTeleported then
                -- วาบไปที่ตำแหน่งที่กำหนดครั้งแรก
                player.Character.HumanoidRootPart.CFrame = CFrame.new(2898, 40, -843)
                alreadyTeleported = true -- วาบแล้วจะไม่วาบซ้ำ
                print("วาบครั้งแรก")
            end
            
            -- ถ้า Stone ลดลงให้รีเซ็ตสถานะการวาบใหม่
            if stoneAmount < 60 then
                alreadyTeleported = false -- รีเซ็ตเพื่อให้วาบใหม่เมื่อ Stone ถึง 60
                print("จำนวน Stone ไม่พอ, รอการวาบใหม่")
            end
        else
            print("ไม่พบ Inventory หรือ Stone ในผู้เล่น")
        end
    end

    -- ถ้า Toggle ถูกเปิดใช้งานให้ทำการเช็คต่อเนื่อง
    if state then
        while state do
            checkAndTeleport() -- เรียกฟังก์ชันเช็คและวาบ
            wait(2) -- รอ 2 วินาทีแล้วเช็คใหม่
        end
    end
end)



local Toggle = Tabs.Main:AddToggle("MyToggle", {Title = "check v1", Default = false })

Toggle:OnChanged(function(state) -- รับค่า state เพื่อตรวจสอบว่าถูกเปิดหรือปิด
    if state then -- เมื่อ Toggle ถูกเปิด
        local player = game.Players.LocalPlayer -- ใช้ LocalPlayer เพื่อเข้าถึงผู้เล่นที่รันสคริปต์

        -- ฟังก์ชันสำหรับตรวจสอบว่า Stone มีอยู่ใน Inventory หรือไม่
        local function hasNoStone()
            return not player.Inventory:FindFirstChild("Stone") or player.Inventory.Stone.Value == 0
        end

        -- วาบไปยังตำแหน่งที่กำหนด
        local function teleport()
            if hasNoStone() then
                player.Character.HumanoidRootPart.CFrame = CFrame.new(-4784, 33, 1391) -- วาบไปยังตำแหน่งแรก
                wait(10) -- รอ 10 วินาทีก่อนวาบไปยังตำแหน่งถัดไป
                player.Character.HumanoidRootPart.CFrame = CFrame.new(-4445, 13, -222) -- วาบไปยังตำแหน่งที่สอง
            end
        end

        -- ลูปเพื่อตรวจสอบ Stone อย่างต่อเนื่อง
        while state do -- ตรวจสอบ Toggle ตลอดว่าเปิดอยู่หรือไม่
            wait(1) -- รอ 1 วินาทีก่อนตรวจสอบอีกครั้ง
            teleport() -- เช็คและวาบถ้ามีเงื่อนไข
        end
    else
        -- หยุดการทำงานเมื่อ Toggle ถูกปิด
        print("Toggle ปิดแล้ว หยุดการตรวจสอบ")
    end
end)


local Toggle = Tabs.Main:AddToggle("MyToggle", {Title = "Craft", Default = false })
local isRunning = false -- ตัวแปรสำหรับเช็คสถานะการทำงานของลูป

Toggle:OnChanged(function(state)
    isRunning = state -- อัปเดตสถานะการทำงานของลูปตามค่า Toggle
    if state then
        while isRunning do
            -- สร้าง SteelBar
            local args = {
                [1] = "Material",
                [2] = "SteelBar",
                [3] = 1
            }

            game:GetService("ReplicatedStorage"):WaitForChild("Remote"):WaitForChild("Craft"):FireServer(unpack(args))
            wait(5) -- รอ 5 วินาที

            -- สร้าง SteelBar อีกครั้ง
            game:GetService("ReplicatedStorage"):WaitForChild("Remote"):WaitForChild("Craft"):FireServer(unpack(args))
            wait(5) -- รอ 5 วินาที

            -- สร้าง Bullet
            local args = {
                [1] = "Material",
                [2] = "Bullet",
                [3] = 1
            }

            game:GetService("ReplicatedStorage"):WaitForChild("Remote"):WaitForChild("Craft"):FireServer(unpack(args))
            wait(10) -- รอ 10 วินาทีก่อนที่จะเริ่มลูปใหม่
        end
    end
end)

local Toggle = Tabs.AirDrop:AddToggle("MyToggle", {Title = "Toggle", Default = false })

local running = false -- ตัวแปรเพื่อตรวจสอบสถานะของ Toggle

Toggle:OnChanged(function(state)
    running = state -- อัปเดตสถานะเมื่อ Toggle เปลี่ยน

    if running then
        while running do -- ทำซ้ำเมื่อ Toggle เปิดใช้งาน
            -- เคลื่อนที่ไปยังตำแหน่งเริ่มต้น

            game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(-3182, 34, 3234)

            wait(5)

            -- ฟังก์ชันซื้อสินค้า
            local function buy(item, quantity)
                local args = { [1] = item, [2] = quantity }
                game:GetService("ReplicatedStorage"):WaitForChild("Remote"):WaitForChild("Shop"):FireServer(unpack(args))
            end

            -- ซื้อสินค้า
            buy("Tea", 10)
            buy("Water", 10)
            buy("Bread", 10)

            -- รอ 1 วินาที
            wait(2.5)
            
            game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(-3838, 41, 1573)

            -- รอ 70 วินาที
            wait(70)

            -- วาบไปยังตำแหน่งใหม่
            game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(2975, 39, 102)

            -- รออีก 70 วินาที ก่อนลูปใหม่
            wait(70)
        end
    end
end)


local Toggle = Tabs.groblins:AddToggle("MyToggle", {Title = "groblins V1", Default = false })

-- Variable to control the loop
local running = false

Toggle:OnChanged(function(state)
    local player = game.Players.LocalPlayer
    local character = player.Character or player.CharacterAdded:Wait()

    if state then
        running = true
        while running do
            -- วาบไปที่พิกัดที่ระบุ
            character:SetPrimaryPartCFrame(CFrame.new(-2583, 36, 1775))

            -- รอ 0 วินาทีก่อนทำซ้ำ
            task.wait(0)
        end
    else
        -- Stop the loop when the toggle is off
        running = false
    end
end)


local Toggle = Tabs.groblins:AddToggle("MyToggle", {Title = "groblins V2", Default = false})

local isActive = false -- ตัวแปรเพื่อบอกสถานะของ Toggle
local groblinCoroutine -- ตัวแปรเพื่อเก็บ Coroutine

Toggle:OnChanged(function(value)
    isActive = value -- อัปเดตสถานะของ Toggle

    if isActive then
        -- เริ่ม Coroutine เมื่อ Toggle ถูกเปิด
        groblinCoroutine = coroutine.create(function()
            -- หาตัวผู้เล่น
            local player = game.Players.LocalPlayer
            local character = player.Character or player.CharacterAdded:Wait()

            -- เช็คว่ามี HumanoidRootPart ของผู้เล่นหรือไม่
            if character and character:FindFirstChild("HumanoidRootPart") then
                while isActive do
                    -- หาวัตถุทุกตัวใน Groblin
                    local groblins = workspace.Groblin:GetChildren()

                    -- วนผ่านทุกตัวของ Groblin
                    for _, groblin in ipairs(groblins) do
                        -- เช็คว่ามี HumanoidRootPart และ Humanoid ของ Groblin หรือไม่
                        if groblin:IsA("Model") and groblin:FindFirstChild("HumanoidRootPart") and groblin:FindFirstChild("Humanoid") then
                            -- ปรับ WalkSpeed ของ Humanoid เป็น 0 เพื่อไม่ให้เดินได้
                            groblin.Humanoid.WalkSpeed = 0

                            -- ตั้งตำแหน่งของ Groblin ให้ตรงกับผู้เล่น
                            groblin.HumanoidRootPart.CFrame = character.HumanoidRootPart.CFrame
                        end
                    end

                    -- รอ 10 วินาทีก่อนทำงานในรอบถัดไป
                    wait(10)
                end
            end
        end)

        -- เริ่ม Coroutine
        coroutine.resume(groblinCoroutine)
    else
        -- หยุด Coroutine เมื่อ Toggle ถูกปิด
        if groblinCoroutine then
            coroutine.yield(groblinCoroutine)
        end
    end
end)

Tabs.groblins:AddButton({
        Title = "groblins v3 Load map",
        Description = "",
        Callback = function()
            Window:Dialog({
                Title = "Title",
                Content = "This is a dialog",
                Buttons = {
                    {
                        Title = "Confirm",
                        Callback = function()
                            local player = game.Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()

-- Teleportation sequence
local teleportLocations = {
    CFrame.new(1229, 33, 5307),
    CFrame.new(684, 33, 4986),
    CFrame.new(220, 30, 5206),
    CFrame.new(389, 29, 5606),
    CFrame.new(892, 73, 5864),
    CFrame.new(721, 73, 5838),
    CFrame.new(1106, 81, 5662),
    CFrame.new(1286, 80, 5832),
    CFrame.new(1594, 73, 5725),
    CFrame.new(1910, 34, 5799),
    CFrame.new(2017, 72, 5244),
    CFrame.new(1981, 25, 4914)
}

for _, location in ipairs(teleportLocations) do
    character:SetPrimaryPartCFrame(location)
    wait(0.5)  -- Wait for 2 seconds between teleports
end
local player = game.Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()

-- Teleportation sequence
local teleportLocations = {
    CFrame.new(1229, 33, 5307),
    CFrame.new(684, 33, 4986),
    CFrame.new(220, 30, 5206),
    CFrame.new(389, 29, 5606),
    CFrame.new(892, 73, 5864),
    CFrame.new(721, 73, 5838),
    CFrame.new(1106, 81, 5662),
    CFrame.new(1286, 80, 5832),
    CFrame.new(1594, 73, 5725),
    CFrame.new(1910, 34, 5799),
    CFrame.new(2017, 72, 5244),
    CFrame.new(-2583, 36, 1775)
}

for _, location in ipairs(teleportLocations) do
    character:SetPrimaryPartCFrame(location)
    wait(0.5)  -- Wait for 2 seconds between teleports
end

                        end
                    },
                    {
                        Title = "Cancel",
                        Callback = function()
                             
                        end
                    }
                }
            })
        end
    })

local Toggle = Tabs.AirDrop:AddToggle("MyToggle", {Title = "Auto E", Default = false })

local VirtualInputManager = game:GetService("VirtualInputManager")
local isRunning = false -- ตัวแปรเพื่อบันทึกสถานะการทำงาน

Toggle:OnChanged(function(state)
    if state then
        isRunning = true
        while isRunning do
            -- จำลองการกดปุ่ม E
            VirtualInputManager:SendKeyEvent(true, Enum.KeyCode.E, false, game)
            wait(50) -- หน่วงเวลาสั้นๆ ก่อนปล่อยปุ่ม E
            -- จำลองการปล่อยปุ่ม E
            VirtualInputManager:SendKeyEvent(false, Enum.KeyCode.E, false, game)
            wait(2) -- รอ 5 วินาทีก่อนทำซ้ำ
        end
    else
        isRunning = false -- หยุดการทำงานเมื่อ Toggle ปิด
    end
end)


local Toggle = Tabs.Main:AddToggle("MyToggle", {Title = "collect", Default = false })

local running = false  -- ตัวแปรเพื่อควบคุมสถานะของลูป

Toggle:OnChanged(function(value)
    running = value  -- อัปเดตสถานะการทำงานตามค่าของ Toggle

    if running then
        -- เริ่มลูปเมื่อ Toggle ถูกเปิด
        while running do
            local args = {
                [1] = "Store",
                [2] = "Bullet",
                [3] = 1
            }

            game:GetService("ReplicatedStorage"):WaitForChild("Remote"):WaitForChild("Vault"):FireServer(unpack(args))

            wait(5) -- รอ 5 วินาทีก่อนรันใหม่อีกครั้ง
        end
    end
end)



local Toggle = Tabs.Main:AddToggle("MyToggle", {Title = "Show UI", Default = false })

Toggle:OnChanged(function()
    -- สร้าง ScreenGui
    local screenGui = Instance.new("ScreenGui")
    screenGui.Parent = game.Players.LocalPlayer:WaitForChild("PlayerGui")

    -- สร้าง Frame สำหรับจัดวางข้อมูล
    local frame = Instance.new("Frame")
    frame.Size = UDim2.new(0, 400, 0, 300) -- ปรับขนาดให้กว้างขึ้นและยาวขึ้น
    frame.Position = UDim2.new(0, 10, 0, 10) -- วางที่มุมซ้ายของจอ
    frame.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
    frame.Parent = screenGui

    -- สร้างปุ่ม X สำหรับปิด UI
    local closeButton = Instance.new("TextButton")
    closeButton.Size = UDim2.new(0, 30, 0, 30)
    closeButton.Position = UDim2.new(1, -35, 0, 5) -- วางปุ่มที่มุมขวาบนของ Frame
    closeButton.BackgroundColor3 = Color3.fromRGB(200, 0, 0)
    closeButton.TextColor3 = Color3.fromRGB(255, 255, 255)
    closeButton.TextSize = 18
    closeButton.Text = "X"
    closeButton.Parent = frame

    -- ฟังก์ชันเพื่อปิด UI
    closeButton.MouseButton1Click:Connect(function()
        screenGui:Destroy() -- ลบ ScreenGui เมื่อคลิกที่ปุ่ม X
    end)

    -- สร้าง TextLabel สำหรับ Diamond
    local diamondLabel = Instance.new("TextLabel")
    diamondLabel.Size = UDim2.new(0, 380, 0, 50) -- ปรับขนาดให้พอดีกับ Frame
    diamondLabel.Position = UDim2.new(0, 10, 0, 10)
    diamondLabel.BackgroundColor3 = Color3.fromRGB(100, 100, 100)
    diamondLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
    diamondLabel.TextSize = 24
    diamondLabel.Text = "Diamond: Loading..."
    diamondLabel.Parent = frame

    -- สร้าง TextLabel สำหรับ Gold
    local goldLabel = Instance.new("TextLabel")
    goldLabel.Size = UDim2.new(0, 380, 0, 50) -- ปรับขนาดให้พอดีกับ Frame
    goldLabel.Position = UDim2.new(0, 10, 0, 70)
    goldLabel.BackgroundColor3 = Color3.fromRGB(100, 100, 100)
    goldLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
    goldLabel.TextSize = 24
    goldLabel.Text = "Gold: Loading..."
    goldLabel.Parent = frame

    -- สร้าง TextLabel สำหรับ Steel
    local steelLabel = Instance.new("TextLabel")
    steelLabel.Size = UDim2.new(0, 380, 0, 50) -- ปรับขนาดให้พอดีกับ Frame
    steelLabel.Position = UDim2.new(0, 10, 0, 130)
    steelLabel.BackgroundColor3 = Color3.fromRGB(100, 100, 100)
    steelLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
    steelLabel.TextSize = 24
    steelLabel.Text = "Steel: Loading..."
    steelLabel.Parent = frame

    -- สร้าง TextLabel สำหรับ Vibranium
    local vibraniumLabel = Instance.new("TextLabel")
    vibraniumLabel.Size = UDim2.new(0, 380, 0, 50) -- ปรับขนาดให้พอดีกับ Frame
    vibraniumLabel.Position = UDim2.new(0, 10, 0, 190)
    vibraniumLabel.BackgroundColor3 = Color3.fromRGB(100, 100, 100)
    vibraniumLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
    vibraniumLabel.TextSize = 24
    vibraniumLabel.Text = "Vibranium: Loading..."
    vibraniumLabel.Parent = frame

    -- ฟังก์ชันอัปเดตค่าใน UI
    local function updateVaultValues()
        local playerVault = game:GetService("Players")["18y_033"].Vault
        diamondLabel.Text = "Diamond: " .. playerVault.Diamond.Value
        goldLabel.Text = "Gold: " .. playerVault.Gold.Value
        steelLabel.Text = "Steel: " .. playerVault.Steel.Value
        vibraniumLabel.Text = "Vibranium: " .. playerVault.Vibranium.Value
    end

    -- เรียกฟังก์ชันอัปเดตค่า
    updateVaultValues()

    -- หากต้องการให้ UI อัปเดตตลอดเวลา คุณสามารถใช้ Loop ได้
    while true do
        updateVaultValues()
        wait(5) -- อัปเดตทุกๆ 5 วินาที
    end
end)
