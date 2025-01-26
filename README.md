# tuahei-HUB
--// Services
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")

--// Variables
local player = Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "ตัวเห้ย HUB"
screenGui.Parent = player:WaitForChild("PlayerGui")

--// Main Frame
local frame = Instance.new("Frame")
frame.Size = UDim2.new(0, 300, 0, 400)
frame.Position = UDim2.new(0.5, -150, 0.5, -200)
frame.BackgroundColor3 = Color3.new(0.1, 0.1, 0.1)
frame.BorderSizePixel = 0
frame.AnchorPoint = Vector2.new(0.5, 0.5)
frame.Parent = screenGui

local uiCorner = Instance.new("UICorner")
uiCorner.CornerRadius = UDim.new(0, 10)
uiCorner.Parent = frame

-- Title
local title = Instance.new("TextLabel")
title.Size = UDim2.new(0, 300, 0, 50)
title.Position = UDim2.new(0, 0, 0, 0)
title.BackgroundTransparency = 1
title.Text = "ตัวเห้ย HUB"
title.Font = Enum.Font.FredokaOne
title.TextSize = 30
title.TextColor3 = Color3.new(1, 1, 1)
title.TextScaled = true
title.Parent = frame

-- Players List Container
local playersListFrame = Instance.new("Frame")
playersListFrame.Size = UDim2.new(1, 0, 0.7, 0)
playersListFrame.Position = UDim2.new(0, 0, 0.15, 0)
playersListFrame.BackgroundTransparency = 1
playersListFrame.Parent = frame

-- ScrollFrame for Player List
local scrollFrame = Instance.new("ScrollingFrame")
scrollFrame.Size = UDim2.new(1, 0, 1, 0)
scrollFrame.CanvasSize = UDim2.new(0, 0, 0, 0)
scrollFrame.ScrollBarThickness = 8
scrollFrame.BackgroundTransparency = 1
scrollFrame.Parent = playersListFrame

local uiListLayout = Instance.new("UIListLayout")
uiListLayout.SortOrder = Enum.SortOrder.LayoutOrder
uiListLayout.Padding = UDim.new(0, 5)
uiListLayout.Parent = scrollFrame

-- Function to update the player list
local function updatePlayerList()
    -- Clear the existing list
    for _, child in ipairs(scrollFrame:GetChildren()) do
        if child:IsA("TextButton") then
            child:Destroy()
        end
    end

    -- Add buttons for each player
    for _, otherPlayer in pairs(Players:GetPlayers()) do
        if otherPlayer ~= player then
            local playerButton = Instance.new("TextButton")
            playerButton.Size = UDim2.new(1, 0, 0, 30)
            playerButton.BackgroundColor3 = Color3.new(0.2, 0.2, 0.2)
            playerButton.Text = otherPlayer.Name
            playerButton.TextColor3 = Color3.new(1, 1, 1)
            playerButton.Font = Enum.Font.SourceSans
            playerButton.TextSize = 20
            playerButton.Parent = scrollFrame

            -- Action when clicking on a player's name
            playerButton.MouseButton1Click:Connect(function()
                if otherPlayer.Character and otherPlayer.Character:FindFirstChild("HumanoidRootPart") then
                    local targetPosition = otherPlayer.Character.HumanoidRootPart.Position
                    character:SetPrimaryPartCFrame(CFrame.new(targetPosition))  -- Warp to the selected player
                end
            end)
        end
    end

    -- Update the canvas size of the scroll frame
    scrollFrame.CanvasSize = UDim2.new(0, 0, 0, #scrollFrame:GetChildren() * 35)
end

-- Update the player list when a player joins or leaves
Players.PlayerAdded:Connect(updatePlayerList)
Players.PlayerRemoving:Connect(updatePlayerList)

-- Update the list initially
updatePlayerList()

