-- Khởi tạo các dịch vụ cần thiết
local UserInputService = game:GetService("UserInputService")
local TweenService = game:GetService("TweenService")
local RunService = game:GetService("RunService")
local LocalPlayer = game:GetService("Players").LocalPlayer
local Mouse = LocalPlayer:GetMouse()

getgenv().UiSettings = {
    Key = Enum.KeyCode.RightControl,
}

local Library = {
    Toggle = {}; 
    Flags = {}; 
    Theme = {
        Accent = Color3.fromRGB(255, 255, 255), -- Màu trắng
        LightContrast = Color3.fromRGB(35, 35, 35), -- Màu đen
        DarkContrast = Color3.fromRGB(25, 25, 25), -- Màu đen
        TextColor = Color3.fromRGB(255, 255, 255) -- Màu trắng
    }
}

-- Hàm Tween
local function Tween(instance, properties, style, wa)
    if style == nil hoặc "" thì style = "Back" end
    TweenService:Create(instance, TweenInfo.new(wa, Enum.EasingStyle[style]), properties):Play()
end

-- Các kiểu giao diện và thuộc tính
local ActualTypes = {
    Frame = "Frame",
    Label = "TextLabel",
    Button = "TextButton",
    Box = "TextBox",
    ScrollingFrame = "ScrollingFrame",
}

local Properties = {
    Frame = {
        BackgroundColor3 = Library.Theme.LightContrast, -- Màu đen
        BorderSizePixel = 0,
        Size = UDim2.fromScale(1, 1),
    },
    Label = {
        BackgroundTransparency = 1,
        TextColor3 = Library.Theme.TextColor, -- Màu trắng
        TextSize = 14,
        TextXAlignment = Enum.TextXAlignment.Left,
    },
    Button = {
        BackgroundColor3 = Library.Theme.TextColor, -- Màu trắng
        TextColor3 = Library.Theme.DarkContrast, -- Màu đen
        BorderSizePixel = 0,
        TextSize = 14,
        TextXAlignment = Enum.TextXAlignment.Center,
    },
    Box = {
        BackgroundColor3 = Library.Theme.TextColor, -- Màu trắng
        TextColor3 = Library.Theme.DarkContrast, -- Màu đen
        BorderSizePixel = 0,
        TextSize = 14,
        TextXAlignment = Enum.TextXAlignment.Left,
    },
    ScrollingFrame = {
        BackgroundColor3 = Library.Theme.LightContrast, -- Màu đen
        ScrollBarThickness = 4,
        CanvasSize = UDim2.fromScale(0, 0),
        Size = UDim2.fromScale(1, 1),
    },
}

-- Hàm tạo đối tượng mới
local Objects = {}

function Objects.new(Type)
    local NewObject = Instance.new(ActualTypes[Type])
    if Properties[Type] then
        for Property, Value in next, Properties[Type] do
            NewObject[Property] = Value
        end
    end
    return NewObject
end

-- Tạo khung UI chính cho điện thoại
local function CreateMainUI()
    local ScreenGui = Instance.new("ScreenGui")
    ScreenGui.Name = "MainUI"
    ScreenGui.Parent = game:GetService("CoreGui")

    local MainFrame = Objects.new("Frame")
    MainFrame.Size = UDim2.new(0.8, 0, 0.8, 0) -- Kích cỡ vừa phải cho điện thoại
    MainFrame.Position = UDim2.new(0.1, 0, 0.1, 0) -- Căn giữa màn hình
    MainFrame.Parent = ScreenGui

    local TitleLabel = Objects.new("Label")
    TitleLabel.Text = "Giao Diện Chính"
    TitleLabel.Size = UDim2.new(1, 0, 0.1, 0)
    TitleLabel.Parent = MainFrame

    local Button = Objects.new("Button")
    Button.Text = "Nhấn vào tôi"
    Button.Size = UDim2.new(0.6, 0, 0.1, 0)
    Button.Position = UDim2.new(0.2, 0, 0.3, 0)
    Button.Parent = MainFrame

    local ScrollingFrame = Objects.new("ScrollingFrame")
    ScrollingFrame.Size = UDim2.new(0.8, 0, 0.5, 0)
    ScrollingFrame.Position = UDim2.new(0.1, 0, 0.5, 0)
    ScrollingFrame.Parent = MainFrame

    -- Thêm phần tử vào ScrollingFrame
    for i = 1, 10 do
        local ItemLabel = Objects.new("Label")
        ItemLabel.Text = "Phần tử " .. i
        ItemLabel.Size = UDim2.new(1, 0, 0.1, 0)
        ItemLabel.Parent = ScrollingFrame
    end

    -- Tạo Tween mở rộng khung chính
    TweenService:Create(MainFrame, TweenInfo.new(0.3, Enum.EasingStyle.Quad, Enum.EasingDirection.Out), {Size = UDim2.new(0.8, 0, 0.8, 0)}):Play()
