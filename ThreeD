
-- Made by Xelostar: https://www.youtube.com/channel/UCDE2STpSWJrIUyKtiYGeWxw

local function round(number)
	return math.floor(number + 0.5)
end

local function sortPolygons(polygons, objectX, objectY, objectZ, cameraX, cameraY, cameraZ)
	local sortedPolygons = {}

	for i = 1, #polygons do
		local polygon = polygons[i]

		local avgX = (math.min(polygon[1] + objectX, polygon[4] + objectX, polygon[7] + objectX) + math.max(polygon[1] + objectX, polygon[4] + objectX, polygon[7] + objectX)) * 0.5
		local avgY = (math.min(polygon[2] + objectY, polygon[5] + objectY, polygon[8] + objectY) + math.max(polygon[2] + objectY, polygon[5] + objectY, polygon[8] + objectY)) * 0.5
		local avgZ = (math.min(polygon[3] + objectZ, polygon[6] + objectZ, polygon[9] + objectZ) + math.max(polygon[3] + objectZ, polygon[6] + objectZ, polygon[9] + objectZ)) * 0.5

		local dX = cameraX - avgX
		local dY = cameraY - avgY
		local dZ = cameraZ - avgZ

		local distance = math.pow(dX*dX + dY*dY + dZ*dZ, 0.5)

		polygon[14] = distance
		sortedPolygons[#sortedPolygons+1] = polygon
	end
	
	table.sort(sortedPolygons, function(a, b) return a[14] > b[14] end)
	
	return sortedPolygons
end

local function loadModel(modelName, modelsDir)
	local modelFile = fs.open(modelsDir.."/"..modelName, "r")
	content = modelFile.readAll()
	modelFile.close()

	local model = textutils.unserialise(content)

	return model
end

local function rotatePolygon(x, y, z, rotationY)
	local x = x + 0.0000001
	local y = y + 0.0000001
	local z = z + 0.0000001
	local hAlpha = math.deg(math.atan(x / z))
	if (z < 0) then
		if (x < 0) then
			hAlpha = hAlpha - 180
		else
			hAlpha = hAlpha + 180
		end
	else
		hAlpha = hAlpha
	end

	local hAlpha2 = hAlpha + rotationY
	if (hAlpha2 > 180) then
		while (hAlpha2 > 180) do
			hAlpha2 = hAlpha2 - 360
		end
	elseif (hAlpha2 < -180) then
		while (hAlpha2 < -180) do
			hAlpha2 = hAlpha2 + 360
		end
	end

	local hDistance = x / math.sin(math.rad(hAlpha))

	local x = hDistance * math.sin(math.rad(hAlpha2))
	local z = hDistance * math.cos(math.rad(hAlpha2))

	return x, y, z
end

local function rotateModel(model, rotationY)
	if (rotationY ~= 0) then
		local rotatedModel = {}

		for _, polygon in pairs(model) do
			local x1, y1, z1 = rotatePolygon(polygon[1], polygon[2], polygon[3], rotationY)
			local x2, y2, z2 = rotatePolygon(polygon[4], polygon[5], polygon[6], rotationY)
			local x3, y3, z3 = rotatePolygon(polygon[7], polygon[8], polygon[9], rotationY)

			rotatedModel[#rotatedModel+1] = {x1, y1, z1, x2, y2, z2, x3, y3, z3}
			rotatedModel[#rotatedModel][10] = polygon[10]
			rotatedModel[#rotatedModel][11] = polygon[11]
			rotatedModel[#rotatedModel][12] = polygon[12]
			rotatedModel[#rotatedModel][13] = polygon[13]
		end

		return rotatedModel
	else
		return model
	end
end

local function sortObjects(objects, cameraX, cameraY, cameraZ, renderDistance)
	local sortedObjects = {}

	for i = 1, #objects do
		local object = objects[i]
		local dX = cameraX - object.x
		local dY = cameraY - object.y
		local dZ = cameraZ - object.z
		local distance = math.pow(dX*dX + dY*dY + dZ*dZ, 0.5)
		if (distance <= renderDistance) then
			object.distance = distance
			sortedObjects[#sortedObjects+1] = object
		end
	end
	
	table.sort(sortedObjects, function (a, b) return a.distance > b.distance end)
	
	return sortedObjects
end

local function rotate90(vector)
	local newVector = {x = -vector.y, y = vector.x}
	return newVector
end
 
local function dot(a, b)
	return (a.x * b.x + a.y * b.y)
end
 
local function isFacingCamera(pointA, pointB, pointC)
	local dirAB = {}
	dirAB.x = pointB.x - pointA.x
	dirAB.y = pointB.y - pointA.y
 
	local dirBC = {}
	dirBC.x = pointC.x - pointB.x
	dirBC.y = pointC.y - pointB.y
 
	if (dot(rotate90(dirAB), dirBC) < 0) then
		return true
	end
 
	return false
end

local function linear(x1, y1, x2, y2)
	local dy = y2 - y1
	local dx = x2 - x1

	local a = math.pow(10, 99)
	if (dx ~= 0) then
		a = dy / dx
	end
	local b = y1 - a * x1

	return a, b
end

local function isInLine(checkX, checkY, x1, y1, x2, y2, framex1, framey1, framex2, framey2)
	local a, b = linear(x1, y1, x2, y2)

	if (x2 >= x1) then
		local start = x1>1 and x1 or 1
		for x = start, x2 do
			if (x == checkX) then
				local y = a * x + b
				if (y == checkY) then
					return true
				end
			end

			if (x > framex2 - framex1 + 6) then
				break
			end
		end
	else
		local start = x2>1 and x2 or 1
		for x = start, x1 do
			if (x == checkX) then
				local y = a * x + b
				if (y == checkY) then
					return true
				end
			end

			if (x > framex2 - framex1 + 6) then
				break
			end
		end
	end

	if (y2 >= y1) then
		local start = y1>1 and y1 or 1
		for y = start, y2 do
			if (y == checkY) then
				local x = (y - b) / a
				if (x == checkX) then
					return true
				end
			end

			if (y > framey2 - framey1 + 6) then
				break
			end
		end
	else
		local start = y2>1 and y2 or 1
		for y = start, y1 do
			if (y == checkY) then
				local x = (y - b) / a
				if (x == checkX) then
					return true
				end
			end

			if (y > framey2 - framey1 + 6) then
				break
			end
		end
	end

	return false
end

local function isInHorLine(checkX, checkY, a1, b1, a2, b2, startY, endY, framex1, framey1, framex2, framey2)
	if (startY < 0) then startY = 0
	elseif (startY > framey2 - framey1 + 2) then startY = framey2 - framey1 + 2 end
	if (endY < 0) then endY = 0
	elseif (endY > framey2 - framey1 + 2) then endY = framey2 - framey1 + 2 end

	for y = startY, endY do
		if (y == checkY) then
			local y2 = y
			if (y ~= startY and y ~= endY) then
				y2 = round(y)
			end

			local x1 = round((round(y2 - 0.5) - b1) / a1)
			local x2 = round((round(y2 - 0.5) - b2) / a2)

			if (x1 < 0) then x1 = 0
			elseif (x1 > framex2 - framex1 + 2) then x1 = framex2 - framex1 + 2 end
			if (x2 < 0) then x2 = 0
			elseif (x2 > framex2 - framex1 + 2) then x2 = framex2 - framex1 + 2 end

			if (x1 < x2) then
				for x = x1, x2 do
					if (x == checkX) then
						return true
					end
				end
			else
				for x = x2, x1 do
					if (x == checkX) then
						return true
					end
				end
			end
		end
	end

	return false
end

local function sign(p1, p2, p3)
	return (p1.x - p3.x) * (p2.y - p3.y) - (p2.x - p3.x) * (p1.y - p3.y)
end

local function isInTriangle(checkX, checkY, x1, y1, x2, y2, x3, y3, framex1, framey1, framex2, framey2, alternative)
	if (not alternative) then
		local a1, b1 = linear(x1, y1, x2, y2)
		local a2, b2 = linear(x2, y2, x3, y3)
		local a3, b3 = linear(x1, y1, x3, y3)

		if (y1 <= y2 and y1 <= y3) then
			if (y2 <= y3) then
				if (a1 ~= 0) then
					if (isInHorLine(checkX, checkY, a1, b1, a3, b3, y1, y2, framex1, framey1, framex2, framey2)) then
						return true
					end
				end
				if (a2 ~= 0) then
					if (isInHorLine(checkX, checkY, a2, b2, a3, b3, y2, y3, framex1, framey1, framex2, framey2)) then
						return true
					end
				end
			else
				if (a3 ~= 0) then
					if (isInHorLine(checkX, checkY, a1, b1, a3, b3, y1, y3, framex1, framey1, framex2, framey2)) then
						return true
					end
				end
				if (a2 ~= 0) then
					if (isInHorLine(checkX, checkY, a1, b1, a2, b2, y3, y2, framex1, framey1, framex2, framey2)) then
						return true
					end
				end
			end
		elseif (y2 <= y1 and y2 <= y3) then
			if (y1 <= y3) then
				if (a1 ~= 0) then
					if (isInHorLine(checkX, checkY, a1, b1, a2, b2, y2, y1, framex1, framey1, framex2, framey2)) then
						return true
					end
				end
				if (a3 ~= 0) then
					if (isInHorLine(checkX, checkY, a2, b2, a3, b3, y1, y3, framex1, framey1, framex2, framey2)) then
						return true
					end
				end
			else
				if (a2 ~= 0) then
					if (isInHorLine(checkX, checkY, a1, b1, a2, b2, y2, y3, framex1, framey1, framex2, framey2)) then
						return true
					end
				end
				if (a3 ~= 0) then
					if (isInHorLine(checkX, checkY, a1, b1, a3, b3, y3, y1, framex1, framey1, framex2, framey2)) then
						return true
					end
				end
			end
		else
			if (y1 <= y2) then
				if (a3 ~= 0) then
					if (isInHorLine(checkX, checkY, a2, b2, a3, b3, y3, y1, framex1, framey1, framex2, framey2)) then
						return true
					end
				end
				if (a1 ~= 0) then
					if (isInHorLine(checkX, checkY, a1, b1, a2, b2, y1, y2, framex1, framey1, framex2, framey2)) then
						return true
					end
				end
			else
				if (a2 ~= 0) then
					if (isInHorLine(checkX, checkY, a2, b2, a3, b3, y3, y2, framex1, framey1, framex2, framey2)) then
						return true
					end
				end
				if (a1 ~= 0) then
					if (isInHorLine(checkX, checkY, a1, b1, a3, b3, y2, y1, framex1, framey1, framex2, framey2)) then
						return true
					end
				end
			end
		end

		if (isInLine(checkX, checkY, x1, y1, x2, y2, framex1, framey1, framex2, framey2)) then
			return true
		elseif (isInLine(checkX, checkY, x2, y2, x3, y3, framex1, framey1, framex2, framey2)) then
			return true
		elseif (isInLine(checkX, checkY, x3, y3, x1, y1, framex1, framey1, framex2, framey2)) then
			return true
		end

		return false
	else -- alternative (faster but doesn't exactly match the triangles that are drawn)
		local b1 = sign({x = checkX, y = checkY}, {x = x1, y = y1}, {x = x2, y = y2}) < 0
		local b2 = sign({x = checkX, y = checkY}, {x = x2, y = y2}, {x = x3, y = y3}) < 0
		local b3 = sign({x = checkX, y = checkY}, {x = x3, y = y3}, {x = x1, y = y1}) < 0

		return ((b1 == b2) and (b2 == b3))
	end
end

local pi = math.pi
local doublepi = 2 * math.pi
local halfpi = 0.5 * math.pi
 
function newFrame(x1, y1, x2, y2, FoV, cameraX, cameraY, cameraZ, cameraDirZ, cameraDirY, modelsDir, oldModelsDir)
	if (x1 == nil or y1 == nil or x2 == nil or y2 == nil) then
		error("When creating a new frame, the size must be specified!")
	end

	local frame = {}
	frame.modelsDir = oldModelsDir or modelsDir or "/models"
	frame.models = {}
	frame.modelsSorted = {}
	frame.saveSortedModelsBool = false
 
	frame.frameX1 = x1
	frame.frameY1 = y1
	frame.frameX2 = x2
	frame.frameY2 = y2

	frame.buffer = bufferAPI.newBuffer(x1, y1, x2, y2)
	frame.buffer:clear(colors.white)

	frame.blittleOn = false
	frame.pixelratio = 1.5
 
	frame.renderDistance = math.huge
	frame.FoV = FoV or 90
	frame.d = 0.2
	frame.t = math.tan(math.rad(frame.FoV / 2)) * 2 * frame.d
 
	frame.cameraDirZ = 0
	if (cameraDirZ) then
		frame.cameraDirZ = math.rad(cameraDirZ)
	end
	frame.cameraDirY = 0
	if (cameraDirY) then
		frame.cameraDirY = math.rad(cameraDirY)
	end
 
	frame.cameraX = cameraX or 0
	frame.cameraY = cameraY or 0
	frame.cameraZ = cameraZ or 0

	frame.loadedBackgroundColor = false
 
	function frame:setSize(x1, y1, x2, y2)
		self.frameX1 = x1
		self.frameY1 = y1
		self.frameX2 = x2
		self.frameY2 = y2
 
		if (not self.blittleOn) then
			self.buffer:setBufferSize(x1, y1, x2, y2)
			self.width = x2 - x1 + 1
			self.height = y2 - y1 + 1
			self.pixelratio = 1.5
		else
			self.buffer:setBufferSize(x1 * 2, y1 * 3, x2 * 2, y2 * 3)
			self.width = (x2 - x1 + 1) * 2
			self.height = (y2 - y1 + 1) * 3
			self.pixelratio = 1
		end
	end
 
	function frame:useBLittle(use)
		self.blittleOn = use
		if (use) then
			self.buffer:setBufferSize((self.frameX1 - 1) * 2 + 1, (self.frameY1 - 1) * 3 + 1, (self.frameX2) * 2, (self.frameY2 + 1) * 3)
			self.width = (self.frameX2 - self.frameX1 + 1) * 2
			self.height = (self.frameY2 - self.frameY1 + 1) * 3
			self.pixelratio = 1
		else
			self.buffer:setBufferSize(self.frameX1, self.frameY1, self.frameX2, self.frameY2)
			self.width = self.frameX2 - self.frameX1 + 1
			self.height = self.frameY2 - self.frameY1 + 1
			self.pixelratio = 1.5
		end
	end
 
	function frame:setCamera(cameraX, cameraY, cameraZ, cameraDirY, cameraDirZ, FoV)
		self.cameraX = cameraX + 0.001
		self.cameraY = cameraY + 0.001
		self.cameraZ = cameraZ + 0.001
 
		if (cameraDirY ~= nil) then
			self.cameraDirY = math.rad(cameraDirY) + 0.001
		end
		if (cameraDirZ ~= nil) then
			self.cameraDirZ = math.rad(cameraDirZ) + 0.001
		end
		if (FoV ~= nil) then
			self.FoV = FoV
		end
	end
 
	function frame:setRenderDistance(renderDistance)
		self.renderDistance = renderDistance
	end

	function frame:loadModel(modelName, model)
		local transformedModel = {}
		for plyNr, polygon in pairs(model) do
			transformedModel[#transformedModel+1] = {}
			transformedModel[#transformedModel][1] = polygon.x1
			transformedModel[#transformedModel][2] = polygon.y1
			transformedModel[#transformedModel][3] = polygon.z1
			transformedModel[#transformedModel][4] = polygon.x2
			transformedModel[#transformedModel][5] = polygon.y2
			transformedModel[#transformedModel][6] = polygon.z2
			transformedModel[#transformedModel][7] = polygon.x3
			transformedModel[#transformedModel][8] = polygon.y3
			transformedModel[#transformedModel][9] = polygon.z3
			transformedModel[#transformedModel][10] = polygon.forceRender
			transformedModel[#transformedModel][11] = polygon.c
			transformedModel[#transformedModel][12] = polygon.char
			transformedModel[#transformedModel][13] = polygon.charc
		end
		self.models[modelName] = transformedModel
	end

	function frame:getModel(modelName)
		local model = self.models[modelName]
		local transformedModel = {}
		for key, polygon in pairs(model) do
			transformedModel[key] = {x1 = polygon[1], y1 = polygon[2], z1 = polygon[3], x2 = polygon[4], y2 = polygon[5], z2 = polygon[6], x3 = polygon[7], y3 = polygon[8], z3 = polygon[9], forceRender = polygon[10], c = polygon[11], char = polygon[12], charc = polygon[13]}
		end

		return transformedModel
	end

	function frame:saveSortedModels(c)
		self.saveSortedModelsBool = c
	end
 
	function frame:findPointOnScreen(oX, oY, oZ)
		local dX = oX - self.cameraX
		local dY = oY - self.cameraY
		local dZ = oZ - self.cameraZ
 
		-- Horizontal rotation
 
		local hAlpha = math.atan(dX / dZ)
		if (dZ < 0) then
			if (dX < 0) then
				hAlpha = hAlpha - pi
			else
				hAlpha = hAlpha + pi
			end
		end
 
		local hAlpha2 = hAlpha + self.cameraDirY
		if (hAlpha2 > pi) then
				hAlpha2 = hAlpha2 - doublepi
		elseif (hAlpha2 < -pi) then
				hAlpha2 = hAlpha2 + doublepi
		end
 
		local hDistance = dX / math.sin(hAlpha)
 
		local dX = hDistance * math.sin(hAlpha2)
		local dZ = hDistance * math.cos(hAlpha2)
 
		-- Vertical rotation
 
		local vAlpha = math.atan(dY / dX)
 
		if (dX < 0) then
			if (dY < 0) then
				vAlpha = vAlpha - pi
			else
				vAlpha = vAlpha + pi
			end
		end
 
		local vAlpha2 = vAlpha - self.cameraDirZ
		if (vAlpha2 > pi) then
			vAlpha2 = vAlpha2 - doublepi
		elseif (hAlpha2 < -pi) then
			vAlpha2 = vAlpha2 + doublepi
		end

		if (vAlpha2 >= halfpi or vAlpha2 <= -halfpi) then
			return 0, 0, false
		end
 
		local vDistance = dX / math.cos(vAlpha)
 
		local dY = vDistance * math.sin(vAlpha2)
		local dX = vDistance * math.cos(vAlpha2)
		
		-- Recalculating horizontal angle after vertical rotation
 
		local hAlpha3 = math.atan(dX / dZ)
		if (dZ < 0) then
			if (dX < 0) then
				hAlpha3 = hAlpha3 - pi
			else
				hAlpha3 = hAlpha3 + pi
			end
		end
 
		-- Using the angles to calculate the coordinates on the screen
 
 		local tx = 0
		if (hAlpha3 > 0 and hAlpha3 < halfpi) then
			tx = self.d / math.tan(hAlpha3)
		elseif (hAlpha3 >= halfpi and hAlpha3 < pi) then
			tx = self.d * -math.tan(hAlpha3 - halfpi)
		end
 
		if (hAlpha3 >= pi or hAlpha3 <= 0) then
			return 0, 0, false
		end

		local x = tx / self.t * self.width + math.floor(self.width * 0.5) + 1
		local y = -self.d * math.tan(vAlpha2) * self.width / (self.t * self.height * self.pixelratio) * self.height + math.floor(self.height * 0.5)
		if (y ~= y) then
			y = math.floor(self.height * 0.5)
		end
 
		return x, y, true
	end
 
	function frame:loadObject(object)
		local x = object.x
		local y = object.y
		local z = object.z
 
		local oX, oY, onScreen = self:findPointOnScreen(x, y, z)
 
		if (oX ~= nil) then
			local modelName = object.model
			if (modelName == "flip") then
				local oWidth = object.width
				local oHeight = object.height
 
				local x1, y1 = self:findPointOnScreen(x, y - 0.5 + oHeight, z)
				local x2, y2 = self:findPointOnScreen(x, y - 0.5, z)
 
				local height = math.abs(y2 - y1)
				local width = 1
				if (self.blittleOn) then
					width = math.abs(height * oWidth / oHeight)
				else
					width = math.abs(height * oWidth / oHeight * 1.5)
				end
 
				height = math.floor(height)
				width = math.floor(width)
 
				local color = object.color
 
				if (x1 ~= nil and y1 ~= nil and x2 ~= nil and y2 ~= nil) then 
					self.buffer:loadBox(x1 - (0.5 * width), y1, x2 + (0.5 * width), y2, color, color, " ")
				end
			else
				local model = {}
				if (self.models[modelName] == nil) then
					self:loadModel(modelName, loadModel(modelName, self.modelsDir))
				end
				model = self.models[modelName]
 
				local rotatedModel = object.rotationY ~= nil and object.rotationY ~= 0 and rotateModel(model, object.rotationY) or model
				local sortedPolygons = sortPolygons(rotatedModel, x, y, z, self.cameraX, self.cameraY, self.cameraZ)

				if (self.saveSortedModelsBool) then
					if (object.rotationY == nil or object.rotationY == 0) then
						if (self.modelsSorted[modelName] == nil) then
							self.models[modelName] = sortedPolygons
							self.modelsSorted[modelName] = true
						end
					end
				end
 
				for i = 1, #sortedPolygons do
					local polygon = sortedPolygons[i]
					
					local x1, y1, onScreen1 = self:findPointOnScreen(x + polygon[1], y + polygon[2], z + polygon[3])
					if (onScreen1) then
						local x2, y2, onScreen2 = self:findPointOnScreen(x + polygon[4], y + polygon[5], z + polygon[6])
						if (onScreen2) then
							local x3, y3, onScreen3 = self:findPointOnScreen(x + polygon[7], y + polygon[8], z + polygon[9])
							if (onScreen3) then
								if (polygon[10] or isFacingCamera({x = x1, y = y1}, {x = x2, y = y2}, {x = x3, y = y3})) then
									self.buffer:loadTriangle(round(x1), round(y1), round(x2), round(y2), round(x3), round(y3), polygon[11], polygon[12], polygon[13])
								end
							end
						end
					end
				end
			end
		end
	end
 
	function frame:loadObjects(objects)
		if (not self.loadedBackgroundColor) then
			self.buffer:clear(colors.white)
		end

		local sortedObjects = sortObjects(objects, self.cameraX, self.cameraY, self.cameraZ, self.renderDistance)
 
 		self.modelsSorted = {}
		for i = 1, #sortedObjects do
			self:loadObject(sortedObjects[i])
		end

		self.loadedBackgroundColor = false

		return sortedObjects
	end
 
	function frame:drawBuffer()
		self.buffer:drawBuffer(self.blittleOn)
	end
 
	function frame:loadGround(color, char, charc)
		local distance = 10
		local gX = distance * math.cos(self.cameraDirY) + self.cameraX
		local gZ = distance * math.sin(self.cameraDirY) + self.cameraZ
		local x, y = self:findPointOnScreen(gX, self.cameraY - 0.01, gZ)

		if (y < 0) then y = 0 end
		if (y > (self.frameY2 - self.frameY1 + 2) * 3) then y = (self.frameY2 - self.frameY1 + 2) * 3 end
 
		if (not self.blittleOn) then
			self.buffer:loadBox(1, y, self.frameX2 - self.frameX1 + 1, self.frameY2 - self.frameY1 + 2, charc or color, color, char or " ")
		else
			self.buffer:loadBox(1, y, (self.frameX2 - self.frameX1 + 1) * 2, (self.frameY2 - self.frameY1 + 2) * 3, charc or color, color, char or " ")
		end

		self.loadedBackgroundColor = true
	end
 
	function frame:loadSky(color, char, charc)
		local distance = 10
		local gX = distance * math.cos(self.cameraDirY) + self.cameraX
		local gZ = distance * math.sin(self.cameraDirY) + self.cameraZ
		local x, y = self:findPointOnScreen(gX, self.cameraY - 0.01, gZ)
 
		if (y < 0) then y = 0 end
		if (y > (self.frameY2 - self.frameY1 + 2) * 3) then y = (self.frameY2 - self.frameY1 + 2) * 3 end
		
		if (not self.blittleOn) then
			self.buffer:loadBox(1, 1, self.frameX2 - self.frameX1 + 1, y, charc or color, color, char or " ")
		else
			self.buffer:loadBox(1, 1, (self.frameX2 - self.frameX1 + 1) * 2, y, charc or color, color, char or " ")
		end

		self.loadedBackgroundColor = true
	end

	function frame:getObjectIndexTrace(objects, x, y, alternative)
		local solutions = {}
		if (self.blittleOn) then
			x = x * 2
			y = y * 3
		end

		for i = 1, #objects do
			local object = objects[i]

			local model = {}
			if (self.models[object.model] ~= nil) then
				model = self.models[object.model]
			else
				model = loadModel(object.model, self.modelsDir)
				self.models[object.model] = model
			end

			local rotatedModel = object.rotationY ~= nil and object.rotationY ~= 0 and rotateModel(model, object.rotationY) or model

			for j = 1, #rotatedModel do
				local polygon = rotatedModel[j]
				
				local x1, y1, onScreen1 = self:findPointOnScreen(object.x + polygon[1], object.y + polygon[2], object.z + polygon[3])
				local x2, y2, onScreen2 = self:findPointOnScreen(object.x + polygon[4], object.y + polygon[5], object.z + polygon[6])
				local x3, y3, onScreen3 = self:findPointOnScreen(object.x + polygon[7], object.y + polygon[8], object.z + polygon[9])

				if (polygon[10] or isFacingCamera({x = x1, y = y1}, {x = x2, y = y2}, {x = x3, y = y3})) then
					if (onScreen1 and onScreen2 and onScreen3) then
						if (not self.blittleOn) then
							if (isInTriangle(x, y, round(x1), round(y1), round(x2), round(y2), round(x3), round(y3), self.frameX1, self.frameY1, self.frameX2, self.frameY2, alternative)) then
								solutions[#solutions+1] = {objectIndex = i, polygonIndex = j}
							end
						else
							if (isInTriangle(x, y, round(x1), round(y1), round(x2), round(y2), round(x3), round(y3), (self.frameX1 - 1) * 2 + 1, (self.frameY1 - 1) * 3 + 1, (self.frameX2) * 2, (self.frameY2 + 1) * 3, alternative)) then
								solutions[#solutions+1] = {objectIndex = i, polygonIndex = j}
							end
						end
					end
				end
			end
		end

		if (#solutions <= 0) then
			return
		end

		local objectSolution = {}

		local closestObject = -1
		local closestObjectDistance = math.huge
		for i = 1, #solutions do
			local object = objects[solutions[i].objectIndex]

			local dX = self.cameraX - object.x
			local dY = self.cameraY - object.y
			local dZ = self.cameraZ - object.z
			local distance = math.pow(dX*dX + dY*dY + dZ*dZ, 0.5)

			if (distance < closestObjectDistance) then
				closestObjectDistance = distance
				closestObject = solutions[i].objectIndex
			end
		end
		for i = 1, #solutions do
			local object = objects[solutions[i].objectIndex]

			local dX = self.cameraX - object.x
			local dY = self.cameraY - object.y
			local dZ = self.cameraZ - object.z
			local distance = math.pow(dX*dX + dY*dY + dZ*dZ, 0.5)

			if (distance == closestObjectDistance) then
				objectSolution[#objectSolution+1] = solutions[i].polygonIndex
			end
		end

		local object = objects[closestObject]
		local model = {}
		if (self.models[object.model] ~= nil) then
			model = self.models[object.model]
		else
			model = loadModel(object.model, self.modelsDir)
			self.models[object.model] = model
		end

		local closestPolygon = -1
		local closestPolygonDistance = math.huge

		for i = 1, #objectSolution do
			local polygon = model[objectSolution[i]]

			local avgX = (math.min(unpack({polygon[1] + object.x, polygon[4] + object.x, polygon[7] + object.x})) + math.max(unpack({polygon[1] + object.x, polygon[4] + object.x, polygon[7] + object.x}))) / 2
			local avgY = (math.min(unpack({polygon[2] + object.y, polygon[5] + object.y, polygon[8] + object.y})) + math.max(unpack({polygon[2] + object.y, polygon[5] + object.y, polygon[8] + object.y}))) / 2
			local avgZ = (math.min(unpack({polygon[3] + object.z, polygon[6] + object.z, polygon[9] + object.z})) + math.max(unpack({polygon[3] + object.z, polygon[6] + object.z, polygon[9] + object.z}))) / 2

			local dX = self.cameraX - avgX
			local dY = self.cameraY - avgY
			local dZ = self.cameraZ - avgZ

			local distance = math.pow(dX*dX + dY*dY + dZ*dZ, 0.5)

			if (distance < closestPolygonDistance) then
				closestPolygonDistance = distance
				closestPolygon = objectSolution[i]
			end
		end

		return closestObject, closestPolygon
	end
 
	return frame
end