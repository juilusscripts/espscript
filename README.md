-- Julius a God GUI

-- Instances:

local ScreenGui = Instance.new("ScreenGui")
local MainFrame = Instance.new("Frame")
local Header = Instance.new("Frame")
local TitleLabel = Instance.new("TextLabel")
local ESPButton = Instance.new("TextButton")

-- Properties:

ScreenGui.Name = "JuliusAGodGUI"
ScreenGui.Parent = game.Players.LocalPlayer:WaitForChild("PlayerGui")
ScreenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
ScreenGui.ResetOnSpawn = false

MainFrame.Name = "MainFrame"
MainFrame.Parent = ScreenGui
MainFrame.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
MainFrame.BorderSizePixel = 0
MainFrame.Position = UDim2.new(0.35, 0, 0.3, 0)
MainFrame.Size = UDim2.new(0, 250, 0, 200)
MainFrame.ClipsDescendants = true
MainFrame.BackgroundTransparency = 0.1

Header.Name = "Header"
Header.Parent = MainFrame
Header.BackgroundColor3 = Color3.fromRGB(0, 102, 204)
Header.BorderSizePixel = 0
Header.Size = UDim2.new(1, 0, 0, 40)

TitleLabel.Name = "TitleLabel"
TitleLabel.Parent = Header
TitleLabel.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
TitleLabel.BackgroundTransparency = 1
TitleLabel.Size = UDim2.new(1, -20, 1, 0)
TitleLabel.Position = UDim2.new(0, 10, 0, 0)
TitleLabel.Font = Enum.Font.GothamBold
TitleLabel.Text = "Julius a God"
TitleLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
TitleLabel.TextSize = 20
TitleLabel.TextXAlignment = Enum.TextXAlignment.Left

ESPButton.Name = "ESPButton"
ESPButton.Parent = MainFrame
ESPButton.BackgroundColor3 = Color3.fromRGB(0, 204, 102)
ESPButton.BorderSizePixel = 0
ESPButton.Position = UDim2.new(0.2, 0, 0.5, 0)
ESPButton.Size = UDim2.new(0.6, 0, 0.2, 0)
ESPButton.Font = Enum.Font.GothamBold
ESPButton.Text = "ESP OFF"
ESPButton.TextColor3 = Color3.fromRGB(255, 255, 255)
ESPButton.TextSize = 18

-- ESP Script:

local espEnabled = false

ESPButton.MouseButton1Click:Connect(function()
    espEnabled = not espEnabled
    if espEnabled then
        ESPButton.Text = "ESP ON"
        ESPButton.BackgroundColor3 = Color3.fromRGB(255, 50, 50)
        enableESP()
        print("ESP Activated!")
    else
        ESPButton.Text = "ESP OFF"
        ESPButton.BackgroundColor3 = Color3.fromRGB(0, 204, 102)
        disableESP()
        print("ESP Deactivated!")
    end
end)

function createESP(player)
    local character = player.Character
    if character then
        local highlight = Instance.new("Highlight")
        highlight.Name = "ESPHighlight"
        highlight.Adornee = character
        highlight.Parent = character
        highlight.FillColor = Color3.fromRGB(255, 50, 50)
        highlight.OutlineTransparency = 0.7
    end
end

function removeESP(player)
    local character = player.Character
    if character and character:FindFirstChild("ESPHighlight") then
        character.ESPHighlight:Destroy()
    end
end

function enableESP()
    for _, player in pairs(game.Players:GetPlayers()) do
        if player ~= game.Players.LocalPlayer then
            createESP(player)
        end
    end

    game.Players.PlayerAdded:Connect(function(player)
        player.CharacterAdded:Connect(function()
            createESP(player)
        end)
    end)

    game.Players.PlayerRemoving:Connect(function(player)
        removeESP(player)
    end)
end

function disableESP()
    for _, player in pairs(game.Players:GetPlayers()) do
        removeESP(player)
    end
end
