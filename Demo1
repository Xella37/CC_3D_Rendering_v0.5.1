
-- Made by Xelostar: https://www.youtube.com/channel/UCDE2STpSWJrIUyKtiYGeWxw

local path = "/"..shell.dir()

os.loadAPI(path.."/ThreeD")
os.loadAPI(path.."/bufferAPI")
os.loadAPI(path.."/blittle")
os.loadAPI(path.."/noise")

local function generateSeed()
	local preSeed = 0
	local webPage = http.get("https://www.random.org/cgi-bin/randbyte?nbytes=7&format=d")

	if (webPage) then
		local rawSeed = webPage.readAll()
		for byteString in rawSeed:gmatch("%d+") do
			preSeed = preSeed * 256 + tonumber(byteString)
		end
	else
		preSeed = os.time()
	end

	return preSeed - 1
end

local function generateWorld(seed, rows, columns, maxHeight, relativeSnowHeight)
	local objects = {}
	local models = {}

	local xOffset = -(4 + 8/4)
	local zOffset = -(4 * math.pow(0.75, 0.5))

	math.randomseed(seed)
	for chunkX = 0, columns - 1 do
		for chunkZ = 0, rows - 1 do
			local mapNoise1 = noise.createNoise(8, chunkX, chunkZ, seed)
			local mapNoise2 = noise.createNoise(8, chunkX + 1, chunkZ, seed)
			local mapNoise3 = noise.createNoise(8, chunkX, chunkZ + 1, seed)
			local mapNoise4 = noise.createNoise(8, chunkX + 1, chunkZ + 1, seed)

			local map = {}
			for a = 1, 8 do
				for b = 1, 8 do
					local height1 = mapNoise1[a][b]*maxHeight - 0.5*maxHeight
					local height2 = 0
					if (a < 8) then
						height2 = mapNoise1[a+1][b]*maxHeight - 0.5*maxHeight
					else
						height2 = mapNoise2[1][b]*maxHeight - 0.5*maxHeight
					end
					local height3 = 0
					if (b < 8) then
						height3 = mapNoise1[a][b+1]*maxHeight - 0.5*maxHeight
					else
						height3 = mapNoise3[a][1]*maxHeight - 0.5*maxHeight
					end
					local height4 = 0
					if (a == 8 and b == 8) then
						height4 = mapNoise4[1][1]*maxHeight - 0.5*maxHeight
					elseif (a == 8) then
						height4 = mapNoise2[1][b+1]*maxHeight - 0.5*maxHeight
					elseif (b == 8) then
						height4 = mapNoise3[a+1][1]*maxHeight - 0.5*maxHeight
					else
						height4 = mapNoise1[a+1][b+1]*maxHeight - 0.5*maxHeight
					end

					local c1 = colors.lime
					local c2 = colors.green

					local snowHeight = relativeSnowHeight * maxHeight - 0.5*maxHeight

					if (height1 >= snowHeight or height2 >= snowHeight or height3 >= snowHeight) then
						c1 = colors.white
					end
					if (height2 >= snowHeight or height3 >= snowHeight or height4 >= snowHeight) then
						c2 = colors.lightGray
					end
					
					map[#map+1] = {
						x1 = xOffset + b/2 + a+1, y1 = height2, z1 = zOffset + b*math.pow(0.75, 0.5),
						x2 = xOffset + b/2 + a, y2 = height1, z2 = zOffset + b*math.pow(0.75, 0.5),
						x3 = xOffset + b/2 + a+0.5, y3 = height3, z3 = zOffset + (b+1)*math.pow(0.75, 0.5),
						c = c1
					}

					map[#map+1] = {
						x1 = xOffset + b/2 + a+0.5, y1 = height3, z1 = zOffset + (b+1)*math.pow(0.75, 0.5),
						x2 = xOffset + b/2 + a+1.5, y2 = height4, z2 = zOffset + (b+1)*math.pow(0.75, 0.5),
						x3 = xOffset + b/2 + a+1, y3 = height2, z3 = zOffset + b*math.pow(0.75, 0.5),
						c = c2
					}
				end
			end
			objects[#objects+1] = {model = "chunk"..chunkX..";"..chunkZ, x = chunkX * 8 + chunkZ * 4, y = 0, z = chunkZ * 8 * math.pow(0.75, 0.5)}
			models[#models+1] = {modelname = "chunk"..chunkX..";"..chunkZ, model = map}
		end
	end

	return objects, models
end

local playerX = 0
local playerY = 0
local playerZ = 0

local playerSpeed = 3
local playerTurnSpeed = 180
local keysDown = {}
local blockThickness = 0.9

local FoV = 90

local playerDirectionHor = 30
local playerDirectionVer = -45

local screenWidth, screenHeight = term.getSize()

local backgroundColor1 = colors.lightBlue
local backgroundColor2 = colors.lightBlue

local ThreeDFrame = ThreeD.newFrame(1, 1, screenWidth, screenHeight, FoV, playerX, playerY + 0.5, playerZ, playerDirectionVer, playerDirectionHor, path.."/models")
ThreeDFrame:saveSortedModels(true)
local blittleOn = true
ThreeDFrame:useBLittle(blittleOn)

local totalTime = 0

local seed = generateSeed()
local chunkRows = 1
local chunkCollumns = 1
local maxHeight = 8
local relativeSnowHeight = 0.70

local objects, models = generateWorld(seed, chunkRows, chunkCollumns, maxHeight, relativeSnowHeight)

for i = 1, #models do
	ThreeDFrame:loadModel(models[i].modelname, models[i].model)
end

local function rendering()
	while true do
		ThreeDFrame:loadGround(backgroundColor1)
		ThreeDFrame:loadSky(backgroundColor2)
		ThreeDFrame:loadObjects(objects)
		ThreeDFrame:drawBuffer()

		os.queueEvent("FakeEvent")
		os.pullEvent("FakeEvent")
	end
end

local function free(x, y, z)
	for _, object in pairs(objects) do
		if (object.solid == true) then
			if (x >= object.x - blockThickness and x <= object.x + blockThickness) then
				if (y >= object.y - 1 and y <= object.y + 0.5 + 0.2) then
					if (z >= object.z - blockThickness and z <= object.z + blockThickness) then
						return false
					end
				end
			end
		end
	end

	return true
end

local function inputPlayer(time)
	local dx = 0
	local dy = 0
	local dz = 0

	if (keysDown[keys.left]) then
		playerDirectionHor = playerDirectionHor - playerTurnSpeed * time
		if (playerDirectionHor <= -180) then
			playerDirectionHor = playerDirectionHor + 360
		end
	end
	if (keysDown[keys.right]) then
		playerDirectionHor = playerDirectionHor + playerTurnSpeed * time
		if (playerDirectionHor >= 180) then
			playerDirectionHor = playerDirectionHor - 360
		end
	end
	if (keysDown[keys.down]) then
		playerDirectionVer = playerDirectionVer - playerTurnSpeed * time
		if (playerDirectionVer < -80) then
			playerDirectionVer = -80
		end
	end
	if (keysDown[keys.up]) then
		playerDirectionVer = playerDirectionVer + playerTurnSpeed * time
		if (playerDirectionVer > 80) then
			playerDirectionVer = 80
		end
	end
	if (keysDown[keys.w]) then
		dx = playerSpeed * math.cos(math.rad(playerDirectionHor)) + dx
		dz = playerSpeed * math.sin(math.rad(playerDirectionHor)) + dz
	end
	if (keysDown[keys.s]) then
		dx = -playerSpeed * math.cos(math.rad(playerDirectionHor)) + dx
		dz = -playerSpeed * math.sin(math.rad(playerDirectionHor)) + dz
	end
	if (keysDown[keys.a]) then
		dx = playerSpeed * math.cos(math.rad(playerDirectionHor - 90)) + dx
		dz = playerSpeed * math.sin(math.rad(playerDirectionHor - 90)) + dz
	end
	if (keysDown[keys.d]) then
		dx = playerSpeed * math.cos(math.rad(playerDirectionHor + 90)) + dx
		dz = playerSpeed * math.sin(math.rad(playerDirectionHor + 90)) + dz
	end
	
	if (keysDown[keys.space]) then
		dy = playerSpeed + dy
	end
	if (keysDown[keys.leftShift]) then
		dy = -playerSpeed + dy
	end
	
	if (free(playerX + dx * time, playerY, playerZ) == true) then
		playerX = playerX + dx * time
	end
	if (free(playerX, playerY + dy * time, playerZ) == true) then
		playerY = playerY + dy * time
	end
	if (free(playerX, playerY, playerZ + dz * time) == true) then
		playerZ = playerZ + dz * time
	end

	ThreeDFrame:setCamera(playerX, playerY + 0.5, playerZ, playerDirectionHor, playerDirectionVer)
end

local function keyInput()
	while true do
		local event, key, x, y = os.pullEventRaw()

		if (event == "key") then
			keysDown[key] = true
			if (key == keys.g) then
				if (blittleOn == false) then
					blittleOn = true
					ThreeDFrame:useBLittle(true)
				else
					blittleOn = false
					ThreeDFrame:useBLittle(false)
				end
			end
		elseif (event == "key_up") then
			keysDown[key] = nil
		end
	end
end

local function updateGame(time)
	totalTime = totalTime + time
end

local function gameUpdate()
	local timeFromLastUpdate = os.clock()
	local avgUpdateSpeed = 0
	local updateCount = 0
	local timeOff = 0

	while true do
		local currentTime = os.clock()
		if (currentTime > timeFromLastUpdate) then
			updateGame(currentTime - timeFromLastUpdate - timeOff)
			inputPlayer(currentTime - timeFromLastUpdate - timeOff)
			avgUpdateSpeed = (currentTime - timeFromLastUpdate) / (updateCount + 1)
			updateCount = 0
			timeOff = 0
		else
			updateGame(avgUpdateSpeed)
			inputPlayer(avgUpdateSpeed)
			newTimeOff = timeOff + avgUpdateSpeed
			newUpdateCount = updateCount + 1
		end
		timeFromLastUpdate = currentTime

		sleep(0)
	end
end

parallel.waitForAll(keyInput, gameUpdate, rendering)