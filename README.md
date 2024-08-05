-- Tạo ScreenGui và Toggle UI Button
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "ShinyHubUI"
screenGui.Parent = game.Players.LocalPlayer:WaitForChild("PlayerGui")

local toggleButton = Instance.new("TextButton")
toggleButton.Size = UDim2.new(0, 50, 0, 50)
toggleButton.Position = UDim2.new(0, 10, 0, 10)
toggleButton.BackgroundColor3 = Color3.fromRGB(255, 105, 180)
toggleButton.Text = ""
toggleButton.TextColor3 = Color3.fromRGB(255, 255, 255)
toggleButton.TextScaled = true
toggleButton.Parent = screenGui
toggleButton.BorderSizePixel = 0
toggleButton.AutoButtonColor = false

local toggleIcon = Instance.new("ImageLabel")
toggleIcon.Size = UDim2.new(0.8, 0, 0.8, 0)
toggleIcon.Position = UDim2.new(0.1, 0, 0.1, 0)
toggleIcon.BackgroundTransparency = 1
toggleIcon.Image = "rbxassetid://3944680095"  -- ID của hình tròn
toggleIcon.Parent = toggleButton

-- Tạo Main Frame và ẩn nó ban đầu
local mainFrame = Instance.new("Frame")
mainFrame.Size = UDim2.new(0, 500, 0, 350)  -- Tăng chiều rộng của UI
mainFrame.Position = UDim2.new(0.5, -250, 0.5, -175)
mainFrame.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
mainFrame.BorderColor3 = Color3.fromRGB(255, 105, 180)  -- Màu hồng ở rìa UI
mainFrame.BorderSizePixel = 2
mainFrame.Visible = false  -- Ẩn ban đầu
mainFrame.Active = true
mainFrame.Draggable = true  -- Khả năng di chuyển UI
mainFrame.Parent = screenGui

-- Tạo Label Shiny Hub
local hubLabel = Instance.new("TextLabel")
hubLabel.Size = UDim2.new(0, 100, 0, 30)
hubLabel.Position = UDim2.new(0, 10, 0, 10)
hubLabel.BackgroundTransparency = 1
hubLabel.Text = "Shiny Hub"
hubLabel.TextColor3 = Color3.fromRGB(255, 105, 180)
hubLabel.TextScaled = true
hubLabel.Parent = mainFrame

-- Thay thế nút đóng UI bằng ImageButton với hình ảnh bình exp của Minecraft
local closeButton = Instance.new("ImageButton")
closeButton.Size = UDim2.new(0, 30, 0, 30)
closeButton.Position = UDim2.new(1, -40, 0, 10)
closeButton.BackgroundTransparency = 1
closeButton.Image = "rbxassetid://(minecraft_exp_bottle_asset_id)" -- Thay thế bằng asset ID của bình exp Minecraft
closeButton.Parent = mainFrame

-- Tạo Sidebar cho menu bên trái
local sidebar = Instance.new("ScrollingFrame")  -- Thay vì Frame, dùng ScrollingFrame để cuộn
sidebar.Size = UDim2.new(0, 150, 1, -50)
sidebar.Position = UDim2.new(0, 0, 0, 50)  -- Di chuyển tab xuống
sidebar.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
sidebar.BorderSizePixel = 0
sidebar.ScrollBarThickness = 6  -- Độ dày thanh cuộn
sidebar.CanvasSize = UDim2.new(0, 0, 0, 320)  -- Đặt kích thước canvas để hỗ trợ cuộn
sidebar.Parent = mainFrame

-- Tạo các nút cho menu bên trái
local menuItems = {"Owner Info", "Shop", "Status & Server", "Settings Farm", "Main Farm", "Get Item", "Fruits & Raid", "Upgraded Race"}
local buttons = {}

for i, menuItem in ipairs(menuItems) do
    local button = Instance.new("TextButton")
    button.Size = UDim2.new(1, 0, 0, 40)  -- Chiều cao nút nhỏ hơn
    button.Position = UDim2.new(0, 0, 0, (i-1) * 40)
    button.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
    button.Text = menuItem
    button.TextColor3 = Color3.fromRGB(255, 255, 255)
    button.TextScaled = true
    button.Parent = sidebar
    table.insert(buttons, button)
end

-- Tạo Content Frame để hiển thị nội dung của mỗi tab
local contentFrame = Instance.new("Frame")
contentFrame.Size = UDim2.new(1, -150, 1, -50)
contentFrame.Position = UDim2.new(0, 150, 0, 50)
contentFrame.BackgroundColor3 = Color3.fromRGB(35, 35, 35)
contentFrame.Parent = mainFrame

