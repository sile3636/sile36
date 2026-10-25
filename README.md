# --!strict
--[[\
	MobileFixLagMenu.client.lua
	Client-side Mobile Utility cho Roblox (Solid Grey Map / Smooth Plastic)
]]--

local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local Lighting = game:GetService("Lighting")
local Workspace = game:GetService("Workspace")

local LocalPlayer = Players.LocalPlayer
local playerGui = LocalPlayer:WaitForChild("PlayerGui")

--------------------------------------------------------------------------------
-- REMOVE EXISTING GUI
--------------------------------------------------------------------------------
local existingGui = playerGui:FindFirstChild("SileMobileGui")
if existingGui then existingGui:Destroy() end

--------------------------------------------------------------------------------
-- UI CONSTRUCTION
--------------------------------------------------------------------------------
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "SileMobileGui"
screenGui.ResetOnSpawn = false
screenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
screenGui.Parent = playerGui

-- 1. THANH TÊN "SILE" Ở TRÊN CÙNG MÀN HÌNH
local topHeader = Instance.new("Frame")
topHeader.Name = "TopHeader"
topHeader.Size = UDim2.new(0, 150, 0, 26)
topHeader.Position = UDim2.new(0.5, -75, 0, 8)
topHeader.BackgroundColor3 = Color3.fromRGB(12, 14, 22)
topHeader.BackgroundTransparency = 0.2
topHeader.Parent = screenGui

Instance.new("UICorner", topHeader).CornerRadius = UDim.new(0, 8)
local topHeaderStroke = Instance.new("UIStroke")
topHeaderStroke.Color = Color3.fromRGB(0, 210, 255)
topHeaderStroke.Thickness = 1.2
topHeaderStroke.Transparency = 0.3
topHeaderStroke.Parent = topHeader

local topHeaderText = Instance.new("TextLabel")
topHeaderText.Size = UDim2.new(1, 0, 1, 0)
topHeaderText.BackgroundTransparency = 1
topHeaderText.Font = Enum.Font.GothamBold
topHeaderText.Text = "⚡ SILE UTILITY ⚡"
topHeaderText.TextColor3 = Color3.fromRGB(0, 220, 255)
topHeaderText.TextSize = 11
topHeaderText.Parent = topHeader

-- 2. KHUNG FPS ĐẶT Ở NGOÀI MÀN HÌNH
local fpsWidget = Instance.new("Frame")
fpsWidget.Name = "FPSWidget"
fpsWidget.Size = UDim2.new(0, 90, 0, 26)
fpsWidget.Position = UDim2.new(1, -105, 0, 8)
fpsWidget.BackgroundColor3 = Color3.fromRGB(12, 14, 22)
fpsWidget.BackgroundTransparency = 0.2
fpsWidget.Parent = screenGui

Instance.new("UICorner", fpsWidget).CornerRadius = UDim.new(0, 8)
local fpsWidgetStroke = Instance.new("UIStroke")
fpsWidgetStroke.Color = Color3.fromRGB(0, 255, 160)
fpsWidgetStroke.Thickness = 1.2
fpsWidgetStroke.Transparency = 0.3
fpsWidgetStroke.Parent = fpsWidget

local fpsWidgetText = Instance.new("TextLabel")
fpsWidgetText.Size = UDim2.new(1, 0, 1, 0)
fpsWidgetText.BackgroundTransparency = 1
fpsWidgetText.Font = Enum.Font.GothamBold
fpsWidgetText.Text = "60 FPS"
fpsWidgetText.TextColor3 = Color3.fromRGB(0, 255, 160)
fpsWidgetText.TextSize = 11
fpsWidgetText.Parent = fpsWidget

-- 3. NÚT NỔI MENU CẢM ỨNG
local toggleBtn = Instance.new("TextButton")
toggleBtn.Name = "MobileToggleBtn"
toggleBtn.Size = UDim2.new(0, 50, 0, 50)
toggleBtn.Position = UDim2.new(0, 15, 0.35, 0)
toggleBtn.BackgroundColor3 = Color3.fromRGB(15, 18, 28)
toggleBtn.BackgroundTransparency = 0.15
toggleBtn.Font = Enum.Font.GothamBold
toggleBtn.Text = "⚡"
toggleBtn.TextColor3 = Color3.fromRGB(0, 220, 255)
toggleBtn.TextSize = 22
toggleBtn.Parent = screenGui

Instance.new("UICorner", toggleBtn).CornerRadius = UDim.new(1, 0)
local toggleStroke = Instance.new("UIStroke")
toggleStroke.Color = Color3.fromRGB(0, 190, 255)
toggleStroke.Thickness = 1.8
toggleStroke.Parent = toggleBtn

