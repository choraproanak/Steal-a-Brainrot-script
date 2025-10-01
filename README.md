local player = game.Players.LocalPlayer
local uis = game:GetService("UserInputService")

-- HUB SETUP
local gui = Instance.new("ScreenGui", game.CoreGui)
gui.Name = "AnakBlockHub"

local frame = Instance.new("Frame", gui)
frame.Size = UDim2.new(0, 260, 0, 210)
frame.Position = UDim2.new(0, 40, 0, 100)
frame.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
frame.BorderSizePixel = 0
frame.Active = true
frame.Draggable = true

local corner = Instance.new("UICorner", frame)
corner.CornerRadius = UDim.new(0, 18)

local title = Instance.new("TextLabel", frame)
title.Text = "Steal a Brainrot"
title.Size = UDim2.new(1, 0, 0, 40)
title.Position = UDim2.new(0, 0, 0, 0)
title.BackgroundTransparency = 1
title.TextColor3 = Color3.fromRGB(255, 0, 0)
title.Font = Enum.Font.GothamBold
title.TextScaled = true

local blockBtn = Instance.new("TextButton", frame)
blockBtn.Size = UDim2.new(0.85, 0, 0, 58)
blockBtn.Position = UDim2.new(0.075, 0, 0, 60)
blockBtn.BackgroundColor3 = Color3.fromRGB(40, 255, 60)
blockBtn.TextColor3 = Color3.fromRGB(0, 0, 0)
blockBtn.Font = Enum.Font.GothamBold
blockBtn.TextScaled = true
blockBtn.Text = "Bloco: OFF (F)"
blockBtn.AutoButtonColor = true
local btncorner = Instance.new("UICorner", blockBtn)
btncorner.CornerRadius = UDim.new(0, 12)

local credit = Instance.new("TextLabel", frame)
credit.Text = "creditos: choraproanak"
credit.Size = UDim2.new(1, -20, 0, 30)
credit.Position = UDim2.new(0, 10, 0, 170)
credit.BackgroundTransparency = 1
credit.TextColor3 = Color3.fromRGB(255,255,255)
credit.Font = Enum.Font.GothamBold
credit.TextScaled = true

-- BLOCO VERDE COM AURA
local blockActive = false
local activeBlock, weld, bv

function spawnBlock()
    local character = player.Character
    if not character or not character:FindFirstChild("HumanoidRootPart") then return end

    activeBlock = Instance.new("Part")
    activeBlock.Size = Vector3.new(4, 1, 4)
    activeBlock.Anchored = false
    activeBlock.CanCollide = true
    activeBlock.Position = character.HumanoidRootPart.Position - Vector3.new(0, 3, 0)
    activeBlock.Color = Color3.fromRGB(50, 255, 60)
    activeBlock.Name = "GreenLiftBlock"
    activeBlock.Parent = workspace

    local highlight = Instance.new("Highlight")
    highlight.FillColor = Color3.new(0, 1, 0)
    highlight.OutlineColor = Color3.new(0, 1, 0)
    highlight.FillTransparency = 0.5
    highlight.OutlineTransparency = 0.1
    highlight.Adornee = activeBlock
    highlight.Parent = activeBlock

    bv = Instance.new("BodyVelocity")
    bv.MaxForce = Vector3.new(0, math.huge, 0)
    bv.Velocity = Vector3.new(0, 12, 0) -- velocidade reduzida
    bv.Parent = activeBlock

    weld = Instance.new("WeldConstraint")
    weld.Part0 = activeBlock
    weld.Part1 = character.HumanoidRootPart
    weld.Parent = activeBlock
end

function removeBlock()
    if weld then weld:Destroy() weld = nil end
    if bv then bv:Destroy() bv = nil end
    if activeBlock then activeBlock:Destroy() activeBlock = nil end
end

function toggleBlock()
    blockActive = not blockActive
    if blockActive then
        spawnBlock()
        blockBtn.Text = "Bloco: ON (F)"
        blockBtn.BackgroundColor3 = Color3.fromRGB(0, 255, 0)
    else
        removeBlock()
        blockBtn.Text = "Bloco: OFF (F)"
        blockBtn.BackgroundColor3 = Color3.fromRGB(40, 255, 60)
    end
end

blockBtn.MouseButton1Click:Connect(toggleBlock)

uis.InputBegan:Connect(function(input, gp)
    if gp then return end
    if input.KeyCode == Enum.KeyCode.F then
        toggleBlock()
    end
end)
