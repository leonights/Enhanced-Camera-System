local ECS = {}

--- Services ---

local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local UserGameSettings = UserSettings():GetService("UserGameSettings")
local TweenService = game:GetService("TweenService")

--- Constants ---

local Player = Players.LocalPlayer
local CurrentCamera = workspace.CurrentCamera
local DefaultFieldOfView = 70
local CameraTweenInfo = TweenInfo.new(0.1, Enum.EasingStyle.Quad, Enum.EasingDirection.InOut) --Feel free to edit your desired tween style or adding more styles as you like

--- States ---

local isAligned = false --To define the state when the character facing forward as the camera angle
local isSteppedIn = false --To define the state when moveing camera without right-clicking

--- Functions ---

function ECS:MouseCameraOn() --Enable moving-camera-without-right-clicking mode
	
	isSteppedIn = true
	UserInputService.MouseIconEnabled = false
	
end

function ECS:MouseCameraOff() --Disable moving-camera-without-right-clicking mode

	isSteppedIn = false
	UserInputService.MouseIconEnabled = true

end

function ECS:AlignOn() --Enable character alignment to the camera
	
	local Character = Player.Character
	local Humanoid = Character:WaitForChild("Humanoid")
	
	Humanoid.AutoRotate = false
	isAligned = true
	
end

function ECS:AlignOff() --Disable character alignment to the camera

	local Character = Player.Character
	local Humanoid = Character:WaitForChild("Humanoid")

	Humanoid.AutoRotate = true
	isAligned = false

end

function ECS:Aim() --Enable camera offset with zoom effect
	
	local Character = Player.Character
	local Humanoid = Character:WaitForChild("Humanoid")
	
	local FieldOfView = {FieldOfView = DefaultFieldOfView - 20} --Edit the amount of FOV that you would like to substract to achieve zoom effect
	local CameraOffset = {CameraOffset = Vector3.new(2, 1, 0)} --Edit your desired camera offset
	
	local TweenFieldOfView = TweenService:Create(CurrentCamera, CameraTweenInfo, FieldOfView)
	local TweenCameraOffset = TweenService:Create(Humanoid, CameraTweenInfo, CameraOffset)
	
	TweenFieldOfView:Play()
	TweenCameraOffset:Play()
		
end

function ECS:AimScope() --Enable first person mode with scope zoom effect

	local Character = Player.Character

	local FieldOfView = {FieldOfView = DefaultFieldOfView - 50} --Edit the amount of FOV that you would like to substract to achieve zoom effect

	local TweenFieldOfView = TweenService:Create(CurrentCamera, CameraTweenInfo, FieldOfView)

	if Player.CameraMode == Enum.CameraMode.Classic then --To achieve first person camera

		Player.CameraMode = Enum.CameraMode.LockFirstPerson

	end

	TweenFieldOfView:Play()

end

function ECS:AimFirstPerson() --Enable first person mode
	
	if Player.CameraMode == Enum.CameraMode.Classic then --To achieve first person camera
		
		Player.CameraMode = Enum.CameraMode.LockFirstPerson
		
	end

end

function ECS:UnAim() --Reset all offset / camera mode
	
	local Character = Player.Character
	local Humanoid = Character:WaitForChild("Humanoid")

	local FieldOfView = {FieldOfView = DefaultFieldOfView} --To reset FOV
	local CameraOffset = {CameraOffset = Vector3.new(0, 0, 0)} --To reset camera offset
	
	local TweenFieldOfView = TweenService:Create(CurrentCamera, CameraTweenInfo, FieldOfView)
	local TweenCameraOffset = TweenService:Create(Humanoid, CameraTweenInfo, CameraOffset)
	
	if Player.CameraMode == Enum.CameraMode.LockFirstPerson then --To reset camera mode
		
		Player.CameraMode = Enum.CameraMode.Classic

	end
	
	TweenFieldOfView:Play()
	TweenCameraOffset:Play()

end

--- RenderStep ---

RunService.RenderStepped:Connect(function()
	
	if isSteppedIn == true then --To achieve moving-camera-without-right-clicking mode 
		
		UserInputService.MouseBehavior = Enum.MouseBehavior.LockCenter
		
	end
	
	if isAligned == true then --To achieve character alignment to the camera & camera offset obstruction detection
		
		local Character = Player.Character
		local HumanoidRootPart = Character:WaitForChild("HumanoidRootPart")
		local Head = Character:WaitForChild("Head")
		
		local Params = RaycastParams.new()
		Params.FilterDescendantsInstances = {Character}
		Params.FilterType = Enum.RaycastFilterType.Blacklist
		
		local Result = workspace:Raycast(Head.Position, CurrentCamera.CFrame.Position - Head.Position, Params)
	
		if Result ~= nil then
			
			---Credit to Arbeiters for below collision calculation method
			local ObstructionDisplacement = (Result.Position - Head.Position)
			local ObstructionPosition = Head.Position + (ObstructionDisplacement.Unit * (ObstructionDisplacement.Magnitude - 1))
			local x,y,z,r00,r01,r02,r10,r11,r12,r20,r21,r22 = CurrentCamera.CFrame:components()
			
			CurrentCamera.CFrame = CFrame.new(ObstructionPosition.x, ObstructionPosition.y, ObstructionPosition.z , r00, r01, r02, r10, r11, r12, r20, r21, r22)
			
		end
		
		local rx, ry, rz = CurrentCamera.CFrame:ToOrientation()
		HumanoidRootPart.CFrame = CFrame.new(HumanoidRootPart.CFrame.Position) * CFrame.fromOrientation(0, ry, 0)
					
	end
	
end)

return ECS
