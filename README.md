local Players = game:GetService("Players")
local UserInputService = game:GetService("UserInputService")
local TweenService = game:GetService("TweenService")

local player = Players.LocalPlayer
local playerGui = player:WaitForChild("PlayerGui")

local function make(class, props)
	local obj = Instance.new(class)
	for k, v in pairs(props or {}) do
		obj[k] = v
	end
	return obj
end

local function roundify(obj, radius)
	local c = Instance.new("UICorner")
	c.CornerRadius = UDim.new(0, radius)
	c.Parent = obj
end

local function addStroke(obj, color, thickness)
	local s = Instance.new("UIStroke")
	s.Color = color
	s.Thickness = thickness or 1
	s.Transparency = 0
	s.Parent = obj
end

local function tween(obj, info, goal)
	local tween = TweenService:Create(obj, info, goal)
	tween:Play()
	return tween
end

local theme = {
	Background = Color3.fromRGB(7, 9, 13),
	Background2 = Color3.fromRGB(11, 13, 18),
	Sidebar = Color3.fromRGB(12, 13, 19),
	SidebarSelected = Color3.fromRGB(19, 21, 27),
	Panel = Color3.fromRGB(16, 18, 23),
	Panel2 = Color3.fromRGB(19, 21, 26),
	Border = Color3.fromRGB(48, 52, 58),
	Text = Color3.fromRGB(234, 234, 234),
	TextDim = Color3.fromRGB(165, 165, 165),
	Muted = Color3.fromRGB(123, 123, 123),
	Yellow = Color3.fromRGB(247, 213, 61),
	YellowDark = Color3.fromRGB(190, 145, 0),
	Purple = Color3.fromRGB(147, 104, 255),
	Teal = Color3.fromRGB(122, 240, 255),
	Off = Color3.fromRGB(128, 128, 128),
	White = Color3.fromRGB(255, 255, 255),
}

local gui = make("ScreenGui", {
	Name = "DucklerMorgan",
	ResetOnSpawn = false,
	IgnoreGuiInset = true,
	ZIndexBehavior = Enum.ZIndexBehavior.Sibling,
	Parent = playerGui,
})

local window = make("Frame", {
	AnchorPoint = Vector2.new(0.5, 0.5),
	Position = UDim2.new(0.5, 0, 0.5, 0),
	Size = UDim2.new(0.8, 0, 0.9, 0),
	BackgroundColor3 = theme.Background,
	BorderSizePixel = 0,
	Parent = gui,
})
roundify(window, 0)

local topbar = make("Frame", {
	Size = UDim2.new(1, 0, 0, 62),
	BackgroundColor3 = theme.Panel2,
	BorderSizePixel = 0,
	Parent = window,
})

local title = make("TextLabel", {
	Text = "duckler_morgan",
	Font = Enum.Font.GothamBold,
	TextSize = 28,
	TextColor3 = theme.Text,
	BackgroundTransparency = 1,
	Size = UDim2.new(0, 240, 1, 0),
	Position = UDim2.new(0, 18, 0, 0),
	TextXAlignment = Enum.TextXAlignment.Left,
	Parent = topbar,
})

local searchFrame = make("Frame", {
	AnchorPoint = Vector2.new(0.5, 0.5),
	Position = UDim2.new(0.5, 0, 0.5, 0),
	Size = UDim2.new(0.53, -10, 0, 40),
	BackgroundColor3 = theme.Panel,
	BorderSizePixel = 0,
	Parent = topbar,
})
roundify(searchFrame, 8)
addStroke(searchFrame, theme.Border, 1)

local searchIcon = make("TextLabel", {
	Text = "⌕",
	Font = Enum.Font.Gotham,
	TextSize = 22,
	TextColor3 = theme.TextDim,
	BackgroundTransparency = 1,
	Size = UDim2.new(0, 32, 1, 0),
	Position = UDim2.new(0, 12, 0, 0),
	TextXAlignment = Enum.TextXAlignment.Center,
	Parent = searchFrame,
})

