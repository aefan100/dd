local OrionLib = loadstring(game:HttpGet(('https://raw.githubusercontent.com/shlexware/Orion/main/source')))()
local Window = OrionLib:MakeWindow({Name = "Title of the library", HidePremium = false, SaveConfig = true, ConfigFolder = "OrionTest"})
local Tab = Window:MakeTab({
    Name = "Tab 1",
    Icon = "rbxassetid://4483345998",
    PremiumOnly = false
})
local Section = Tab:AddSection({ Name = "Section" })
TablePlayers = {}
for i,v in pairs(game.Players:GetChildren()) do table.insert(TablePlayers,v.Name) end
function SendMoney(Players,Money)
    local args = { [1] = "Transfer", [2] = Money, [3] = Players }
    game:GetService("ReplicatedStorage").BankFolder.RemoteFunction:InvokeServer(unpack(args))
    OrionLib:MakeNotification({ Name = "Send Money!", Content = "Amount : " .. tostring(Money), Image = "rbxassetid://4483345998", Time = 5 })
end
SelectPlayers = nil
RefreshPlayersDropdown = Tab:AddDropdown({
    Name = "Select Players",
    Default = "",
    Options = TablePlayers,
    Callback = function(Value)
        SelectPlayers = Value
    end    
})
Tab:AddButton({
    Name = "Refresh Players",
    Callback = function()
        local TablePlayers = {}
        for i,v in pairs(game.Players:GetChildren()) do
            table.insert(TablePlayers,v.Name)
        end
        RefreshPlayersDropdown:Refresh(TablePlayers,true)
      end    
})
AmountMoney = 0
Tab:AddSlider({ 
    Name = "Slider", 
    Min = 100, 
    Max = 1000000, 
    Default = 100, 
    Color = Color3.fromRGB(255,255,255), 
    Increment = 1, 
    ValueName = "bananas", 
    Callback = function(Value)
        AmountMoney = Value 
    end 
})
Tab:AddButton({ 
    Name = "Send Money", 
    Callback = function() 
        if SelectPlayers ~= nil then  
            SendMoney(SelectPlayers,tonumber(AmountMoney)) 
        end 
    end 
})
Tab:AddToggle({ 
    Name = "Auto Send Money", 
    Default = false, 
    Callback = function(Value) 
        AutoSendMoney = Value 
    end 
})
spawn(function()
    while wait() do
        if AutoSendMoney and SelectPlayers ~= nil then
            SendMoney(SelectPlayers,tonumber(AmountMoney))
            wait(1)
        end
    end
end)
