-- ✅ Grow a Garden: Auto Buy Plant with UI (Smart Stock Check)
-- 🧠 by พี่ชาย GPT

--== SETUP ==--
local Players = game:GetService("Players")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local Workspace = game:GetService("Workspace")
local player = Players.LocalPlayer

local screenGui = Instance.new("ScreenGui", player:WaitForChild("PlayerGui"))
screenGui.Name = "AutoBuyUI"

local frame = Instance.new("Frame", screenGui)
frame.Size = UDim2.new(0, 280, 0, 300)
frame.Position = UDim2.new(0, 20, 0.5, -150)
frame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
frame.BorderSizePixel = 0
frame.Name = "AutoBuyFrame"

local uiList = Instance.new("UIListLayout", frame)
uiList.Padding = UDim.new(0, 5)
uiList.FillDirection = Enum.FillDirection.Vertical
uiList.HorizontalAlignment = Enum.HorizontalAlignment.Center
uiList.VerticalAlignment = Enum.VerticalAlignment.Top

--== DATA ==--
local toggles = {} -- เก็บว่าให้ซื้อ seed อะไรบ้าง
local shopFolder = Workspace:WaitForChild("Shops"):WaitForChild("SeedShop"):WaitForChild("Stock")
local buyRemote = ReplicatedStorage:WaitForChild("Remotes"):WaitForChild("BuyItem")

--== CREATE BUTTON FROM STOCK ==--
local function refreshUI()
    for _, c in pairs(frame:GetChildren()) do
        if c:IsA("TextButton") then c:Destroy() end
    end
    for _, item in ipairs(shopFolder:GetChildren()) do
        local name = item.Name
        if not toggles[name] then toggles[name] = false end

        local btn = Instance.new("TextButton", frame)
        btn.Size = UDim2.new(0, 250, 0, 30)
        btn.Text = (toggles[name] and "✅ " or "❌ ") .. name
        btn.BackgroundColor3 = toggles[name] and Color3.fromRGB(60, 100, 60) or Color3.fromRGB(60, 60, 60)
        btn.TextColor3 = Color3.new(1,1,1)
        btn.Name = name

        btn.MouseButton1Click:Connect(function()
            toggles[name] = not toggles[name]
            btn.Text = (toggles[name] and "✅ " or "❌ ") .. name
            btn.BackgroundColor3 = toggles[name] and Color3.fromRGB(60, 100, 60) or Color3.fromRGB(60, 60, 60)
        end)
    end
end

--== UI REFRESH ON START ==--
refreshUI()

--== SHOP UPDATE LISTENER (OPTIONAL IF SERVER UPDATES) ==--
shopFolder.ChildAdded:Connect(refreshUI)
shopFolder.ChildRemoved:Connect(refreshUI)

--== BUY LOOP ==--
while true do
    for _, item in ipairs(shopFolder:GetChildren()) do
        local itemName = item.Name
        local stockValue = item:FindFirstChild("Stock")
        if toggles[itemName] and stockValue and stockValue.Value > 0 then
            pcall(function()
                buyRemote:FireServer(item)
                print("🪴 Auto ซื้อ:", itemName)
            end)
        end
    end
    task.wait(2)
end
