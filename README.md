local function Check(productName, key)
    local hwid = game:GetService("RbxAnalyticsService"):GetClientId()
    if not hwid or not productName then
        return false
    end
    local data = {
        action = "checkAndSetHwid",
        productName = productName,
        key = key,
        hwid = hwid
    }
    local request = http_request or request or (http and http.request) or (syn and syn.request)
    local jsonData = game:GetService("HttpService"):JSONEncode(data)
    local response = request{
        Url = "https://servicemain.vercel.app/api/whitelist",
        Method = 'POST',
        Headers = {
            ['Content-Type'] = 'application/json'
        },
        Body = jsonData
    }
    if response.StatusCode == 200 then
        local result = game:GetService("HttpService"):JSONDecode(response.Body)

        if result.status == 'success' then
            return true
        end
    else
        local result = game:GetService("HttpService"):JSONDecode(response.Body)
        if result.Success then
            return true
        end
    end
    local result = game:GetService("HttpService"):JSONDecode(response.Body)
    game.Players.LocalPlayer:Kick(result.message or "Error: Unknown issue")
    return false
end
local product = "lerk.lua"
script_key = script_key or ""
local checkdone = Check(product, script_key) or false
if not checkdone then return end
local RunService = game:GetService("RunService")
local LocalPlayer = Players.LocalPlayer
local Mouse = game.Players.LocalPlayer:GetMouse()
local CamlockState = true
local Prediction = 0.129934
local HorizontalPrediction = 5.88
local VerticalPrediction = 7.7524999999999995
local XPrediction = 0.176073
local YPrediction = 0.167092
local Smoothness = 0.1

getgenv().Key = "q"

function BlackPussy()
    local ClosestDistance, ClosestPlayer = math.huge, nil
    local CenterPosition =
        Vector2.new(
        game:GetService("GuiService"):GetScreenResolution().X / 2,
        game:GetService("GuiService"):GetScreenResolution().Y / 2
    )

    for _, Player in ipairs(game:GetService("Players"):GetPlayers()) do
        if Player ~= LocalPlayer then
            local Character = Player.Character
            if Character and Character:FindFirstChild("HumanoidRootPart") and Character.Humanoid.Health > 0 then
                local Position, IsVisibleOnViewport =
                    game:GetService("Workspace").CurrentCamera:WorldToViewportPoint(Character.HumanoidRootPart.Position)

                if IsVisibleOnViewport then
                    local Distance = (CenterPosition - Vector2.new(Position.X, Position.Y)).Magnitude
                    if Distance < ClosestDistance then
                        ClosestPlayer = Character.HumanoidRootPart
                        ClosestDistance = Distance
                    end
                end
            end
        end
    end

    return ClosestPlayer
end
local enemy = nil

RunService.Heartbeat:Connect(function()
    if CamlockState == true then
        if enemy then
            local camera = workspace.CurrentCamera
            local targetPosition = enemy.Position + enemy.Velocity * Prediction
            targetPosition = Vector3.new(targetPosition.X + XPrediction, targetPosition.Y + YPrediction, targetPosition.Z)
            
            -- Interpolate between the current camera position and the target position
            local currentPosition = camera.CFrame.Position
            local newPosition = currentPosition:Lerp(targetPosition, Smoothness)
            
            camera.CFrame = CFrame.new(newPosition, targetPosition)
        end
    end
end)

Mouse.KeyDown:Connect(function(k)    
    if k == getgenv().Key then    
            Locked = not Locked    
            if Locked then    
                enemy = BlackPussy()
                CamlockState = true
                TargetState = true
             else    
                if enemy ~= nil then    
                    enemy = nil    
                    CamlockState = false
                    TargetState = false
                end    
            end    
    end    
end)

local UICorner = Instance.new("UICorner")
local UICorner2 = Instance.new("UICorner")

local Eclipse = Instance.new("ScreenGui")

Eclipse.Name = "BuddieForZxx"
Eclipse.Parent = game.CoreGui
Eclipse.ZIndexBehavior = Enum.ZIndexBehavior.Sibling

local Frame = Instance.new("Frame")

Frame.Parent = Lerk.lua
Frame.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
Frame.BorderColor3 = Color3.fromRGB(96, 96, 96)
Frame.BorderSizePixel = 5
Frame.BackgroundTransparency = 0.6
Frame.Position = UDim2.new(0.5, -97.5, 0.5, -32.5)
Frame.Size = UDim2.new(0, 110, 0, 60)
Frame.Active = true
Frame.Draggable = true

UICorner2.CornerRadius = UDim.new(0, 10)
UICorner2.Parent = Frame

local TextButton = Instance.new("TextButton")

TextButton.Parent = Frame
TextButton.BackgroundColor3 = Color3.fromRGB(96, 96, 96)
TextButton.BorderColor3 = Color3.fromRGB(96, 96, 96)
TextButton.BorderSizePixel = 0
TextButton.Position = UDim2.new(0.0792079195, 0, 0.18571429, 0)
TextButton.Size = UDim2.new(0, 93, 0, 40)
TextButton.Font = Enum.Font.Gotham
TextButton.Text = ""
TextButton.TextColor3 = Color3.fromRGB(255, 255, 255)
TextButton.TextScaled = true
TextButton.TextSize = 24
TextButton.TextWrapped = true
local state = true
TextButton.MouseButton1Click:Connect(
    function()
        state = not state
        if not state then
            TextButton.Text = "Lerk.lua On"
            CamlockState = true
            enemy = BlackPussy()
        else
            TextButton.Text = "Lerk.lua Off"
            CamlockState = false
            enemy = nil
        end
    end
)
UICorner.CornerRadius = UDim.new(0, 10)
UICorner.Parent = TextButton
Frame.Active = true
Frame.Draggable = true

--// credits to @8lwz
