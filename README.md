local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local Mouse = LocalPlayer:GetMouse()

-- Criar GUI
local ScreenGui = Instance.new("ScreenGui", LocalPlayer:WaitForChild("PlayerGui"))
ScreenGui.Name = "GOD_HUB"
local Frame = Instance.new("Frame", ScreenGui)
Frame.Size = UDim2.new(0, 300, 0, 400)
Frame.Position = UDim2.new(0.5, -150, 0.5, -200)
Frame.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
Frame.Visible = true

local UICorner = Instance.new("UICorner", Frame)
UICorner.CornerRadius = UDim.new(0, 8)

local UIListLayout = Instance.new("UIListLayout", Frame)
UIListLayout.Padding = UDim.new(0, 10)

-- Título
local Title = Instance.new("TextLabel", Frame)
Title.Text = "🔥 GOD HUB UNIVERSAL 🔥"
Title.Size = UDim2.new(1, 0, 0, 40)
Title.BackgroundTransparency = 1
Title.TextColor3 = Color3.fromRGB(255, 255, 255)
Title.Font = Enum.Font.GothamBold
Title.TextScaled = true

-- Botão: Fly
local FlyButton = Instance.new("TextButton", Frame)
FlyButton.Text = "✈️ Ativar Fly (F)"
FlyButton.Size = UDim2.new(1, -20, 0, 40)
FlyButton.Position = UDim2.new(0, 10, 0, 50)
FlyButton.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
FlyButton.TextColor3 = Color3.fromRGB(255, 255, 255)
FlyButton.Font = Enum.Font.GothamBold
FlyButton.TextScaled = true
Instance.new("UICorner", FlyButton)

-- Botão: Noclip
local NoclipButton = FlyButton:Clone()
NoclipButton.Text = "🧱 Ativar Noclip (G)"
NoclipButton.Parent = Frame

-- Botão: God Mode
local GodButton = FlyButton:Clone()
GodButton.Text = "💀 God Mode"
GodButton.Parent = Frame

-- Botão: ESP
local ESPButton = FlyButton:Clone()
ESPButton.Text = "🔍 Ativar ESP"
ESPButton.Parent = Frame

-- Fly
FlyButton.MouseButton1Click:Connect(function()
    loadstring([[
        local p = game.Players.LocalPlayer
        local mouse = p:GetMouse()
        local flying = false
        local torso = p.Character:FindFirstChild("HumanoidRootPart")
        local speed = 2
        local bg = Instance.new("BodyGyro", torso)
        local bv = Instance.new("BodyVelocity", torso)
        bg.P = 9e4
        bg.maxTorque = Vector3.new(9e9, 9e9, 9e9)
        bg.cframe = torso.CFrame
        bv.velocity = Vector3.new(0, 0.1, 0)
        bv.maxForce = Vector3.new(9e9, 9e9, 9e9)

        mouse.KeyDown:Connect(function(key)
            if key == "f" then
                flying = not flying
                if not flying then
                    bg:Destroy()
                    bv:Destroy()
                end
            end
        end)

        game:GetService("RunService").Stepped:Connect(function()
            if flying then
                bg.cframe = workspace.CurrentCamera.CFrame
                local direction = Vector3.new()
                if mouse:IsKeyDown("w") then direction = direction + workspace.CurrentCamera.CFrame.LookVector end
                if mouse:IsKeyDown("s") then direction = direction - workspace.CurrentCamera.CFrame.LookVector end
                if mouse:IsKeyDown("a") then direction = direction - workspace.CurrentCamera.CFrame.RightVector end
                if mouse:IsKeyDown("d") then direction = direction + workspace.CurrentCamera.CFrame.RightVector end
                bv.velocity = direction.unit * speed
            end
        end)
    ]])()
end)

-- Noclip
NoclipButton.MouseButton1Click:Connect(function()
    loadstring([[
        local noclip = false
        game:GetService('RunService').Stepped:Connect(function()
            if noclip then
                for _, part in pairs(game.Players.LocalPlayer.Character:GetDescendants()) do
                    if part:IsA("BasePart") then
                        part.CanCollide = false
                    end
                end
            end
        end)
        local mouse = game.Players.LocalPlayer:GetMouse()
        mouse.KeyDown:Connect(function(k)
            if k == "g" then
                noclip = not noclip
            end
        end)
    ]])()
end)

-- God Mode
GodButton.MouseButton1Click:Connect(function()
    local char = LocalPlayer.Character
    if char then
        char.Humanoid.Name = "Humanoid_God"
        local newHum = char.Humanoid_God:Clone()
        newHum.Name = "Humanoid"
        newHum.Parent = char
        wait(0.1)
        char.Humanoid_God:Destroy()
        workspace.CurrentCamera.CameraSubject = char:FindFirstChild("Humanoid")
    end
end)

-- ESP
ESPButton.MouseButton1Click:Connect(function()
    for _, p in pairs(Players:GetPlayers()) do
        if p ~= LocalPlayer then
            local esp = Instance.new("BillboardGui", p.Character:WaitForChild("Head"))
            esp.Size = UDim2.new(0, 100, 0, 40)
            esp.AlwaysOnTop = true
            local name = Instance.new("TextLabel", esp)
            name.Text = p.Name
            name.Size = UDim2.new(1, 0, 1, 0)
            name.TextColor3 = Color3.new(1, 0, 0)
            name.BackgroundTransparency = 1
            name.Font = Enum.Font.GothamBold
            name.TextScaled = true
        end
    end
end)
