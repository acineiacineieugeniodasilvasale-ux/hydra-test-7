if not game:IsLoaded() then game.Loaded:Wait() end

local UserInputService = game:GetService("UserInputService")
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local LocalPlayer = Players.LocalPlayer
local Character = LocalPlayer.Character or LocalPlayer.CharacterAdded:Wait()

local PRIMARY_COLOR = Color3.fromRGB(30, 30, 30)
local SECONDARY_COLOR = Color3.fromRGB(60, 60, 60)
local ACCENT_COLOR = Color3.fromRGB(200, 200, 200)
local GREEN_COLOR = Color3.fromRGB(76, 175, 80)
local RED_COLOR = Color3.fromRGB(244, 67, 54)

local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "HydraHub"
ScreenGui.ResetOnSpawn = false

pcall(function()
    ScreenGui.Parent = LocalPlayer:WaitForChild("PlayerGui")
end)

local MainFrame = Instance.new("Frame")
MainFrame.Name = "MainFrame"
MainFrame.Size = UDim2.new(0, 350, 0, 500)
MainFrame.Position = UDim2.new(0.5, -175, 0.5, -250)
MainFrame.BackgroundColor3 = PRIMARY_COLOR
MainFrame.BorderSizePixel = 0
MainFrame.CornerRadius = UDim.new(0, 15)
MainFrame.Parent = ScreenGui

local TopBar = Instance.new("Frame")
TopBar.Name = "TopBar"
TopBar.Size = UDim2.new(1, 0, 0, 40)
TopBar.BackgroundColor3 = SECONDARY_COLOR
TopBar.BorderSizePixel = 0
TopBar.CornerRadius = UDim.new(0, 15)
TopBar.Parent = MainFrame

local TitleLabel = Instance.new("TextLabel")
TitleLabel.Name = "Title"
TitleLabel.Size = UDim2.new(0.7, 0, 1, 0)
TitleLabel.Position = UDim2.new(0.05, 0, 0, 0)
TitleLabel.BackgroundTransparency = 1
TitleLabel.Text = "HYDRA HUB"
TitleLabel.TextColor3 = ACCENT_COLOR
TitleLabel.TextScaled = true
TitleLabel.Font = Enum.Font.GothamBold
TitleLabel.Parent = TopBar

local MinimizeBtn = Instance.new("TextButton")
MinimizeBtn.Name = "MinimizeBtn"
MinimizeBtn.Size = UDim2.new(0, 35, 0, 35)
MinimizeBtn.Position = UDim2.new(1, -80, 0.5, -17.5)
MinimizeBtn.BackgroundColor3 = SECONDARY_COLOR
MinimizeBtn.TextColor3 = ACCENT_COLOR
MinimizeBtn.Text = "_"
MinimizeBtn.Font = Enum.Font.GothamBold
MinimizeBtn.TextScaled = true
MinimizeBtn.Parent = TopBar

local CloseBtn = Instance.new("TextButton")
CloseBtn.Name = "CloseBtn"
CloseBtn.Size = UDim2.new(0, 35, 0, 35)
CloseBtn.Position = UDim2.new(1, -40, 0.5, -17.5)
CloseBtn.BackgroundColor3 = RED_COLOR
CloseBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
CloseBtn.Text = "✕"
CloseBtn.Font = Enum.Font.GothamBold
CloseBtn.TextScaled = true
CloseBtn.Parent = TopBar

local ContentFrame = Instance.new("ScrollingFrame")
ContentFrame.Name = "ContentFrame"
ContentFrame.Size = UDim2.new(1, -10, 1, -50)
ContentFrame.Position = UDim2.new(0, 5, 0, 45)
ContentFrame.BackgroundTransparency = 1
ContentFrame.BorderSizePixel = 0
ContentFrame.ScrollBarThickness = 6
ContentFrame.ScrollBarImageColor3 = SECONDARY_COLOR
ContentFrame.Parent = MainFrame

local Features = {
    Fly = false,
    Speed = false,
    SuperJump = false,
    NoClip = false,
    ShowPlayerNames = false
}

local SpeedValue = 50
local JumpPowerValue = 50
local FlySpeed = 50

