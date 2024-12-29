local shared = {}
shared.LoaderTitle = 'EZ HUB LOADING...' -- ใส่ชื่อ
shared.LoaderKeyFrames = {
   [1] = {1, 30}, -- [Time (s), Percentage] 
   [2] = {3, 100} -- [เวลา, เปอร์เซ็น]
}

local Metadata = {
	LoaderData = {
		Name = (shared.LoaderTitle or 'EZ HUB LOADING...'),
		Colors = shared.LoaderColors or {
			Main = Color3.fromRGB(24, 24, 24),
			Topic = Color3.fromRGB(0, 135, 251),
			Title = Color3.fromRGB(0, 135, 251),
			LoaderBackground = Color3.fromRGB(30, 30, 30),
			LoaderSplash = Color3.fromRGB(255, 255, 255)
		}
	},
	Keyframes = shared.LoaderKeyFrames or {
		[1] = {1, 10}, -- [Time (s), Percentage]
		[2] = {2, 30},
		[3] = {3, 60},
		[4] = {2, 100}
	}
}

local function tweenObject(object, speed, info)
	game.TweenService:Create(object, TweenInfo.new(speed, Enum.EasingStyle.Linear, Enum.EasingDirection.InOut), info):Play()
end

local function createObject(className, properties)
	local instance = Instance.new(className)
	local parent
	for propertyName, propertyValue in pairs(properties) do
		if propertyName ~= "Parent" then
			instance[propertyName] = propertyValue
		else
			parent = propertyValue
		end
	end
	instance.Parent = parent
	return instance
end

-- สร้าง UI ใน CoreGui
local CoreGui = createObject("ScreenGui", {
	Name = "LoaderCore",
	Parent = game.CoreGui
})

-- ส่วน MainFrame สำหรับ UI
local MainFrame = createObject("Frame", {
	Name = "MainFrame",
	Parent = CoreGui,
	BackgroundColor3 = Metadata.LoaderData.Colors.Main,
	BorderSizePixel = 0,
	ClipsDescendants = true,
	Position = UDim2.new(0.5, 0, 0.5, 0),
	AnchorPoint = Vector2.new(0.5, 0.5),
	Size = UDim2.new(0, 0, 0, 0),
	BackgroundTransparency = 0.6 -- ความโปร่งแสง
})

-- Adding rounded corners to MainFrame using UICorner
local UICornerMain = createObject("UICorner", {
	Parent = MainFrame,
	CornerRadius = UDim.new(0, 10) -- ขอบโค้งมน
})

-- ส่วน Header สำหรับหัวข้อ
local Header = createObject("TextLabel", {
	Name = "Header",
	TextTransparency = 1,
	Parent = MainFrame,
	BackgroundColor3 = Color3.fromRGB(255, 255, 255),
	BackgroundTransparency = 1,
	Position = UDim2.new(0, 30, 0, 8),
	Size = UDim2.new(0, 301, 0, 50),
	Font = Enum.Font.Gotham,
	Text = "Loading...",
	TextColor3 = Metadata.LoaderData.Colors.Topic,
	TextSize = 14,
	TextXAlignment = Enum.TextXAlignment.Left,
})

-- ส่วน Title สำหรับชื่อ Loader
local LoaderTitle = createObject("TextLabel", {
	Name = "LoaderTitle",
	Parent = MainFrame,
	TextTransparency = 1,
	BackgroundColor3 = Color3.fromRGB(255, 255, 255),
	BackgroundTransparency = 1,
	Position = UDim2.new(0, 30, 0, 27),
	Size = UDim2.new(0, 301, 0, 46),
	Font = Enum.Font.Gotham,
	RichText = true,
	Text = "<b>" .. Metadata.LoaderData.Name .. "</b>",
	TextColor3 = Metadata.LoaderData.Colors.Title,
	TextSize = 16,
	TextXAlignment = Enum.TextXAlignment.Left,
})

-- ส่วน BackgroundFrame สำหรับ Progress bar
local BackgroundFrame = createObject("Frame", {
	Name = "BackgroundFrame",
	Parent = MainFrame,
	AnchorPoint = Vector2.new(0.5, 0),
	BackgroundTransparency = 1,
	BackgroundColor3 = Metadata.LoaderData.Colors.LoaderBackground,
	BorderSizePixel = 0,
	Position = UDim2.new(0.5, 0, 0, 70),
	Size = UDim2.new(0.85, 0, 0, 24),
})

-- Adding rounded corners to BackgroundFrame using UICorner
local UICornerBackground = createObject("UICorner", {
	Parent = BackgroundFrame,
	CornerRadius = UDim.new(0, 5) -- ขอบโค้งมน
})

-- ส่วน ProgressBar สำหรับแสดงสถานะการโหลด
local ProgressBar = createObject("Frame", {
	Name = "ProgressBar",
	Parent = BackgroundFrame,
	BackgroundColor3 = Metadata.LoaderData.Colors.LoaderSplash,
	BackgroundTransparency = 1,
	BorderSizePixel = 0,
	Size = UDim2.new(0, 0, 0, 24),
})

-- Adding rounded corners to ProgressBar using UICorner
local UICornerProgressBar = createObject("UICorner", {
	Parent = ProgressBar,
	CornerRadius = UDim.new(0, 5) -- ขอบโค้งมน
})

-- ฟังก์ชั่นเพิ่มเงาให้กับ UI
local function addShadow(frame)
	local shadow = createObject("ImageLabel", {
		Parent = frame,
		BackgroundTransparency = 1,
		Image = "rbxassetid://2527998236", -- รูปภาพเงา
		Size = UDim2.new(1, 10, 1, 10),
		Position = UDim2.new(0, -5, 0, -5),
		ZIndex = frame.ZIndex - 1,
	})
	shadow.ImageColor3 = Color3.fromRGB(0, 0, 0)
	shadow.ImageTransparency = 0.5
end

-- เพิ่มเงาให้กับ MainFrame และ BackgroundFrame
addShadow(MainFrame)
addShadow(BackgroundFrame)

-- ฟังก์ชั่นในการอัพเดต Progress bar
local function updatePercentage(percentage)
	tweenObject(ProgressBar, 0.5, {
		Size = UDim2.new((percentage / 100), 0, 0, 24)
	})
end

-- เริ่มการโหลด
tweenObject(MainFrame, 0.25, {
	Size = UDim2.new(0, 346, 0, 121)
})

wait(0.25)
tweenObject(Header, 0.5, {
	TextTransparency = 0
})
tweenObject(LoaderTitle, 0.5, {
	TextTransparency = 0
})
tweenObject(BackgroundFrame, 0.5, {
	BackgroundTransparency = 0
})
tweenObject(ProgressBar, 0.5, {
	BackgroundTransparency = 0
})

-- อัพเดต Progress ตาม Keyframes
for i, v in pairs(Metadata.Keyframes) do
	wait(v[1])
	updatePercentage(v[2])
end
updatePercentage(100)

-- ซ่อน UI หลังจากโหลดเสร็จ
tweenObject(Header, 0.5, {
	TextTransparency = 1
})
tweenObject(LoaderTitle, 0.5, {
	TextTransparency = 1
})
tweenObject(BackgroundFrame, 0.5, {
	BackgroundTransparency = 1
})
tweenObject(ProgressBar, 0.5, {
	BackgroundTransparency = 1
})

wait(0.5)
tweenObject(MainFrame, 0.25, {
	Size = UDim2.new(0, 0, 0, 0)
})

wait(0.25)
CoreGui:Destroy()