end

-- Hàm chính của Library
function Library:Window()
    local Toggle = false
    local ui = Instance.new("ScreenGui")
    ui.Parent = game.CoreGui
    ui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling

    local MainFrame = Instance.new("Frame")
    MainFrame.Parent = ui
    MainFrame.BackgroundColor3 = Library.Theme.DarkContrast -- Màu đen
    MainFrame.BorderSizePixel = 0
    MainFrame.Position = UDim2.new(0.5, -248, 0.5, -248)
    MainFrame.Size = UDim2.new(0, 496, 0, 496)
    MainFrame.Active = true
    MainFrame.Draggable = true

    -- Tạo nút mở UI điện thoại
    local OpenButton = Instance.new("TextButton")
    OpenButton.Parent = MainFrame
    OpenButton.BackgroundColor3 = Library.Theme.Accent
    OpenButton.BorderSizePixel = 0
    OpenButton.Position = UDim2.new(0, 10, 0, 10)
    OpenButton.Size = UDim2.new(0, 100, 0, 30)
    OpenButton.Font = Enum.Font.SourceSans
    OpenButton.Text = "Mở UI Điện Thoại"
    OpenButton.TextColor3 = Library.Theme.DarkContrast
    OpenButton.TextSize = 18
    OpenButton.MouseButton1Click:Connect(function()
        CreateMainUI()
    end)

    -- Thêm các tab vào menu
    local Win = Library:Window("Zaque Hub Pc")

    local tab1 = Win:CraftTab("Main Farm")
    local tab1_5 = Win:CraftTab("Misc Farm")
    local tab2 = Win:CraftTab("Stats")
    local tab2_5 = Win:CraftTab("Combat")
    local tab3 = Win:CraftTab("Teleport")
    local tab3_5 = Win:CraftTab("Dungeon")
    local tab5_5 = Win:CraftTab("Races V4")
    local tab4 = Win:CraftTab("Shop")
    local tab4_5 = Win:CraftTab("Devil Fruit")
    local tab5 = Win:CraftTab("Misc")

    -- Còn lại phần code cũ từ file của bạn
    local TabFrame = Instance.new("Frame")
    TabFrame.Name = "TabFrame"
    TabFrame.Parent = MainFrame
    TabFrame.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
    TabFrame.BackgroundTransparency = 1.000
    TabFrame.Position = UDim2.new(0.0299999993, 0, 0.0700000003, 0)
    TabFrame.Size = UDim2.new(0, 118, 0, 457)

    local Main = Instance.new("Frame")
    Main.Name = "Main"
    Main.Parent = MainFrame
    Main.BackgroundColor3 = Color3.fromRGB(35, 35, 35)
    Main.BorderSizePixel = 0
    Main.Position = UDim2.new(0.287499994, 0, 0.0700000003, 0)
    Main.Size = UDim2.new(0, 344, 0, 457)

    -- Bạn có thể thêm các thành phần UI khác vào đây

    -- Thêm cửa sổ cuối file
    local WindowFrame = Instance.new("Frame")
    WindowFrame.Parent = MainFrame
    WindowFrame.BackgroundColor3 = Library.Theme.LightContrast -- Màu đen
    WindowFrame.BorderSizePixel = 0
    WindowFrame.Position = UDim2.new(0.1, 0, 0.9, 0) -- Vị trí phù hợp cuối giao diện
    WindowFrame.Size = UDim2.new(0.8, 0, 0.1, 0)

    local WindowLabel = Instance.new("TextLabel")
    WindowLabel