local function createToggleButton(parent, name, yPosition)
    local Container = Instance.new("Frame")
    Container.Name = name .. "Container"
    Container.Size = UDim2.new(1, -10, 0, 60)
    Container.Position = UDim2.new(0, 5, 0, yPosition)
    Container.BackgroundColor3 = SECONDARY_COLOR
    Container.BorderSizePixel = 0
    Container.CornerRadius = UDim.new(0, 8)
    Container.Parent = parent

    local Label = Instance.new("TextLabel")
    Label.Name = name .. "Label"
    Label.Size = UDim2.new(0.7, 0, 1, 0)
    Label.Position = UDim2.new(0.05, 0, 0, 0)
    Label.BackgroundTransparency = 1
    Label.Text = name
    Label.TextColor3 = ACCENT_COLOR
    Label.Font = Enum.Font.Gotham
    Label.TextScaled = true
    Label.Parent = Container

    local ToggleBtn = Instance.new("TextButton")
    ToggleBtn.Name = name .. "Toggle"
    ToggleBtn.Size = UDim2.new(0, 50, 0, 50)
    ToggleBtn.Position = UDim2.new(1, -60, 0.5, -25)
    ToggleBtn.BackgroundColor3 = RED_COLOR
    ToggleBtn.BorderSizePixel = 0
    ToggleBtn.CornerRadius = UDim.new(0, 10)
    ToggleBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
    ToggleBtn.Text = ""
    ToggleBtn.Font = Enum.Font.GothamBold
    ToggleBtn.Parent = Container

    return ToggleBtn, Container
end

local function createSpeedButton(parent, name, yPosition, onPlus, onMinus)
    local Container = Instance.new("Frame")
    Container.Name = name .. "Container"
    Container.Size = UDim2.new(1, -10, 0, 60)
    Container.Position = UDim2.new(0, 5, 0, yPosition)
    Container.BackgroundColor3 = SECONDARY_COLOR
    Container.BorderSizePixel = 0
    Container.CornerRadius = UDim.new(0, 8)
    Container.Parent = parent

    local Label = Instance.new("TextLabel")
    Label.Name = name .. "Label"
    Label.Size = UDim2.new(0.4, 0, 1, 0)
    Label.Position = UDim2.new(0.05, 0, 0, 0)
    Label.BackgroundTransparency = 1
    Label.Text = name
    Label.TextColor3 = ACCENT_COLOR
    Label.Font = Enum.Font.Gotham
    Label.TextScaled = true
    Label.Parent = Container

    local ValueLabel = Instance.new("TextLabel")
    ValueLabel.Name = name .. "Value"
    ValueLabel.Size = UDim2.new(0.2, 0, 1, 0)
    ValueLabel.Position = UDim2.new(0.45, 0, 0, 0)
    ValueLabel.BackgroundTransparency = 1
    ValueLabel.Text = "50"
    ValueLabel.TextColor3 = ACCENT_COLOR
    ValueLabel.Font = Enum.Font.Gotham
    ValueLabel.TextScaled = true
    ValueLabel.Parent = Container

    local MinusBtn = Instance.new("TextButton")
    MinusBtn.Name = name .. "Minus"
    MinusBtn.Size = UDim2.new(0, 35, 0, 35)
    MinusBtn.Position = UDim2.new(1, -95, 0.5, -17.5)
    MinusBtn.BackgroundColor3 = SECONDARY_COLOR
    MinusBtn.TextColor3 = ACCENT_COLOR
    MinusBtn.Text = "−"
    MinusBtn.Font = Enum.Font.GothamBold
    MinusBtn.TextScaled = true
    MinusBtn.Parent = Container

    local PlusBtn = Instance.new("TextButton")
    PlusBtn.Name = name .. "Plus"
    PlusBtn.Size = UDim2.new(0, 35, 0, 35)
    PlusBtn.Position = UDim2.new(1, -55, 0.5, -17.5)
    PlusBtn.BackgroundColor3 = SECONDARY_COLOR
    PlusBtn.TextColor3 = ACCENT_COLOR
    PlusBtn.Text = "+"
    PlusBtn.Font = Enum.Font.GothamBold
    PlusBtn.TextScaled = true
    PlusBtn.Parent = Container

    MinusBtn.MouseButton1Click:Connect(function()
        pcall(function()
            onMinus()
            ValueLabel.Text = tostring(math.max(0, tonumber(ValueLabel.Text) - 5))
        end)
    end)

    PlusBtn.MouseButton1Click:Connect(function()
        pcall(function()
            onPlus()
            ValueLabel.Text = tostring(math.min(200, tonumber(ValueLabel.Text) + 5))
        end)
    end)

    return MinusBtn, PlusBtn, Container, ValueLabel
