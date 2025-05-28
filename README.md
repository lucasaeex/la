local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer

function createESP(player)
    if player == LocalPlayer then return end
    if not player.Character then return end

    local head = player.Character:FindFirstChild("Head")
    if not head then return end

    if head:FindFirstChild("ESP") then return end -- já existe

    local billboard = Instance.new("BillboardGui")
    billboard.Name = "ESP"
    billboard.Adornee = head
    billboard.Size = UDim2.new(0, 100, 0, 40)
    billboard.StudsOffset = Vector3.new(0, 2, 0)
    billboard.AlwaysOnTop = true

    local label = Instance.new("TextLabel")
    label.Size = UDim2.new(1, 0, 1, 0)
    label.Text = player.Name
    label.BackgroundTransparency = 1
    label.TextColor3 = Color3.new(1, 0, 0)
    label.TextStrokeTransparency = 0.5
    label.Font = Enum.Font.SourceSansBold
    label.TextScaled = true
    label.Parent = billboard

    billboard.Parent = head
end

-- Adiciona para todos os jogadores
for _, player in ipairs(Players:GetPlayers()) do
    if player ~= LocalPlayer then
        player.CharacterAdded:Connect(function()
            wait(1) -- espera o personagem carregar
            createESP(player)
        end)
        if player.Character then
            createESP(player)
        end
    end
end

-- Novos jogadores
Players.PlayerAdded:Connect(function(player)
    player.CharacterAdded:Connect(function()
        wait(1)
        createESP(player)
    end)
end)
