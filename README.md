local Config = {
    WindowName = " HUB -- [  ]",
    Color = Color3.fromRGB(0,183,255),
    Keybind = Enum.KeyCode.RightControl
}

local Library = loadstring(game:HttpGet("https://raw.githubusercontent.com/kickTh/New-Ui/main/BracketV3.lua.txt"))()
local Window = Library:CreateWindow(Config, game:GetService("CoreGui"))

local Tab1 = Window:CreateTab("Main")
local Section1 = Tab1:CreateSection("Main")
local Label1 = Section1:CreateLabel("Credit : nill")
local Label1 = Section1:CreateLabel("Discord : https://discord.gg")
local Button1 = Section1:CreateButton("Copy Discord", function()
setclipboard("https://discord.gg")
end)
local Tab1 = Window:CreateTab("Example")
local Tab2 = Window:CreateTab("UI Settings")

local Section1 = Tab1:CreateSection("First Section")
local Section2 = Tab1:CreateSection("Second Section")
local Section3 = Tab2:CreateSection("Menu")
local Section4 = Tab2:CreateSection("Background")

local Label1 = Section1:CreateLabel("Label 1")
Label1:UpdateText("lol")
-------------
local Button1 = Section1:CreateButton("Button 1", function()
    print("Click Button 1")
end)
Button1:AddToolTip("Button 1 ToolTip")
-------------
local Toggle1 = Section1:CreateToggle("Toggle 1", nil, function(State)
    print(State)
end)
Toggle1:AddToolTip("Toggle 1 ToolTip")
Toggle1:CreateKeybind("Y", function(Key)
    print(Key)
end)

local TextBox1 = Section1:CreateTextBox("TextBox 1", "Only numbers", true, function(Value)
    print(Value)
end) 
TextBox1:AddToolTip("Yes only numbers")
TextBox1:SetValue("new value here")
Section1:CreateTextBox("TextBox 1\nMultiline", "numbers and letters", false, function(String)
    print(String)
end)
-------------
local Slider1 = Section1:CreateSlider("Slider 1", 0,100,nil,true, function(Value)
    print(Value)
end)
Slider1:AddToolTip("Slider 1 ToolTip")
Slider1:SetValue(50)
-------------
local Dropdown1 = Section1:CreateDropdown("Dropdown 1", {"Option 1","Option 2","Option 3"}, function(String)
    print(String)
end)
Dropdown1:AddToolTip("Dropdown 1 ToolTip")
Dropdown1:SetOption("Option 1")
-------------
local Colorpicker1 = Section1:CreateColorpicker("Colorpicker 1", function(Color)
    print(Color)
end)
Colorpicker1:AddToolTip("Colorpicker 1 ToolTip")
Colorpicker1:UpdateColor(Color3.fromRGB(255,0,0))
-------------
Section2:CreateLabel("Label 2\nMultiline")
-------------
local Button2 = Section2:CreateButton("Button 2\nMultiline", function()
    print("Click Button 2")
end)
Button2:AddToolTip("Button 2 ToolTip\nMultiline")
-------------
local Toggle2 = Section2:CreateToggle("Toggle 2\nMultiline", nil, function(State)
    print(State)
end)
Toggle2:AddToolTip("Toggle 2 ToolTip\nMultiline")
Toggle2:CreateKeybind("U", function(Key)
    print(Key)
end)
-------------
local Slider2 = Section2:CreateSlider("Slider 2\nMultiline", 0,100,nil,false, function(Value)
    print(Value)
end)
Slider2:AddToolTip("Slider 2 ToolTip\nMultiline")
Slider2:SetValue(25)
-------------
local Dropdown2 = Section2:CreateDropdown("Dropdown 2\nMultiline", {"Option 4","Option 5","Option 6"}, function(String)
    print(String)
end)
Dropdown2:AddToolTip("Dropdown 2 ToolTip")
Dropdown2:SetOption("Option 6")
-------------
local Colorpicker2 = Section2:CreateColorpicker("Colorpicker 2\nMultiline", function(Color)
    print(Color)
end)
Colorpicker2:AddToolTip("Colorpicker 2 ToolTip")
Colorpicker2:UpdateColor(Color3.fromRGB(0,0,255))
-------------
local Toggle3 = Section3:CreateToggle("UI Toggle", nil, function(State)
    Window:Toggle(State)
end)
Toggle3:CreateKeybind(tostring(Config.Keybind):gsub("Enum.KeyCode.", ""), function(Key)
    Config.Keybind = Enum.KeyCode[Key]
end)
Toggle3:SetState(true)
