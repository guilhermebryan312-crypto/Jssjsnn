repeat wait() until game:IsLoaded()

local player = game.Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()

-- Auto Missão
local missionZone = workspace:WaitForChild("MissionZone")

missionZone.Touched:Connect(function(hit)
    if hit.Parent == character then
        print("Missão iniciada!")
        -- Ativar missão aqui
    end
end)

-- Coleta Automática de Frutas
local fruitFolder = workspace:WaitForChild("Fruits")

fruitFolder.ChildAdded:Connect(function(fruit)
    local distance = (fruit.Position - character.HumanoidRootPart.Position).Magnitude
    if distance < 20 then
        fruit:Destroy()
        print("Fruta coletada!")
        -- Adicionar ao inventário
    end
end)

-- Radar (ESP) para Frutas
local function createESP(target)
    local esp = Instance.new("BillboardGui", target)
    esp.Size = UDim2.new(0, 100, 0, 50)
    esp.Adornee = target
    esp.AlwaysOnTop = true

    local label = Instance.new("TextLabel", esp)
    label.Size = UDim2.new(1, 0, 1, 0)
    label.Text = "Fruta"
    label.BackgroundTransparency = 1
    label.TextColor3 = Color3.new(1, 0, 0)
    label.TextScaled = true
end

for _, fruit in pairs(fruitFolder:GetChildren()) do
    createESP(fruit)
end

fruitFolder.ChildAdded:Connect(function(fruit)
    createESP(fruit)
end)
