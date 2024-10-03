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
    Main = Window:AddTab({ Title = "Main", Icon = "" }),
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

local Toggle = Tabs.Main:AddToggle("MyToggle", {Title = "Toggle", Default = false })

local player = game:GetService("Players")["povertylive916"]
local targetPosition = Vector3.new(2898, 40, -843)
local checkInventoryConnection -- ตัวแปรเพื่อเก็บการเชื่อมต่อ

-- ฟังก์ชันสำหรับการเช็ค Inventory
local function checkInventory()
    -- เช็คว่า Inventory มี Stone หรือไม่
    if player.Inventory:FindFirstChild("Stone") then
        local stoneCount = player.Inventory.Stone.Value

        -- ถ้ามี Stone 60 หน่วย
        if stoneCount >= 60 then
            -- วาร์ปไปยังตำแหน่งที่ต้องการ
            player.Character.HumanoidRootPart.CFrame = CFrame.new(targetPosition)
        end
    end
end

Toggle:OnChanged(function(isEnabled)
    if isEnabled then
        -- เริ่มต้นการตรวจสอบทุกๆ 5 วินาที
        checkInventoryConnection = game:GetService("RunService").Heartbeat:Connect(function()
            checkInventory()
            wait(5) -- รอ 5 วินาทีก่อนทำซ้ำ
        end)
    else
        -- หยุดการตรวจสอบเมื่อ Toggle ถูกปิด
        if checkInventoryConnection then
            checkInventoryConnection:Disconnect()
            checkInventoryConnection = nil
        end
    end
end)


local Toggle = Tabs.Main:AddToggle("MyToggle", {Title = "check stone v2", Default = false })

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
                    player.Character.HumanoidRootPart.CFrame = CFrame.new(-4539, 38, -227)
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
local Toggle = Tabs.Main:AddToggle("MyToggle", {Title = "Auto E", Default = false })

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
            wait(5) -- รอ 5 วินาทีก่อนทำซ้ำ
        end
    else
        isRunning = false -- หยุดการทำงานเมื่อ Toggle ปิด
    end
end)
end
local Toggle = Tabs.Main:AddToggle("MyToggle", { Title = "eat", Default = false })

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

local Toggle = Tabs.Main:AddToggle("MyToggle", {Title = "Toggle", Default = false })

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
local Toggle = Tabs.Main:AddToggle("MyToggle", {Title = "Toggle", Default = false})

local running = false  -- ตัวแปรสำหรับตรวจสอบสถานะการทำงาน

Toggle:OnChanged(function(state)
    if state then
        running = true
        while running do
            local materials = {
                {name = "Vibranium", amount = 10},
                {name = "Diamond", amount = 10},
                {name = "Gold", amount = 10},
                {name = "Steel", amount = 10}  -- เพิ่ม Steel ที่นี่
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
local Toggle = Tabs.Main:AddToggle("MyToggle", {Title = "Toggle", Default = false })
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