local searchBox = make("TextBox", {
	Text = "Search",
	Font = Enum.Font.Gotham,
	TextSize = 18,
	TextColor3 = theme.TextDim,
	BackgroundTransparency = 1,
	Position = UDim2.new(0, 44, 0, 0),
	Size = UDim2.new(1, -50, 1, 0),
	TextXAlignment = Enum.TextXAlignment.Left,
	ClearTextOnFocus = false,
	Parent = searchFrame,
})

local topButton = make("TextButton", {
	Text = "↗",
	Font = Enum.Font.GothamBold,
	TextSize = 20,
	TextColor3 = theme.Text,
	BackgroundColor3 = theme.Panel,
	BorderSizePixel = 0,
	Size = UDim2.new(0, 42, 0, 38),
	Position = UDim2.new(1, -52, 0.5, -19),
	Parent = topbar,
})
roundify(topButton, 8)
addStroke(topButton, theme.Border, 1)

local sidebar = make("Frame", {
	Size = UDim2.new(0, 260, 1, -62),
	Position = UDim2.new(0, 0, 0, 62),
	BackgroundColor3 = theme.Sidebar,
	BorderSizePixel = 0,
	Parent = window,
})

local sidebarDivider = make("Frame", {
	Size = UDim2.new(0, 1, 1, 0),
	Position = UDim2.new(1, 0, 0, 0),
	BackgroundColor3 = theme.Border,
	BorderSizePixel = 0,
	Parent = sidebar,
})

local content = make("Frame", {
	Size = UDim2.new(1, -260, 1, -62),
	Position = UDim2.new(0, 260, 0, 62),
	BackgroundColor3 = theme.Background2,
	BorderSizePixel = 0,
	Parent = window,
})

local pages = {}
local tabButtons = {}

local function makePage()
	local page = make("Frame", {
		BackgroundTransparency = 1,
		Visible = false,
		Size = UDim2.new(1, 0, 1, 0),
		Parent = content,
	})
	return page
end

local function createSidebarTab(name, icon)
	local btn = make("TextButton", {
		Text = "",
		AutoButtonColor = false,
		BackgroundColor3 = theme.Sidebar,
		BorderSizePixel = 0,
		Size = UDim2.new(1, 0, 0, 58),
		Parent = sidebar,
	})

	local iconLabel = make("TextLabel", {
		Text = icon,
		Font = Enum.Font.GothamBold,
		TextSize = 18,
		TextColor3 = theme.Purple,
		BackgroundTransparency = 1,
		Size = UDim2.new(0, 24, 0, 24),
		Position = UDim2.new(0, 18, 0.5, -12),
		TextXAlignment = Enum.TextXAlignment.Center,
		Parent = btn,
	})

	local label = make("TextLabel", {
		Text = name,
		Font = Enum.Font.GothamMedium,
		TextSize = 20,
		TextColor3 = theme.Text,
		BackgroundTransparency = 1,
		Size = UDim2.new(1, -58, 1, 0),
		Position = UDim2.new(0, 54, 0, 0),
		TextXAlignment = Enum.TextXAlignment.Left,
		Parent = btn,
	})
	label.Name = "TabLabel"

	btn.MouseEnter:Connect(function()
		if not pages[name] or not pages[name].Visible then
			btn.BackgroundColor3 = theme.SidebarSelected
		end
	end)

	btn.MouseLeave:Connect(function()
		if not pages[name] or not pages[name].Visible then
			btn.BackgroundColor3 = theme.Sidebar
		end
	end)

	btn.MouseButton1Click:Connect(function()
		for pn, p in pairs(pages) do
			p.Visible = pn == name
		end

		for _, b in ipairs(tabButtons) do
			if b == btn then
				b.BackgroundColor3 = theme.SidebarSelected
				b.TabLabel.TextColor3 = theme.Yellow
			else
				b.BackgroundColor3 = theme.Sidebar
				b.TabLabel.TextColor3 = theme.Text
			end
		end
	end)

	table.insert(tabButtons, btn)
	return btn
end

