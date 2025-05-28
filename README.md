-- ESP Simples com Team Check e Drawing API
local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local RunService = game:GetService("RunService")

local ESPs = {}

function CreateESP(player)
    if player == LocalPlayer then return end
    if ESPs[player] then return end

    local box = Drawing.new("Text")
    box.Size = 13
    box.Center = true
    box.Outline = true
    box.Color = Color3.fromRGB(255, 0, 0)
    box.Visible = false
    box.Font = 2
    box.Text = ""

    ESPs[player] = box

    local function Update()
        if not player.Character or not player.Character:FindFirstChild("HumanoidRootPart") or not player.Character:FindFirstChild("Humanoid") then
            box.Visible = false
            return
        end

        if player.Team == LocalPlayer.Team then
            box.Visible = false
            return
        end

        local pos, onScreen = workspace.CurrentCamera:WorldToViewportPoint(player.Character.HumanoidRootPart.Position)
        if onScreen then
            box.Position = Vector2.new(pos.X, pos.Y - 25)
            box.Text = player.Name .. " [" .. math.floor(player.Character.Humanoid.Health) .. "]"
            box.Visible = true
        else
            box.Visible = false
        end
    end

    ESPs[player .. "_conn"] = RunService.Render