end

local SectionLabel = Instance.new("TextLabel")
SectionLabel.Name = "SectionLabel"
SectionLabel.Size = UDim2.new(1, -10, 0, 30)
SectionLabel.Position = UDim2.new(0, 5, 0, 5)
SectionLabel.BackgroundTransparency = 1
SectionLabel.Text = "INICIAL"
SectionLabel.TextColor3 = ACCENT_COLOR
SectionLabel.Font = Enum.Font.GothamBold
SectionLabel.TextScaled = true
SectionLabel.Parent = ContentFrame

local FlyBtn, FlyContainer = createToggleButton(ContentFrame, "FLY", 40)
FlyBtn.MouseButton1Click:Connect(function()
    pcall(function()
        Features.Fly = not Features.Fly
        FlyBtn.BackgroundColor3 = Features.Fly and GREEN_COLOR or RED_COLOR
        if Features.Fly then
            local BodyVelocity = Instance.new("BodyVelocity")
            BodyVelocity.Velocity = Vector3.new(0, 0, 0)
            BodyVelocity.MaxForce = Vector3.new(math.huge, math.huge, math.huge)
            BodyVelocity.Parent = Character:FindFirstChild("HumanoidRootPart")

            local connection
            connection = RunService.RenderStepped:Connect(function()
                if not Features.Fly or not Character or not Character:FindFirstChild("Humanoid") then
                    pcall(function() BodyVelocity:Destroy() end)
                    pcall(function() connection:Disconnect() end)
                    return
                end

                local moveDirection = Vector3.new(0, 0, 0)
                if UserInputService:IsKeyDown(Enum.KeyCode.W) then moveDirection = moveDirection + (Character.HumanoidRootPart.CFrame.LookVector * FlySpeed) end
                if UserInputService:IsKeyDown(Enum.KeyCode.S) then moveDirection = moveDirection - (Character.HumanoidRootPart.CFrame.LookVector * FlySpeed) end
                if UserInputService:IsKeyDown(Enum.KeyCode.A) then moveDirection = moveDirection - (Character.HumanoidRootPart.CFrame.RightVector * FlySpeed) end
                if UserInputService:IsKeyDown(Enum.KeyCode.D) then moveDirection = moveDirection + (Character.HumanoidRootPart.CFrame.RightVector * FlySpeed) end
                if UserInputService:IsKeyDown(Enum.KeyCode.Space) then moveDirection = moveDirection + Vector3.new(0, FlySpeed, 0) end
                if UserInputService:IsKeyDown(Enum.KeyCode.LeftControl) then moveDirection = moveDirection - Vector3.new(0, FlySpeed, 0) end

                BodyVelocity.Velocity = moveDirection / 50
            end)
        end
    end)
end)

local SpeedMinusBtn, SpeedPlusBtn, SpeedContainer, SpeedValueLabel = createSpeedButton(
    ContentFrame,
    "VELOCIDADE",
    110,
    function() SpeedValue = math.min(200, SpeedValue + 5) end,
    function() SpeedValue = math.max(0, SpeedValue - 5) end
)

local JumpMinusBtn, JumpPlusBtn, JumpContainer, JumpValueLabel = createSpeedButton(
    ContentFrame,
    "SUPER PULO",
    180,
    function() JumpPowerValue = math.min(200, JumpPowerValue + 5) end,
    function() JumpPowerValue = math.max(0, JumpPowerValue - 5) end
)

local NoClipBtn, NoClipContainer = createToggleButton(ContentFrame, "NOCLIP", 250)
NoClipBtn.MouseButton1Click:Connect(function()
    pcall(function()
        Features.NoClip = not Features.NoClip
        NoClipBtn.BackgroundColor3 = Features.NoClip and GREEN_COLOR or RED_COLOR
        if Features.NoClip then
            local connection
            connection = RunService.Stepped:Connect(function()
                if not Features.NoClip or not Character then
                    pcall(function() connection:Disconnect() end)
                    return
                end
                for _, part in pairs(Character:GetDescendants()) do
                    if part:IsA("BasePart") then
                        part.CanCollide = false
                    end
                end
            end)
        else
            for _, part in pairs(Character:GetDescendants()) do
                if part:IsA("BasePart") then
                    part.CanCollide = true
                end
            end
        end
    end)
end)

