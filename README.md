if _G.Execute then
print('lol')
return
end
_G.Execute = true

--[[
FE Mobile Fly By Rechedmcvn

Type in chat !stop, to stop the script

]]--

local ScreenGui = Instance.new("ScreenGui")
local FlyButton = Instance.new("TextButton")

local NSound = Instance.new("Sound", FlyButton)
NSound.SoundId = "rbxassetid://9086208751"
NSound.Volume = 1

function Notify(Txt, Dur)
STARTERGUI:SetCore("SendNotification",{
        Title = "FED's Mobile Fly",
        Text = Txt,
         Icon = "rbxassetid://278315432",
         Duration = Dur
    })
NSound:Play()
end

-- Detect if script already ran

local VdbwjS = Instance.new("StringValue",game:GetService("ReplicatedStorage"))
VdbwjS.Name = "BZn2q91BzN"

local plr = game:GetService"Players"
local Lp = plr.LocalPlayer
local rs = game:GetService"RunService"
local UserInputService = game:GetService"UserInputService"

local buttonIsOn = false

ScreenGui.Parent = game:GetService("CoreGui")
ScreenGui.ResetOnSpawn = false

FlyButton.Name = "FlyButton"
FlyButton.Parent = ScreenGui
FlyButton.BackgroundColor3 = Color3.new(0.168627, 0.513726, 0.25098)
FlyButton.BorderColor3 = Color3.new(0,0,255)
FlyButton.Position = UDim2.new(0.912970064, 0, 0.194202876, 0)
FlyButton.Size = UDim2.new(0, 50, 0, 50)
FlyButton.Font = Enum.Font.Code
FlyButton.Text = "Fly"
FlyButton.TextColor3 = Color3.new(1,1,1)
FlyButton.TextSize = 14
FlyButton.TextStrokeColor3 = Color3.new(1, 1, 1)
FlyButton.TextWrapped = true
FlyButton.Transparency = 0
FlyButton.Active = true
FlyButton.Draggable = true

local controlModule = require(Lp.PlayerScripts:WaitForChild('PlayerModule'):WaitForChild("ControlModule"))
-- Get joystick

local bv = Instance.new("BodyVelocity")
bv.Name = "VelocityHandler"
bv.Parent = Lp.Character.HumanoidRootPart
bv.MaxForce = Vector3.new(0,0,0)
bv.Velocity = Vector3.new(0,0,0)

local bg = Instance.new("BodyGyro")
bg.Name = "GyroHandler"
bg.Parent = Lp.Character.HumanoidRootPart
bg.MaxTorque = Vector3.new(9e9,9e9,9e9)
bg.P = 1000
bg.D = 50

local Signal1
Signal1 = Lp.CharacterAdded:Connect(function(NewChar)
local bv = Instance.new("BodyVelocity")
bv.Name = "VelocityHandler"
bv.Parent = NewChar:WaitForChild("Humanoid").RootPart
bv.MaxForce = Vector3.new(0,0,0)
bv.Velocity = Vector3.new(0,0,0)

local bg = Instance.new("BodyGyro")
bg.Name = "GyroHandler"
bg.Parent = NewChar:WaitForChild("Humanoid").RootPart
bg.MaxTorque = Vector3.new(9e9,9e9,9e9)
bg.P = 1000
bg.D = 50
end)

local camera = Workspace.CurrentCamera
local speed = 18

local Signal2
Signal2 = rs.RenderStepped:Connect(function()
if Lp.Character and Lp.Character:FindFirstChildOfClass("Humanoid") and Lp.Character.Humanoid.RootPart and Lp.Character.HumanoidRootPart:FindFirstChild("VelocityHandler") and Lp.Character.HumanoidRootPart:FindFirstChild("GyroHandler") then

if buttonIsOn then
FlyButton.Text = "ON"
FlyButton.BackgroundColor3 = Color3.new(0,0,0)
Lp.Character.HumanoidRootPart.VelocityHandler.MaxForce = Vector3.new(9e9,9e9,9e9)
Lp.Character.HumanoidRootPart.GyroHandler.MaxTorque = Vector3.new(9e9,9e9,9e9)
Lp.Character.Humanoid.PlatformStand = true
elseif buttonIsOn == false then
FlyButton.Text = "OFF"
FlyButton.BackgroundColor3 = Color3.new(0,0,0)
Lp.Character.HumanoidRootPart.VelocityHandler.MaxForce = Vector3.new(0,0,0)
Lp.Character.HumanoidRootPart.GyroHandler.MaxTorque = Vector3.new(0,0,0)
Lp.Character.Humanoid.PlatformStand = false
return
end

Lp.Character.HumanoidRootPart.GyroHandler.CFrame = camera.CoordinateFrame
local direction = controlModule:GetMoveVector()
Lp.Character.HumanoidRootPart.VelocityHandler.Velocity = Vector3.new()
if direction.X > 0 then
Lp.Character.HumanoidRootPart.VelocityHandler.Velocity = Lp.Character.HumanoidRootPart.VelocityHandler.Velocity + camera.CFrame.RightVector*(direction.X*speed)
end
if direction.X < 0 then
Lp.Character.HumanoidRootPart.VelocityHandler.Velocity = Lp.Character.HumanoidRootPart.VelocityHandler.Velocity + camera.CFrame.RightVector*(direction.X*speed)
end
if direction.Z > 0 then
Lp.Character.HumanoidRootPart.VelocityHandler.Velocity = Lp.Character.HumanoidRootPart.VelocityHandler.Velocity - camera.CFrame.LookVector*(direction.Z*speed)
end
if direction.Z < 0 then
Lp.Character.HumanoidRootPart.VelocityHandler.Velocity = Lp.Character.HumanoidRootPart.VelocityHandler.Velocity - camera.CFrame.LookVector*(direction.Z*speed)
end
end
end)

FlyButton.TouchTap:Connect(function()
buttonIsOn = not buttonIsOn
end)

local Signal3
Signal3 = SpeedBox:GetPropertyChangedSignal("Text"):Connect(function()
if tonumber(SpeedBox.Text) then
speed = tonumber(SpeedBox.Text)
end
end)

Lp.Chatted:Connect(function(msg)
if msg:sub(1,5) == "!stop" then
Signal1:Disconnect()
Signal2:Disconnect()
Signal3:Disconnect()
game:GetService("ReplicatedStorage"):FindFirstChild("BZn2q91BzN"):Destroy()
ScreenGui:Destroy()
Lp.Character.Humanoid.Health = 0
end
end)