-- Nội dung cho Tab 1 (Owner Info) với các Toggle
local toggleHolder = Instance.new("Frame")
toggleHolder.Size = UDim2.new(1, 0, 1, 0)
toggleHolder.Position = UDim2.new(0, 0, 0, 0)
toggleHolder.BackgroundColor3 = Color3.fromRGB(35, 35, 35)
toggleHolder.Parent = contentFrame

local toggleItems = {"Auto Farm", "Auto Collect", "Auto Attack"}
local toggles = {}
local toggleStates = {}

for i, toggleItem in ipairs(toggleItems) do
    local toggleContainer = Instance.new("Frame")
    toggleContainer.Size = UDim2.new(1, -20, 0, 30)  -- Toggle nhỏ hơn
    toggleContainer.Position = UDim2.new(0, 10, 0, (i-1) * 40)
    toggleContainer.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
    toggleContainer.BorderSizePixel = 0
    toggleContainer.Parent = toggleHolder
    
    local toggleLabel = Instance.new("TextLabel")
    toggleLabel.Size = UDim2.new(0.7, 0, 1, 0)
    toggleLabel.Position = UDim2.new(0, 10, 0, 0)
    toggleLabel.BackgroundTransparency = 1
    toggleLabel.Text = toggleItem
    toggleLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
    toggleLabel.TextScaled = true
    toggleLabel.Parent = toggleContainer
    
    local toggleSwitch = Instance.new("Frame")
    toggleSwitch.Size = UDim2.new(0.15, 0, 0.6, 0)  -- Toggle nhỏ hơn
    toggleSwitch.Position = UDim2.new(0.8, 0, 0.2, 0)
    toggleSwitch.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
    toggleSwitch.BorderSizePixel = 0
    toggleSwitch.Parent = toggleContainer
    
    local toggleKnob = Instance.new("Frame")
    toggleKnob.Size = UDim2.new(0.5, 0, 1, 0)
    toggleKnob.Position = UDim2.new(0, 0, 0, 0)
    toggleKnob.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
    toggleKnob.BorderSizePixel = 0
    toggleKnob.AnchorPoint = Vector2.new(0.5, 0.5)
    toggleKnob.SizeConstraint = Enum.SizeConstraint.RelativeYY
    toggleKnob.Size = UDim2.new(0, toggleSwitch.Size.Y.Offset, 0, toggleSwitch.Size.Y.Offset)
    toggleKnob.Parent = toggleSwitch
    
    toggleStates[i] = false  -- Mặc định trạng thái của Toggle là OFF
    
    toggleContainer.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 then
            toggleStates[i] = not toggleStates[i]
            if toggleStates[i] then
                toggleKnob:TweenPosition(UDim2.new(1, -toggleKnob.Size.X.Offset, 0, 0), "Out", "Sine", 0.2, true)
                toggleSwitch.BackgroundColor3 = Color3.fromRGB(0, 255, 0)  -- Màu xanh lá khi bật
                
                -- Thêm chức năng Auto Jump cho Auto Farm
                if toggleItem == "Auto Farm" then
                    local player = game.Players.LocalPlayer
                    player.Character.Humanoid.JumpPower = 100
                    while toggleStates[i] do
                        player.Character.Humanoid:ChangeState(Enum.HumanoidStateType.Jumping)
                        wait(1)
                    end
                end
            else
                toggleKnob:TweenPosition(UDim2.new(0, 0, 0, 0), "Out", "Sine", 0.2, true)
                toggleSwitch.BackgroundColor3 = Color3.fromRGB(0, 0, 0)  -- Màu đen khi tắt
            end
        end
    end)
end

-- Chức năng chuyển đổi nội dung các tab
local function showContent(index)
    for i, button in ipairs(buttons) do
        if i == index then
            button.BackgroundColor3 = Color3.fromRGB(255, 105, 180)  -- Đổi màu nút được chọn
        else
            button.BackgroundColor3 = Color3.fromRGB(0, 0, 0)  -- Màu mặc định cho các nút không được chọn
        end
    end
end

-- Kết nối các nút tab với hàm showContent
for i, button in ipairs(buttons) do
    button.MouseButton1Click:Connect(function()
        showContent(i)
    end)
end

-- Chức năng bật/tắt UI
toggleButton.MouseButton1Click:Connect(function()
    mainFrame.Visible = not mainFrame.Visible
end)

-- Chức năng đóng UI
closeButton.MouseButton1Click:Connect(function()
    mainFrame.Visible = false
end)

-- Mặc định hiển thị nội dung của tab 1 (Owner Info)
showContent(1)
