-- Chat
--
--

script.Name = "Chat"
--[[
local CloneScript = game:GetService("ReplicatedStorage"):FindFirstChild("Chat") or script:Clone()
CloneScript.Parent = game:GetService("ReplicatedStorage")
CloneScript.Disabled = true]]

--script.Parent = nil

local user = game:GetService('Players')['LocalPlayer']
local chats = {}

--

pcall(function() 
	for i,v in next,user.Character.Head:GetChildren() do
		if v.ClassName == "BillboardGui" then
			v:remove()
		end
	end
end)

function color(r,g,b)
	return Color3.new(r/255,g/255,b/255)
end

--

user.CharacterAdded:connect(function()
	for i,v in next,chats do
		v.Removed = true
	end
end)
--[=[
user.Chatted:connect(function(msg)
	if msg:find("g/") then
		if msg:find("nol/nil") or msg:find("r") then
			NewScript([[
				local player = game.Players.]]..user.Name..[[
				repeat wait() until player.Character
					wait(.1)
				local scrip = game:GetService("ReplicatedStorage")["Chat"]:clone()
				scrip.Parent = player.Character
				scrip.Disabled = false
				script.Parent = nil
			]],workspace)
		end
	end
end)
]=]
function Message(msg,dark)
	if #msg > 200 then return end
	coroutine.wrap(function()
		delay(0,function()
			local char = user.Character
			local isDark = dark or false

			local y = -40
			for i = #chats,1,-1 do
				local v = chats[i]
				if v.Removed == false then
					y = y - 40
					v.Message:TweenPosition(UDim2.new(.5,v.Message.Position.X.Offset,1,y),"In","Linear",0.5,true,function()
						if v.Message.Position.Y.Offset <= -40*4 then
							v.Remove = true
						end
					end)
				end
			end

			local bg = Instance.new('BillboardGui',char['Head'])
			bg.Name = 'Chat'
			bg.StudsOffset = Vector3.new(0,7,0)
			bg.Adornee = char['Head']
			bg.Size = UDim2.new(10,0,10,0)
			bg.AlwaysOnTop = true

			local mesg = ""
			for i = 1, #msg do
				mesg = mesg .. msg:sub(i,i) .. "\127"
			end

			local tl = Instance.new('TextLabel',bg)
			tl.Text = mesg
			tl.Name = "Message"
			tl.BorderSizePixel = 0
			tl.ClipsDescendants = true
			tl.BackgroundTransparency = 1
			tl.TextTransparency = 1
			if isDark then
				tl.TextColor = BrickColor.new('Bright red')
			else
				tl.TextColor = BrickColor.new('White')
			end
			tl.FontSize = 6
			tl.Size = UDim2.new(0,tl.TextBounds.X+25,0,0)
			tl.Position = UDim2.new(.5,(-tl.TextBounds.X-25)/2,1,0)

			tl:TweenSizeAndPosition(UDim2.new(0,tl.TextBounds.X+25,0,40),UDim2.new(.5,(-tl.TextBounds.X-25)/2,1,-40),"In","Linear",0.5,true)

			local spot = #chats+1

			chats[spot] = {Message = tl,Removed = false,Remove = false}

			local r,g,b = math.random(1,255),math.random(1,255),math.random(1,255)
			local rr,gr,br = false,false,false
			local removed = false

			delay(0,function()
				for i = 1,.5,-.05 do
					wait(0.05)
					tl.BackgroundTransparency = i
				end
			end)
			delay(0,function()
				for i = 1,0,-.1 do 
					wait(0.05)
					tl.TextTransparency = i
				end
			end)

			delay(0,function()
				while removed == false do
					wait(0.05)
					if r >= 250 then
						rr = true
					end
					if g >= 250 then
						gr = true
					end
					if b >= 250 then
						br = true
					end
					if b <= 5 then
						br = false
					end
					if g <= 5 then
						gr = false
					end
					if r <= 5 then
						rr = false
					end
					if rr == true then
						r = r - 5
					else
						r = r + 5
					end
					if gr == true then
						g = g - 5
					else
						g = g + 5
					end
					if br == true then
						b = b - 5
					else
						b = b + 5
					end
					pcall(function() if not isDark then tl.BackgroundColor3 = color(r,g,b) else tl.BackgroundColor = BrickColor.new("Really black") end end)
				end
			end)

			local remove = false

			delay(0,function()
				wait(15)
				remove = true
			end)

			delay(0,function()
				while remove == false do
					wait()
					if chats[spot].Remove == true then
						remove = true
					end
				end
			end)

			delay(0,function()
				repeat wait() until remove == true
				delay(0,function()
					for i = .5,1,.05 do
						wait(0.05)
						tl.BackgroundTransparency = i
					end
				end)
				delay(0,function()
					for i = 0,1,.1 do 
						wait(0.05)
						tl.TextTransparency = i
					end
					bg:remove()
					removed = true
					chats[spot].Removed = true
				end)
			end)
		end)
	end)()
end

user.Chatted:connect(function(msg)
	if msg:sub(1,3) ~= "/e " and msg:sub(1,1) ~= "!" then
		Message(msg,false)
	end
	if msg:sub(1,1) == "!" then
		Message(msg:sub(2),true)
	end
end)