-- 4. KHUNG MENU CHÍNH
local mainFrame = Instance.new("Frame")
mainFrame.Name = "MainFrame"
mainFrame.Size = UDim2.new(0, 310, 0, 270)
mainFrame.Position = UDim2.new(0.5, -155, 0.5, -135)
mainFrame.BackgroundColor3 = Color3.fromRGB(12, 14, 22)
mainFrame.BackgroundTransparency = 0.15
mainFrame.BorderSizePixel = 0
mainFrame.Visible = false
mainFrame.Parent = screenGui

Instance.new("UICorner", mainFrame).CornerRadius = UDim.new(0, 14)
local frameStroke = Instance.new("UIStroke")
frameStroke.Color = Color3.fromRGB(0, 170, 255)
frameStroke.Thickness = 1.5
frameStroke.Transparency = 0.3
frameStroke.Parent = mainFrame

local titleBar = Instance.new("Frame")
titleBar.Size = UDim2.new(1, 0, 0, 38)
titleBar.BackgroundTransparency = 1
titleBar.Parent = mainFrame

local titleText = Instance.new("TextLabel")
titleText.Size = UDim2.new(1, -40, 1, 0)
titleText.Position = UDim2.new(0, 14, 0, 0)
titleText.BackgroundTransparency = 1
titleText.Font = Enum.Font.GothamBold
titleText.Text = "SILE UTILITY SETTINGS"
titleText.TextColor3 = Color3.fromRGB(0, 210, 255)
titleText.TextSize = 12
titleText.TextXAlignment = Enum.TextXAlignment.Left
titleText.Parent = titleBar

local closeBtn = Instance.new("TextButton")
closeBtn.Size = UDim2.new(0, 28, 0, 28)
closeBtn.Position = UDim2.new(1, -32, 0.5, -14)
closeBtn.BackgroundTransparency = 1
closeBtn.Font = Enum.Font.GothamBold
closeBtn.Text = "✕"
closeBtn.TextColor3 = Color3.fromRGB(255, 90, 90)
closeBtn.TextSize = 14
closeBtn.Parent = closeBtn.Parent -- Fix parent pointer

local container = Instance.new("Frame")
container.Size = UDim2.new(1, -24, 1, -48)
container.Position = UDim2.new(0, 12, 0, 40)
container.BackgroundTransparency = 1
container.Parent = mainFrame

local uiLayout = Instance.new("UIListLayout")
uiLayout.Padding = UDim.new(0, 8)
uiLayout.Parent = container

--------------------------------------------------------------------------------
-- FPS MONITOR LOGIC
--------------------------------------------------------------------------------
local lastTime = os.clock()
local frameCount = 0

RunService.RenderStepped:Connect(function()
	frameCount += 1
	local currentTime = os.clock()
	if currentTime - lastTime >= 1 then
		local fps = math.floor(frameCount / (currentTime - lastTime))
		fpsWidgetText.Text = string.format("%d FPS", fps)
		
		if fps >= 50 then
			fpsWidgetText.TextColor3 = Color3.fromRGB(0, 255, 160)
			fpsWidgetStroke.Color = Color3.fromRGB(0, 255, 160)
		elseif fps >= 30 then
			fpsWidgetText.TextColor3 = Color3.fromRGB(255, 200, 50)
			fpsWidgetStroke.Color = Color3.fromRGB(255, 200, 50)
		else
			fpsWidgetText.TextColor3 = Color3.fromRGB(255, 80, 80)
			fpsWidgetStroke.Color = Color3.fromRGB(255, 80, 80)
		end
		
		frameCount = 0
		lastTime = currentTime
	end
end)

--------------------------------------------------------------------------------
-- FEATURE 1: GREY MAP (XÁM HÓA TOÀN BỘ BẢN ĐỒ)
--------------------------------------------------------------------------------
local originalColors: { [BasePart]: Color3 } = {}
local originalMaterials: { [BasePart]: Enum.Material } = {}
local disabledDecals: { [Decal]: number } = {}
local hiddenEffects: { [Instance]: boolean } = {}

local GREY_COLOR = Color3.fromRGB(120, 120, 120) -- Màu xám chuẩn Potato Graphics

