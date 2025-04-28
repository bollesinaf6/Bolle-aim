local aimPart = "Head" 
local aimKey = Enum.KeyCode.E 
local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local Mouse = LocalPlayer:GetMouse()
local RunService = game:GetService("RunService")
local holding = false
local function getClosestPlayer()
    local closestPlayer = nil
    local shortestDistance = math.huge

    for _, player in pairs(Players:GetPlayers()) do
        if player ~= LocalPlayer and player.Character and player.Character:FindFirstChild(aimPart) then
            local pos = player.Character[aimPart].Position
            local screenPos, visible = workspace.CurrentCamera:WorldToViewportPoint(pos)

            if visible then
                local distance = (Vector2.new(screenPos.X, screenPos.Y) - Vector2.new(Mouse.X, Mouse.Y)).magnitude
                if distance < shortestDistance then
                    shortestDistance = distance
                    closestPlayer = player
                end
            end
        end
    end

    return closestPlayer
end
RunService.RenderStepped:Connect(function()
    if holding then
        local target = getClosestPlayer()
        if target and target.Character:FindFirstChild(aimPart) then
            workspace.CurrentCamera.CFrame = CFrame.new(workspace.CurrentCamera.CFrame.Position, target.Character[aimPart].Position)
        end
    end
end)
game:GetService("UserInputService").InputBegan:Connect(function(input)
    if input.KeyCode == aimKey then
        holding = true
    end
end)
game:GetService("UserInputService").InputEnded:Connect(function(input)
    if input.KeyCode == aimKey then
        holding = false
    end
end)
