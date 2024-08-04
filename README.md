local httpService = game:GetService('HttpService')
local ThemeManager = {} do
    ThemeManager.Folder = 'LinoriaLibSettings'
    -- if not isfolder(ThemeManager.Folder) then makefolder(ThemeManager.Folder) end

    ThemeManager.Library = nil
    ThemeManager.BuiltInThemes = {
        ['Default']       = { 1, httpService:JSONDecode('{"FontColor":"ffffff","MainColor":"2d2d2d","AccentColor":"ff7043","BackgroundColor":"212121","OutlineColor":"37474f"}') },
        ['BBot']          = { 2, httpService:JSONDecode('{"FontColor":"ffffff","MainColor":"3e3e3e","AccentColor":"00796b","BackgroundColor":"263238","OutlineColor":"1c1c1c"}') },
        ['Fatality']      = { 3, httpService:JSONDecode('{"FontColor":"ffffff","MainColor":"1e3d59","AccentColor":"ffab40","BackgroundColor":"102027","OutlineColor":"4f5b62"}') },
        ['Jester']        = { 4, httpService:JSONDecode('{"FontColor":"ffffff","MainColor":"37474f","AccentColor":"ffd54f","BackgroundColor":"263238","OutlineColor":"546e7a"}') },
        ['Mint']          = { 5, httpService:JSONDecode('{"FontColor":"ffffff","MainColor":"4e342e","AccentColor":"8bc34a","BackgroundColor":"3e2723","OutlineColor":"5d4037"}') },
        ['Tokyo Night']   = { 6, httpService:JSONDecode('{"FontColor":"ffffff","MainColor":"1a237e","AccentColor":"d500f9","BackgroundColor":"0d47a1","OutlineColor":"311b92"}') },
        ['Ubuntu']        = { 7, httpService:JSONDecode('{"FontColor":"ffffff","MainColor":"424242","AccentColor":"ff6f00","BackgroundColor":"212121","OutlineColor":"616161"}') },
        ['Quartz']        = { 8, httpService:JSONDecode('{"FontColor":"ffffff","MainColor":"37474f","AccentColor":"26a69a","BackgroundColor":"263238","OutlineColor":"455a64"}') },
    }

    function ThemeManager:ApplyTheme(theme)
        local customThemeData = self:GetCustomTheme(theme)
        local data = customThemeData or self.BuiltInThemes[theme]

        if not data then return end

        local scheme = data[2]
        for idx, col in next, customThemeData or scheme do
            self.Library[idx] = Color3.fromHex(col)
            
            if Options[idx] then
                Options[idx]:SetValueRGB(Color3.fromHex(col))
            end
        end

        self:ThemeUpdate()
    end

    function ThemeManager:ThemeUpdate()
        local options = { "FontColor", "MainColor", "AccentColor", "BackgroundColor", "OutlineColor" }
        for i, field in next, options do
            if Options and Options[field] then
                self.Library[field] = Options[field].Value
            end
        end

        self.Library.AccentColorDark = self.Library:GetDarkerColor(self.Library.AccentColor);
        self.Library:UpdateColorsUsingRegistry()
    end

    function ThemeManager:LoadDefault()        
        local theme = 'Default'
        local content = isfile(self.Folder .. '/themes/default.txt') and readfile(self.Folder .. '/themes/default.txt')

        local isDefault = true
        if content then
            if self.BuiltInThemes[content] then
                theme = content
            elseif self:GetCustomTheme(content) then
                theme = content
                isDefault = false;
            end
        elseif self.BuiltInThemes[self.DefaultTheme] then
            theme = self.DefaultTheme
        end

        if isDefault then
            Options.ThemeManager_ThemeList:SetValue(theme)
        else
            self:ApplyTheme(theme)
        end
    end

    function ThemeManager:SaveDefault(theme)
        writefile(self.Folder .. '/themes/default.txt', theme)
    end

    function ThemeManager:CreateThemeManager(groupbox)
        groupbox:AddLabel('Background color'):AddColorPicker('BackgroundColor', { Default = self.Library.BackgroundColor });
        groupbox:AddLabel('Main color'):AddColorPicker('MainColor', { Default = self.Library.MainColor });
        groupbox:AddLabel('Accent color'):AddColorPicker('AccentColor', { Default = self.Library.AccentColor });
        groupbox:AddLabel('Outline color'):AddColorPicker('OutlineColor', { Default = self.Library.OutlineColor });
        groupbox:AddLabel('Font color'):AddColorPicker('FontColor', { Default = self.Library.FontColor });

        local ThemesArray = {}
        for Name, Theme in next, self.BuiltInThemes do
            table.insert(ThemesArray, Name)
        end

        table.sort(ThemesArray, function(a, b) return self.BuiltInThemes[a][1] < self.BuiltInThemes[b][1] end)

        groupbox:AddDivider()
        groupbox:AddDropdown('ThemeManager_ThemeList', { Text = 'Theme list', Values = ThemesArray, Default = 1 })

        groupbox:AddButton('Set as default', function()
            self:SaveDefault(Options.ThemeManager_ThemeList.Value)
            self.Library:Notify(string.format('Set default theme to %q', Options.ThemeManager_ThemeList.Value))
        end)

        Options.ThemeManager_ThemeList:OnChanged(function()
            self:ApplyTheme(Options.ThemeManager_ThemeList.Value)
        end)

        groupbox:AddDivider()
        groupbox:AddInput('ThemeManager_CustomThemeName', { Text = 'Custom theme name' })
        groupbox:AddDropdown('ThemeManager_CustomThemeList', { Text = 'Custom themes', Values = self:ReloadCustomThemes(), AllowNull = true, Default = 1 })
        groupbox:AddDivider()
        
        groupbox:AddButton('Save theme', function() 
            self:SaveCustomTheme(Options.ThemeManager_CustomThemeName.Value)

            Options.ThemeManager_CustomThemeList:SetValues(self:ReloadCustomThemes())
            Options.ThemeManager_CustomThemeList:SetValue(nil)
        end):AddButton('Load theme', function() 
            self:ApplyTheme(Options.ThemeManager_CustomThemeList.Value) 
        end)

        groupbox:AddButton('Refresh list', function()
            Options.ThemeManager_CustomThemeList:SetValues(self:ReloadCustomThemes())
            Options.ThemeManager_CustomThemeList:SetValue(nil)
        end)

        groupbox:AddButton('Set as default', function()
            if Options.ThemeManager_CustomThemeList.Value ~= nil and Options.ThemeManager_CustomThemeList.Value ~= '' then
                self:SaveDefault(Options.ThemeManager_CustomThemeList.Value)
                self.Library:Notify(string.format('Set default theme to %q', Options.ThemeManager_CustomThemeList.Value))
            end
        end)

        ThemeManager:LoadDefault()

        local function UpdateTheme()
            self:ThemeUpdate()
        end

        Options.BackgroundColor:OnChanged(UpdateTheme)
        Options.MainColor:OnChanged(UpdateTheme)
        Options.AccentColor:OnChanged(UpdateTheme)
        Options.OutlineColor:OnChanged(UpdateTheme)
        Options.FontColor:OnChanged(UpdateTheme)
    end

    function ThemeManager:GetCustomTheme(file)
        local path = self.Folder .. '/themes/' .. file
        if not isfile(path) then
            return nil
        end

        local data = readfile(path)
        local success, decoded = pcall(httpService.JSONDecode, httpService, data)
        
        if not success then
            return nil
        end

        return decoded
    end

    function ThemeManager:SaveCustomTheme(file)
        if file:gsub(' ', '') == '' then
            return self.Library:Notify('Invalid file name for theme (empty)', 3)
        end

        local theme = {}
        local fields = { "FontColor", "MainColor", "AccentColor", "BackgroundColor", "OutlineColor" }

        for _, field in next, fields do
            theme[field] = Options[field].Value:ToHex()
        end

        writefile(self.Folder .. '/themes/' .. file .. '.json', httpService:JSONEncode(theme))
    end

    function ThemeManager:ReloadCustomThemes()
        local list = listfiles(self.Folder .. '/themes')

        local out = {}
        for i = 1, #list do
            local file = list[i]
            if file:sub(-5) == '.json' then
                local pos = file:find('.json', 1, true)
                local name = file:sub(1, pos - 1)
                name = name:sub(#self.Folder + 9)

                table.insert(out, name)
            end
        end

        return out
    end
end

function SaveManager:BuildConfigSection(Tabs)
    local ConfigSection = Tabs.Main:AddRightGroupbox('Configuration')

    ConfigSection:AddInput('SaveManager_ConfigName', { Text = 'Config name' })

    ConfigSection:AddDropdown('SaveManager_ConfigList', { Text = 'Config list', Values = SaveManager:RefreshConfigList(), AllowNull = true })

    ConfigSection:AddDivider()

    ConfigSection:AddButton('Create config', function()
        SaveManager:SaveConfig(Options.SaveManager_ConfigName.Value)

        Options.SaveManager_ConfigList:SetValues(SaveManager:RefreshConfigList())
        Options.SaveManager_ConfigList:SetValue(nil)
    end):AddButton('Load config', function()
        SaveManager:LoadConfig(Options.SaveManager_ConfigList.Value)
    end)

    ConfigSection:AddButton('Overwrite config', function()
        SaveManager:SaveConfig(Options.SaveManager_ConfigList.Value)
    end)

    ConfigSection:AddButton('Autoload config', function()
        SaveManager:SetIgnoreIndexes({ 'SaveManager_ConfigList' })
        SaveManager:BuildConfigSection(Tabs)
    end)

    Options.SaveManager_ConfigList:SetValues(SaveManager:RefreshConfigList())
end

local Main = Window:CreateTab(6031154871, "Main")
local Misc = Main:CreateTab(6031154871, "Misc Farm")
local Stats = Main:CreateTab(6031154871, "Stats")
local PvP = Main:CreateTab(6031154871, "PvP")
local Teleport = Main:CreateTab(6031154871, "Teleport")
local Dungeon = Main:CreateTab(6031154871, "Dungeon")
local Market = Main:CreateTab(6031154871, "Market")
local DevilFruit = Main:CreateTab(6031154871, "Devil Fruit")

local Main1 = Main:CreatePage("Main 1")
local Main2 = Main:CreatePage("Main 2")
local Main3 = Main:CreatePage("Main 3")
local Main4 = Main:CreatePage("Main 4")
local Main5 = Main:CreatePage("Main 5")
local Main6 = Main:CreatePage("Main 6")
local Main7 = Main:CreatePage("Main 7")
local MiscFarm1 = Misc:CreatePage("Misc Farm 1")
local Main8 = Stats:CreatePage("Main 8")
local Main9 = Stats:CreatePage("Main 9")
local Main10 = PvP:CreatePage("Main 10")
local Main11 = Teleport:CreatePage("Main 11")
local Main12 = Dungeon:CreatePage("Main 12")
local Main13 = Dungeon:CreatePage("Main 13")
local Main14 = Market:CreatePage("Main 14")
local Main15 = Market:CreatePage("Main 15")
local Main16 = DevilFruit:CreatePage("Main 16")
local Main17 = Misc:CreatePage("Main 17")
local Main18 = Misc:CreatePage("Main 18")
local Main19 = Misc:CreatePage("Main 19")
local Main20 = Misc:CreatePage("Main 20")
