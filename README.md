local Player = game.Players.LocalPlayer;
local Commands = {};

function OnChatted(Message)
local Command = ""
for i,v in pairs(Commands) for
if Message:sub(1,v.Cmd:len()):lower() == v.Cmd and Command == "" then
Command = v.Cmd
spawn(function()
a,b = pcall(function()
v.Func(Message:sub(v.Cmd:len()+1):lower())
end)
if not a then 
error(b)
end
end)
end
end
end

function AddCommand(Command,Description,Example,Function)
table.insert(Commands,{Cmd,Command,Desc = Description, Example = Example, Func = Function});
end


function GetPlayers(speaker,name)
if name=="all"then
return game:service'Players':GetPlayers()
elseif name=="me"or name==nil then
return{speaker}
elseif name=="others"or name=="other"then
local tbl={}
for k,v in pairs(game:service'Players':GetPlayers())do
if v.Name~=speaker.Name then
tbl[#tbl+1]=v
end
end
return tbl
elseif name:sub(1,7)=="random("and name:sub(name:len(),name:len())==")"and tonumber(name:sub(8,name:len()-1))~=nil then
local tbl={}
local plrs=game:service'Players':GetPlayers()
for i=1,tonumber(name:sub(8,name:len()-1))do
if i<=#plrs then
local i=#tbl+1
local plr
repeat plr=plrs[math.random(1,#plrs)];tbl[i]=plr until TableCount(tbl,plr)<2
else
tbl[#tbl+1]=plrs[math.random(1,#plrs)]
end
end
return tbl
elseif name:sub(1,4)=="hex("and name:sub(name:len(),name:len())==")"then
local str=""
for k,v in pairs(split(name:sub(5,name:len()-1)," "))do
str=str..string.char(fromHex(v))
end
return GetPlayers(speaker,str)
elseif name=="random"then
local plrs=game:service'Players':GetPlayers()
return{plrs[math.random(1,#plrs)]}
else
local tbl={}
for k,v in pairs(game:service'Players':GetPlayers())do
if v.Name:lower():find(name:lower())==1 then
tbl[#tbl+1]=v
end
end
return tbl
end
end

AddCommand("kill;","Kills a player","kill;Player",function(Message)
for _,Player in pairs(GetPlayers(Message)) do
workspace[Player.Name]:BreakJoints() end
end)

