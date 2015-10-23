

--//Wait
Player	= game.Players.LocalPlayer
repeat wait() until Player.Character
Mouse		= Player:GetMouse()
Camera		= game.Workspace.CurrentCamera
Character	= Player.Character
Torso		= Character:WaitForChild("Torso")
Humanoid	= Character:WaitForChild("Humanoid")

--//Initialize
local Ball 			= Instance.new("Part")
Ball.Anchored		= false
Ball.CanCollide		= true
Ball.FormFactor		= Enum.FormFactor.Symmetric
Ball.Shape			= Enum.PartType.Ball
Ball.Size			= Vector3.new(10, 10, 10)
Ball.TopSurface		= Enum.SurfaceType.SmoothNoOutlines
Ball.BottomSurface	= Enum.SurfaceType.SmoothNoOutlines
	
Ball.CFrame			= Torso.CFrame
local Weld				= Instance.new("Weld", Ball)
Weld.Part0			= Ball
Weld.Part1			= Torso

Ball.Parent 		= Character
Humanoid.Sit		= false

Camera.CameraType	 = Enum.CameraType.Track
Camera.CameraSubject = Ball

Direction = {W = 0, A = 0, S = 0, D = 0}

--//Settings
MaxVelocity			= 500 -- Maximum marble speed
Acceleration		= 5	  -- Rate at which speed changes

Ball.Elasticity 	= 0 	-- Bounciness (0 - 1)
Ball.Friction		= 1		-- How well it sticks to the ground (0 - 2)
Ball.Transparency	= 0.5
Ball.BrickColor		= BrickColor.new("White")
Ball.Material		= Enum.Material.Marble

--//Functions
function Parameters()
	if Character and Torso and Humanoid and Ball then
		return true
	else
		return false
	end
end

function Down(Key)	
	if Key == "w" then
		Direction.W = true
		while Direction.W do		
			Ball.Velocity = Ball.Velocity + (Camera.CoordinateFrame.lookVector * Acceleration)
			wait()
		end	
	elseif Key == "a" then
		Direction.A	= true
		while Direction.A do
			Ball.Velocity = Ball.Velocity + ((Camera.CoordinateFrame * CFrame.Angles(0, math.rad(90), 0)).lookVector * Acceleration)
			wait()
		end		
	elseif Key == "s" then
		Direction.S	= true
		while Direction.S do
			Ball.Velocity = Ball.Velocity + (Camera.CoordinateFrame.lookVector * -Acceleration)
			wait()
		end	
	elseif Key == "d" then
		Direction.D	= true
		while Direction.D do
			Ball.Velocity = Ball.Velocity + ((Camera.CoordinateFrame * CFrame.Angles(0, math.rad(-90), 0)).lookVector * Acceleration)
			wait()
		end		
	end
end

function Up(Key)
	if Key == "w" then
		Direction.W = false
	elseif Key == "a" then
		Direction.A	= false
	elseif Key == "s" then
		Direction.S	= false
	elseif Key == "d" then
		Direction.D	= false
	elseif Key == "e" then
		Direction.W, Direction.A, Direction.S, Direction.D = false, false, false, false
		Ball.Velocity = Vector3.new(0, 0, 0)
	end
end

function BallChanged()
	if 	Ball.Velocity.X > MaxVelocity then  Ball.Velocity = Vector3.new(MaxVelocity, Ball.Velocity.Y, Ball.Velocity.Z)
	elseif Ball.Velocity.X < -MaxVelocity then Ball.Velocity = Vector3.new(-MaxVelocity, Ball.Velocity.Y, Ball.Velocity.Z)
	elseif Ball.Velocity.Y > MaxVelocity then Ball.Velocity = Vector3.new(Ball.Velocity.X, MaxVelocity, Ball.Velocity.Z)
	elseif Ball.Velocity.Y < -MaxVelocity then Ball.Velocity = Vector3.new(Ball.Velocity.X, -MaxVelocity, Ball.Velocity.Z)
	elseif Ball.Velocity.Z > MaxVelocity then Ball.Velocity = Vector3.new(Ball.Velocity.X, Ball.Velocity.Y, MaxVelocity)
	elseif Ball.Velocity.Z < -MaxVelocity then Ball.Velocity = Vector3.new(Ball.Velocity.X, Ball.Velocity.Y, -MaxVelocity)
	end
end

function HumanoidChanged() -- Prevents players from standing up
	Humanoid.Jump	= false	
	Humanoid.Sit 	= true
end

function HumanoidDied()
	Ball:Destroy()
end

--//Logic
Mouse.KeyDown:connect(Down)
Mouse.KeyUp:connect(Up)

Ball.Changed:connect(BallChanged)

Humanoid.Changed:connect(HumanoidChanged)
Humanoid.Died:connect(HumanoidDied)