local function toggleGreyMap(enabled: boolean)
	if enabled then
		Lighting.GlobalShadows = false
		Lighting.FogEnd = 9e9
		
		for _, obj in ipairs(Workspace:GetDescendants()) do
			-- Bỏ qua vật thể thuộc nhân vật của Player
			local isCharacterPart = false
			for _, plr in ipairs(Players:GetPlayers()) do
				if plr.Character and obj:IsDescendantOf(plr.Character) then
					isCharacterPart = true
					break
				end
			end
			
			if not isCharacterPart then
				-- Tắt các hạt hiệu ứng nặng
				if obj:IsA("ParticleEmitter") or obj:IsA("Trail") or obj:IsA("Beam") or obj:IsA("Smoke") or obj:IsA("Fire") or obj:IsA("Sparkles") then
					if obj.Enabled then
						hiddenEffects[obj] = true
						obj.Enabled = false
					end
				-- Ẩn decal/texture hình ảnh dán trên khối
				elseif obj:IsA("Decal") or obj:IsA("Texture") then
					disabledDecals[obj] = obj.Transparency
					obj.Transparency = 1
				-- Đổi tất cả vật thể map sang chất liệu SmoothPlastic màu xám
				elseif obj:IsA("BasePart") then
					originalColors[obj] = obj.Color
					originalMaterials[obj] = obj.Material
					
					obj.Material = Enum.Material.SmoothPlastic
					obj.Color = GREY_COLOR
				end
			end
		end
	else
		Lighting.GlobalShadows = true
		
		-- Khôi phục màu sắc & chất liệu nguyên bản
		for part, oldColor in pairs(originalColors) do
			if part and part.Parent then
				part.Color = oldColor
				part.Material = originalMaterials[part] or Enum.Material.SmoothPlastic
			end
		end
		table.clear(originalColors)
		table.clear(originalMaterials)
		
		for decal, oldTrans in pairs(disabledDecals) do
			if decal and decal.Parent then
				decal.Transparency = oldTrans
			end
		end
		table.clear(disabledDecals)
		
		for effect, _ in pairs(hiddenEffects) do
			if effect and effect.Parent then
				effect.Enabled = true
			end
		end
		table.clear(hiddenEffects)
	end
end

--------------------------------------------------------------------------------
-- FEATURE 2: ESP NAME & DISTANCE
--------------------------------------------------------------------------------
local espBillboards: { [Player]: BillboardGui } = {}

local function updateESP()
	for _, player in ipairs(Players:GetPlayers()) do
		if player ~= LocalPlayer and player.Character then
			local char = player.Character
			local head = char:FindFirstChild("Head") or char:FindFirstChild("HumanoidRootPart")
			
			if head then
				if not espBillboards[player] then
					local billboard = Instance.new("BillboardGui")
					billboard.Name = "SileESP"
					billboard.Size = UDim2.new(0, 120, 0, 40)
					billboard.StudsOffset = Vector3.new(0, 2.5, 0)
					billboard.AlwaysOnTop = true
					billboard.Adornee = head
					
					local textLabel = Instance.new("TextLabel")
					textLabel.Name = "Label"
					textLabel.Size = UDim2.new(1, 0, 1, 0)
					textLabel.BackgroundTransparency = 1
					textLabel.Font = Enum.Font.GothamBold
					textLabel.TextSize = 11
					textLabel.TextColor3 = Color3.fromRGB(0, 220, 255)
					textLabel.TextStrokeTransparency = 0.5
					textLabel.Parent = billboard
					
					billboard.Parent = screenGui
					espBillboards[player] = billboard
				else
					local billboard = espBillboards[player]
					local textLabel = billboard:FindFirstChild("Label") :: TextLabel
					if textLabel and LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart") then
						local myRoot = LocalPlayer.Character.HumanoidRootPart
						local targetRoot = char:FindFirstChild("HumanoidRootPart")
						if targetRoot then
							local distance = math.floor((myRoot.Position - targetRoot.Position).Magnitude)
							textLabel.Text = string.format("[%s]\n%dm", player.Name, distance)
						end
					end
				end
			end
		else
			if espBillboards[player] then
				espBillboards[player]:Destroy()
				espBillboards[player] = nil
			end
		end
	end
end

local espConnection: RBXScriptConnection? = nil

local function toggleESP(enabled: boolean)
	if enabled then
		espConnection = RunService.RenderStepped:Connect(updateESP)
	else
		if espConnection then
			espConnection:Disconnect()
			espConnection = nil
		end
		for player, billboard in pairs(espBillboards) do
			billboard:Destroy()
		end
		table.clear(espBillboards)
	end
end

