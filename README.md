# s-box-coding-standards

_Coding Standards used at s&box studios._

When writing code in s&box studios we always question ourselves, can anyone other than me understand this code that I'm writing? If there's an ounce of doubt we begin rewriting. We put readability first when it comes to coding and consistency. The followwing are the coding standards and styles we opted for when working on projects in s&box studios.

# Script Architecture
At s&box studios, we've opted to embrace Single Script Architecture. Using a single-script architecture is used in most front page Roblox games. These games benefit from controlling when code executes, sharing data structures, better performance, and better code. Code is generally maintained in a single place in the game, and this makes it easier for people to collaborate with modules.

_“It’s not obvious, but modules keep everything clean.
The first thing I teach my friends to do when learning to code is module-hierarchy
I think it’s one of the most important things for new coders to manage the scope of what they have to look at so they don’t get confused.”_

__-Imaginaerum, developer of skybound 1 and 2__

We maintain a strict coding base that we work off of, this differs depending on the script type (Server, Client, Module) but here's the general gist

```lua
local Frame = _G.Framework

local MAX_WALK_SPEED = 20
local ATTACK_COOLDOWN = 0.3
local JUMP_POWER = 65

local ReplicatedStorage = game:GetService("ReplicatedStorage")
local UserInputService = game:GetService("UserInputService")
local ContextActionService = game:GetService("ContextActionService")
local RunService = game:GetService("RunService")

local Remotes = ReplicatedStorage:WaitForChild("Remotes")
local Classes = ReplicatedStorage:WaitForChild("Classes")

local Maid = require(Classes:WaitForChild("Maid"))
local Signal = require(Classes:WaitForChild("Signal"))
local HitboxModule = require(Classes:WaitForChild("RaycastHitboxV4"))

-- public members
local hitbox = HitboxModule.new()

-- private members go below public
local _signal = Signal.new()
local _maid = Maid.new()


local isReady = false
local hasAttacked = false -- I write my booleans based on verbs (i.e can, is) and they're in positive form
-- i.e hasAttacked is better than notCanAttack

local hasWifi = false
local isLoggedIn = false
local hasLoaded = false
local hasAccount = false

local connections = {}

-- I name my subroutine with the prefix of a verb (i.e handle, calculate)
-- use two different subroutines for calculating and handling (not both)
-- rarely use abbreviative variables (i.e a, b, x, y, z) for complex calculations which are really long
-- to type out fully or have no real synonyms
local function calculatePlayerCFrame()
	-- functionality is explained with the key objective to explain "why" rather than "how"
	-- comment a lot to help explain certain tasks and avoid jargin
end


-- all connections that can be connected using an annonymous function would be connected with a procedure.
-- i.e UserInputService.InputBegan:Connect(someFunction) rather than
-- UserInputService.InputBegan:Connect(function(input, processedByUI))

local function handleAttack(input, processedByUI)
	-- avoid a bunch of nested if statements i.e DON'T DO THE FOLLOWING
	
	if (isReady) then
		if (hasAttacked) then
			if (hasWifi) then
				if (hasLoaded) then
					if (hasAccount) then
						print("this is an example of a common bad issue with if statements")
					else
						print("has no account")						
					end
				else
					print("hasn't loaded")
				end
			else
				print("has no wifi")
			end
		else
			print("hasn't attacked")
		end
	else
		print("isn't ready")
	end
	
	-- Instead you'd adopt for this approach:
	if (not isReady) then
		print("isn't ready")
		return
	end
	if (not hasAttacked) then
		print("hasn't attacked")
		return
	end

	if (not hasWifi) then
		print("has no wifi")
		return
	end
	
	if (not hasLoaded) then
		print("hasn't loaded")
		return
	end
	if (not hasAccount) then
		print("has no account")
		return
	end
	
	-- It truly depends on the amount of nested if statements
end


-- I personally handle all connections by adding them into a dictionary if I'm planning on disconnecting them in the future


-- Prevents clogging up global scope
do
	local x = 10
	local y = 10
	
	local z = x + y
	
	warn(z)
end


UserInputService.InputBegan:Connect(handleAttack)
```