local function createToggle(parent, textLabel, defaultValue)
	local frame = make("Frame", {
		BackgroundTransparency = 1,
		Size = UDim2.new(1, 0, 0, 28),
		Parent = parent,
	})

	local label = make("TextLabel", {
		Text = textLabel,
		Font = Enum.Font.Gotham,
		TextSize = 16,
		TextColor3 = theme.TextDim,
		BackgroundTransparency = 1,
		Size = UDim2.new(1, -90, 1, 0),
		Position = UDim2.new(0, 0, 0, 0),
		TextXAlignment = Enum.TextXAlignment.Left,
		Parent = frame,
	})

	local toggle = make("TextButton", {
		Text = "",
		AutoButtonColor = false,
		BackgroundColor3 = theme.Off,
		BorderSizePixel = 0,
		Size = UDim2.new(0, 38, 0, 20),
		Position = UDim2.new(1, -42, 0.5, -10),
		Parent = frame,
	})
	roundify(toggle, 12)

	local knob = make("Frame", {
		BackgroundColor3 = theme.White,
		BorderSizePixel = 0,
		Size = UDim2.new(0, 14, 0, 14),
		Position = UDim2.new(0, 3, 0.5, -7),
		Parent = toggle,
	})
	roundify(knob, 10)

	local enabled = defaultValue or false

	local function update()
		if enabled then
			toggle.BackgroundColor3 = theme.Purple
			knob.Position = UDim2.new(1, -17, 0.5, -7)
		else
			toggle.BackgroundColor3 = theme.Off
			knob.Position = UDim2.new(0, 3, 0.5, -7)
		end
	end

	toggle.MouseButton1Click:Connect(function()
		enabled = not enabled
		update()
	end)

	update()
	return frame
end

local function createSlider(parent, labelText, value, maxValue)
	local frame = make("Frame", {
		BackgroundTransparency = 1,
		Size = UDim2.new(1, 0, 0, 52),
		Parent = parent,
	})

	local label = make("TextLabel", {
		Text = labelText,
		Font = Enum.Font.Gotham,
		TextSize = 16,
		TextColor3 = theme.TextDim,
		BackgroundTransparency = 1,
		Size = UDim2.new(1, -120, 0, 24),
		Position = UDim2.new(0, 0, 0, 0),
		TextXAlignment = Enum.TextXAlignment.Left,
		Parent = frame,
	})

	local valueText = make("TextLabel", {
		Text = tostring(value) .. "/" .. tostring(maxValue),
		Font = Enum.Font.GothamBold,
		TextSize = 15,
		TextColor3 = theme.TextDim,
		BackgroundTransparency = 1,
		Size = UDim2.new(0, 100, 0, 24),
		Position = UDim2.new(1, -100, 0, 0),
		TextXAlignment = Enum.TextXAlignment.Right,
		Parent = frame,
	})

	local bar = make("Frame", {
		BackgroundColor3 = theme.Panel2,
		BorderSizePixel = 0,
		Size = UDim2.new(1, 0, 0, 12),
		Position = UDim2.new(0, 0, 0, 28),
		Parent = frame,
	})
	roundify(bar, 6)
	addStroke(bar, theme.Border, 1)

	local fill = make("Frame", {
		BackgroundColor3 = theme.Purple,
		BorderSizePixel = 0,
		Size = UDim2.new(math.clamp(value / maxValue, 0, 1), 0, 1, 0),
		Parent = bar,
	})
	roundify(fill, 6)

	return frame
end

local function createInput(parent, placeholder, defaultText)
	local box = make("TextBox", {
		BackgroundColor3 = theme.Panel,
		BorderSizePixel = 0,
		Size = UDim2.new(1, 0, 0, 34),
		Text = defaultText or "",
		PlaceholderText = placeholder,
		PlaceholderColor3 = theme.TextDim,
		TextColor3 = theme.Text,
		Font = Enum.Font.Gotham,
		TextSize = 16,
		Parent = parent,
	})
	roundify(box, 7)
	addStroke(box, theme.Border, 1)
	return box
end

local function createButton(parent, text)
	local btn = make("TextButton", {
		Text = text,
		Font = Enum.Font.GothamMedium,
		TextSize = 16,
		TextColor3 = theme.Text,
		BackgroundColor3 = theme.Panel,
		BorderSizePixel = 0,
		Size = UDim2.new(1, 0, 0, 34),
		Parent = parent,
	})
	roundify(btn, 7)
	addStroke(btn, theme.Border, 1)
	return btn
end