--------------------------------------------------------------------------------
-- TOGGLE CREATOR
--------------------------------------------------------------------------------
local function createModernToggle(titleTextStr: string, subtitleStr: string, callback: (boolean) -> ())
	local frame = Instance.new("Frame")
	frame.Size = UDim2.new(1, 0, 0, 50)
	frame.BackgroundColor3 = Color3.fromRGB(20, 24, 36)
	frame.BackgroundTransparency = 0.3
	frame.Parent = container
	
	Instance.new("UICorner", frame).CornerRadius = UDim.new(0, 10)
	local itemStroke = Instance.new("UIStroke")
	itemStroke.Color = Color3.fromRGB(40, 50, 70)
	itemStroke.Thickness = 1
	itemStroke.Parent = frame
	
	local label = Instance.new("TextLabel")
	label.Size = UDim2.new(1, -65, 0, 18)
	label.Position = UDim2.new(0, 12, 0, 8)
	label.BackgroundTransparency = 1
	label.Font = Enum.Font.GothamBold
	label.Text = titleTextStr
	label.TextColor3 = Color3.fromRGB(230, 235, 245)
	label.TextSize = 12
	label.TextXAlignment = Enum.TextXAlignment.Left
	label.Parent = frame
	
	local subLabel = Instance.new("TextLabel")
	subLabel.Size = UDim2.new(1, -65, 0, 14)
	subLabel.Position = UDim2.new(0, 12, 0, 26)
	subLabel.BackgroundTransparency = 1
	subLabel.Font = Enum.Font.GothamMedium
	subLabel.Text = subtitleStr
	subLabel.TextColor3 = Color3.fromRGB(130, 140, 160)
	subLabel.TextSize = 10
	subLabel.TextXAlignment = Enum.TextXAlignment.Left
	subLabel.Parent = frame
	
	local switchBtn = Instance.new("TextButton")
	switchBtn.Size = UDim2.new(0, 42, 0, 22)
	switchBtn.Position = UDim2.new(1, -50, 0.5, -11)
	switchBtn.BackgroundColor3 = Color3.fromRGB(35, 40, 55)
	switchBtn.Font = Enum.Font.GothamBold
	switchBtn.Text = ""
	switchBtn.Parent = frame
	
	Instance.new("UICorner", switchBtn).CornerRadius = UDim.new(0, 11)
	
	local switchDot = Instance.new("Frame")
	switchDot.Size = UDim2.new(0, 16, 0, 16)
	switchDot.Position = UDim2.new(0, 3, 0.5, -8)
	switchDot.BackgroundColor3 = Color3.fromRGB(160, 170, 190)
	switchDot.Parent = switchBtn
	
	Instance.new("UICorner", switchDot).CornerRadius = UDim.new(1, 0)
	
	local enabled = false
	switchBtn.MouseButton1Click:Connect(function()
		enabled = not enabled
		switchBtn.BackgroundColor3 = enabled and Color3.fromRGB(0, 180, 255) or Color3.fromRGB(35, 40, 55)
		switchDot.Position = enabled and UDim2.new(1, -19, 0.5, -8) or UDim2.new(0, 3, 0.5, -8)
		switchDot.BackgroundColor3 = enabled and Color3.fromRGB(255, 255, 255) or Color3.fromRGB(160, 170, 190)
		itemStroke.Color = enabled and Color3.fromRGB(0, 180, 255) or Color3.fromRGB(40, 50, 70)
		callback(enabled)
	end)
end

--------------------------------------------------------------------------------
-- BUILD CONTROLS
--------------------------------------------------------------------------------
createModernToggle("Solid Grey Map", "Đổi toàn bộ bản đồ sang màu xám phẳng siêu mượt", toggleGreyMap)

createModernToggle("ESP Name & Distance", "Hiển thị tên và khoảng cách xuyên tường", toggleESP)

createModernToggle("Full Bright", "Độ sáng tối đa (Không bị tối map)", function(enabled)
	if enabled then
		Lighting.Brightness = 2
		Lighting.Ambient = Color3.fromRGB(200, 200, 200)
	end
end)

--------------------------------------------------------------------------------
-- INTERACTION & DRAGGING
--------------------------------------------------------------------------------
toggleBtn.MouseButton1Click:Connect(function()
	mainFrame.Visible = not mainFrame.Visible
end)

closeBtn.MouseButton1Click:Connect(function()
	mainFrame.Visible = false
end)

local dragging, dragStart, startPos
toggleBtn.InputBegan:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
		dragging = true
		dragStart = input.Position
		startPos = toggleBtn.Position
	end
end)

UserInputService.InputChanged:Connect(function(input)
	if dragging and (input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch) then
		local delta = input.Position - dragStart
		toggleBtn.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
	end
end)

UserInputService.InputEnded:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
		dragging = false
	end
end)
