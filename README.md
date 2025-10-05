local Player = game.Players.LocalPlayer
local UserInputService = game:GetService("UserInputService")
local RunService = game:GetService("RunService")

-- ScreenGui
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "ChlisHub"
ScreenGui.ResetOnSpawn = false
ScreenGui.Parent = Player:WaitForChild("PlayerGui")

-- Frame
local Frame = Instance.new("Frame")
Frame.Size = UDim2.new(0, 200, 0, 150)
Frame.Position = UDim2.new(0, 50, 0, 50)
Frame.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
Frame.Parent = ScreenGui

-- Título
local Title = Instance.new("TextLabel")
Title.Text = "Chlis"
Title.Size = UDim2.new(1, 0, 0, 30)
Title.BackgroundColor3 = Color3.fromRGB(70, 70, 70)
Title.TextColor3 = Color3.new(1, 1, 1)
Title.Parent = Frame

-- Botón Fly
local FlyButton = Instance.new("TextButton")
FlyButton.Text = "Fly"
FlyButton.Size = UDim2.new(0, 100, 0, 30)
FlyButton.Position = UDim2.new(0, 50, 0, 50)
FlyButton.BackgroundColor3 = Color3.fromRGB(100, 100, 100)
FlyButton.TextColor3 = Color3.new(1, 1, 1)
FlyButton.Parent = Frame

-- Variables de vuelo
local flying = false
local flySpeed = 50
local BodyVelocity

-- Función Fly
local function toggleFly()
    local Character = Player.Character or Player.CharacterAdded:Wait()
    local HumanoidRootPart = Character:FindFirstChild("HumanoidRootPart")
    local Humanoid = Character:FindFirstChildOfClass("Humanoid")
    if not HumanoidRootPart or not Humanoid then return end

    flying = not flying
    if flying then
        BodyVelocity = Instance.new("BodyVelocity")
        BodyVelocity.MaxForce = Vector3.new(400000,400000,400000)
        BodyVelocity.Velocity = Vector3.new(0,0,0)
        BodyVelocity.Parent = HumanoidRootPart
        Humanoid.PlatformStand = true
        FlyButton.Text = "Stop Flying"
    else
        if BodyVelocity then BodyVelocity:Destroy() end
        Humanoid.PlatformStand = false
        FlyButton.Text = "Fly"
    end
end

-- Movimiento Fly
RunService.RenderStepped:Connect(function()
    if flying and BodyVelocity then
        local Character = Player.Character
        if not Character then return end
        local HumanoidRootPart = Character:FindFirstChild("HumanoidRootPart")
        if not HumanoidRootPart then return end

        local direction = Vector3.new()
        if UserInputService:IsKeyDown(Enum.KeyCode.W) then
            direction = direction + (HumanoidRootPart.CFrame.LookVector * flySpeed)
        end
        if UserInputService:IsKeyDown(Enum.KeyCode.S) then
            direction = direction - (HumanoidRootPart.CFrame.LookVector * flySpeed)
        end
        if UserInputService:IsKeyDown(Enum.KeyCode.A) then
            direction = direction - (HumanoidRootPart.CFrame.RightVector * flySpeed)
        end
        if UserInputService:IsKeyDown(Enum.KeyCode.D) then
            direction = direction + (HumanoidRootPart.CFrame.RightVector * flySpeed)
        end
        BodyVelocity.Velocity = direction + Vector3.new(0,0,0)
    end
end)

-- Botón
FlyButton.MouseButton1Click:Connect(toggleFly)