local function createSection(parent, titleText, isOpen)
	local sec = make("Frame", {
		BackgroundTransparency = 1,
		Size = UDim2.new(1, 0, 0, 0),
		Parent = parent,
	})

	local header = make("TextButton", {
		Text = titleText,
		Font = Enum.Font.GothamMedium,
		TextSize = 18,
		TextColor3 = theme.Text,
		BackgroundTransparency = 1,
		Size = UDim2.new(1, -12, 0, 42),
		Position = UDim2.new(0, 0, 0, 0),
		TextXAlignment = Enum.TextXAlignment.Left,
		Parent = sec,
	})
	header.AutoButtonColor = false

	local arrow = make("TextLabel", {
		Text = isOpen and "⌃" or "⌄",
		Font = Enum.Font.GothamBold,
		TextSize = 16,
		TextColor3 = theme.TextDim,
		BackgroundTransparency = 1,
		Size = UDim2.new(0, 18, 0, 18),
		Position = UDim2.new(1, -20, 0.5, -9),
		TextXAlignment = Enum.TextXAlignment.Right,
		Parent = header,
	})

	local body = make("Frame", {
		BackgroundTransparency = 1,
		Size = UDim2.new(1, 0, 0, 0),
		Position = UDim2.new(0, 0, 0, 42),
		Parent = sec,
	})

	local open = isOpen ~= false

	local function update()
		body.Visible = open
		arrow.Text = open and "⌃" or "⌄"
	end

	header.MouseButton1Click:Connect(function()
		open = not open
		update()
	end)

	update()
	return sec, body
end

-- Pages
local mainPage = makePage()
pages["Main"] = mainPage
createSidebarTab("Main", "✦")

local visualPage = makePage()
pages["Visual"] = visualPage
createSidebarTab("Visual", "◉")

local movementPage = makePage()
pages["Movement"] = movementPage
createSidebarTab("Movement", "✚")

local spoofPage = makePage()
pages["Spoof"] = spoofPage
createSidebarTab("Spoof", "◍")

local settingsPage = makePage()
pages["Settings"] = settingsPage
createSidebarTab("Settings", "⚙")

-- Main page
local mainContent = make("Frame", {
	BackgroundTransparency = 1,
	Size = UDim2.new(1, -30, 1, -30),
	Position = UDim2.new(0, 15, 0, 20),
	Parent = mainPage,
})
local mainList = make("UIListLayout", {
	Padding = UDim.new(0, 14),
	SortOrder = Enum.SortOrder.LayoutOrder,
	Parent = mainContent,
})
createToggle(mainContent, "Enable Main", true)
createToggle(mainContent, "Visual", false)

-- Visual page
local visualContent = make("Frame", {
	BackgroundTransparency = 1,
	Size = UDim2.new(1, -30, 1, -30),
	Position = UDim2.new(0, 15, 0, 20),
	Parent = visualPage,
})
local visualList = make("UIListLayout", {
	Padding = UDim.new(0, 14),
	SortOrder = Enum.SortOrder.LayoutOrder,
	Parent = visualContent,
})
createToggle(visualContent, "Enable ESP", false)
createToggle(visualContent, "Highlight", false)
createToggle(visualContent, "Box", false)

-- Movement page
local movementContent = make("Frame", {
	BackgroundTransparency = 1,
	Size = UDim2.new(1, -30, 1, -30),
	Position = UDim2.new(0, 15, 0, 20),
	Parent = movementPage,
})
local movementList = make("UIListLayout", {
	Padding = UDim.new(0, 14),
	SortOrder = Enum.SortOrder.LayoutOrder,
	Parent = movementContent,
})
createSlider(movementContent, "Walk Speed", 2, 10)
createSlider(movementContent, "Jump Power", 2, 10)
createToggle(movementContent, "NoClip", false)

-- Spoof page
local spoofContent = make("Frame", {
	BackgroundTransparency = 1,
	Size = UDim2.new(1, -30, 1, -30),
	Position = UDim2.new(0, 15, 0, 20),
	Parent = spoofPage,
})
local spoofList = make("UIListLayout", {
	Padding = UDim.new(0, 14),
	SortOrder = Enum.SortOrder.LayoutOrder,
	Parent = spoofContent,
})
createToggle(spoofContent, "Enable Name Spoof", false)
createToggle(spoofContent, "Enable Device Spoof", false)