local ShowNamesBtn, ShowNamesContainer = createToggleButton(ContentFrame, "NOMES", 320)
ShowNamesBtn.MouseButton1Click:Connect(function()
    pcall(function()
        Features.ShowPlayerNames = not Features.ShowPlayerNames
        ShowNamesBtn.BackgroundColor3 = Features.ShowPlayerNames and GREEN_COLOR or RED_COLOR
        if Features.ShowPlayerNames then
            for _, player in pairs(Players:GetPlayers()) do
                if player ~= LocalPlayer and player.Character then
                    local humanoidRootPart = player.Character:FindFirstChild("HumanoidRootPart")
                    if humanoidRootPart and not humanoidRootPart:FindFirstChild("NameTag") then
                        local billboardGui = Instance.new("BillboardGui")
                        billboardGui.Name = "NameTag"
                        billboardGui.Size = UDim2.new(4, 0, 2, 0)
                        billboardGui.StudsOffset = Vector3.new(0, 3, 0)
                        billboardGui.Parent = humanoidRootPart

                        local textLabel = Instance.new("TextLabel")
                        textLabel.BackgroundTransparency = 1
                        textLabel.Size = UDim2.new(1, 0, 1, 0)
                        textLabel.Text = player.Name
                        textLabel.TextColor3 = ACCENT_COLOR
                        textLabel.TextScaled = true
                        textLabel.Font = Enum.Font.GothamBold
                        textLabel.Parent = billboardGui
                    end
                end
            end
        else
            for _, player in pairs(Players:GetPlayers()) do
                if player.Character then
                    local humanoidRootPart = player.Character:FindFirstChild("HumanoidRootPart")
                    if humanoidRootPart then
                        local nameTag = humanoidRootPart:FindFirstChild("NameTag")
                        if nameTag then
                            nameTag:Destroy()
                        end
                    end
                end
            end
        end
    end)
end)

local CreditsSection = Instance.new("TextLabel")
CreditsSection.Name = "CreditsSection"
CreditsSection.Size = UDim2.new(1, -10, 0, 30)
CreditsSection.Position = UDim2.new(0, 5, 0, 390)
CreditsSection.BackgroundTransparency = 1
CreditsSection.Text = "CRÉDITOS"
CreditsSection.TextColor3 = ACCENT_COLOR
CreditsSection.Font = Enum.Font.GothamBold
CreditsSection.TextScaled = true
CreditsSection.Parent = ContentFrame

local CreditsText = Instance.new("TextLabel")
CreditsText.Name = "CreditsText"
CreditsText.Size = UDim2.new(1, -10, 0, 60)
CreditsText.Position = UDim2.new(0, 5, 0, 420)
CreditsText.BackgroundTransparency = 1
CreditsText.Text = "Criado por: HYDRA\nRoblox: espadachim_8899\nDiscord: Não possui"
CreditsText.TextColor3 = ACCENT_COLOR
CreditsText.Font = Enum.Font.Gotham
CreditsText.TextScaled = true
CreditsText.TextWrapped = true
CreditsText.Parent = ContentFrame

CloseBtn.MouseButton1Click:Connect(function()
    pcall(function()
        ScreenGui:Destroy()
    end)
end)

MinimizeBtn.MouseButton1Click:Connect(function()
    pcall(function()
        ContentFrame.Visible = not ContentFrame.Visible
    end)
end)

LocalPlayer.CharacterAdded:Connect(function(newCharacter)
    pcall(function()
        Character = newCharacter
        Features.Fly = false
        Features.Speed = false
        Features.SuperJump = false
        Features.NoClip = false
        Features.ShowPlayerNames = false
        FlyBtn.BackgroundColor3 = RED_COLOR
        NoClipBtn.BackgroundColor3 = RED_COLOR
        ShowNamesBtn.BackgroundColor3 = RED_COLOR
    end)
end)

RunService.RenderStepped:Connect(function()
    pcall(function()
        if Character and Character:FindFirstChild("Humanoid") then
            Character.Humanoid.WalkSpeed = 16 + (SpeedValue / 5)
        end
    end)
end)

pcall(function()
    Character.Humanoid.Jumping:Connect(function()
        if Character and Character:FindFirstChild("Humanoid") then
            Character.Humanoid.JumpPower = 50 + JumpPowerValue
        end
    end)
end)