-- Settings page
local settingsContent = make("Frame", {
	BackgroundTransparency = 1,
	Size = UDim2.new(1, -26, 1, -20),
	Position = UDim2.new(0, 13, 0, 14),
	Parent = settingsPage,
})

local settingsGrid = make("Frame", {
	BackgroundTransparency = 1,
	Size = UDim2.new(1, 0, 1, 0),
	Parent = settingsContent,
})
local settingsLayout = make("UIListLayout", {
	FillDirection = Enum.FillDirection.Horizontal,
	Padding = UDim.new(0, 18),
	SortOrder = Enum.SortOrder.LayoutOrder,
	Parent = settingsGrid,
})

local leftCol = make("Frame", {
	BackgroundTransparency = 1,
	Size = UDim2.new(0.48, 0, 1, 0),
	Parent = settingsGrid,
})
local rightCol = make("Frame", {
	BackgroundTransparency = 1,
	Size = UDim2.new(0.48, 0, 1, 0),
	Parent = settingsGrid,
})

local keybindSec, keybindBody = createSection(leftCol, "Keybind", true)
keybindSec.Size = UDim2.new(1, 0, 0, 160)

local keybindInput = createInput(keybindBody, "RightShift", "RightShift")
local resetButton = createButton(keybindBody, "Reset Keybinds")
resetButton.Size = UDim2.new(1, 0, 0, 34)

local themeSec, themeBody = createSection(leftCol, "Themes", true)
themeSec.Position = UDim2.new(0, 0, 0, 172)
themeSec.Size = UDim2.new(1, 0, 0, 80)

local linksSec, linksBody = createSection(rightCol, "Links", true)
linksSec.Size = UDim2.new(1, 0, 0, 120)

local discordInput = createInput(linksBody, "discord.gg/duckler", "discord.gg/duckler")
local joinBtn = createButton(linksBody, "Join Discord")

local configSec, configBody = createSection(rightCol, "Configuration", true)
configSec.Position = UDim2.new(0, 0, 0, 130)
configSec.Size = UDim2.new(1, 0, 0, 180)

-- Footer
local footer = make("TextLabel", {
	Text = "discord.gg/duckler",
	Font = Enum.Font.Gotham,
	TextSize = 18,
	TextColor3 = theme.TextDim,
	BackgroundTransparency = 1,
	Size = UDim2.new(1, 0, 0, 24),
	Position = UDim2.new(0, 0, 1, -26),
	TextXAlignment = Enum.TextXAlignment.Center,
	Parent = window,
})

-- Default selected tab
for _, b in ipairs(tabButtons) do
	if b.TabLabel.Text == "Settings" then
		b.BackgroundColor3 = theme.SidebarSelected
		b.TabLabel.TextColor3 = theme.Yellow
	else
		b.BackgroundColor3 = theme.Sidebar
		b.TabLabel.TextColor3 = theme.Text
	end
end
pages["Settings"].Visible = true

-- Search text behavior
searchBox.Focused:Connect(function()
	if searchBox.Text == "Search" then
		searchBox.Text = ""
	end
end)

searchBox.FocusLost:Connect(function()
	if searchBox.Text == "" then
		searchBox.Text = "Search"
	end
end)

-- Dragging
local dragging = false
local dragStart
local startPos

topbar.InputBegan:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.MouseButton1 then
		dragging = true
		dragStart = input.Position
		startPos = window.Position
	end
end)

topbar.InputChanged:Connect(function(input)
	if dragging and input.UserInputType == Enum.UserInputType.MouseMovement then
		local delta = input.Position - dragStart
		window.Position = UDim2.new(
			startPos.X.Scale,
			startPos.X.Offset + delta.X,
			startPos.Y.Scale,
			startPos.Y.Offset + delta.Y
		)
	end
end)

topbar.InputEnded:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.MouseButton1 then
		dragging = false
	end
end)

-- Toggle UI with RightShift
local uiVisible = true
UserInputService.InputBegan:Connect(function(input, gameProcessed)
	if gameProcessed then return end
	if input.KeyCode == Enum.KeyCode.RightShift then
		uiVisible = not uiVisible
		gui.Enabled = uiVisible
	end
end)

return gui
