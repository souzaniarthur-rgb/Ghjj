--[[========================================
    SouzaRub - Blox Fruits
    Autor: Souza
    Versão: 1.0.0
==========================================]]

-- Proteção básica
pcall(function()
    if game.CoreGui:FindFirstChild("SouzaRubUI") then
        game.CoreGui.SouzaRubUI:Destroy()
    end
end)

-- Serviços
local TweenService = game:GetService("TweenService")
local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer

-- GUI
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "SouzaRubUI"
ScreenGui.Parent = game.CoreGui
ScreenGui.ResetOnSpawn = false

-- MAIN FRAME
local Main = Instance.new("Frame")
Main.Size = UDim2.new(0, 620, 0, 380)
Main.Position = UDim2.new(0.5, -310, 0.5, -190)
Main.BackgroundColor3 = Color3.fromRGB(15,15,15)
Main.BorderSizePixel = 0
Main.Parent = ScreenGui
Main.Visible = true

-- Corner
Instance.new("UICorner", Main).CornerRadius = UDim.new(0, 10)

-- Título
local Title = Instance.new("TextLabel", Main)
Title.Size = UDim2.new(1, -40, 0, 40)
Title.Position = UDim2.new(0, 20, 0, 0)
Title.Text = "SouzaRub  |  Blox Fruits"
Title.TextColor3 = Color3.fromRGB(220, 20, 60)
Title.Font = Enum.Font.GothamBold
Title.TextSize = 20
Title.BackgroundTransparency = 1
Title.TextXAlignment = Left

-- Botão X
local CloseBtn = Instance.new("TextButton", Main)
CloseBtn.Size = UDim2.new(0, 30, 0, 30)
CloseBtn.Position = UDim2.new(1, -35, 0, 5)
CloseBtn.Text = "X"
CloseBtn.Font = Enum.Font.GothamBold
CloseBtn.TextSize = 16
CloseBtn.TextColor3 = Color3.fromRGB(255,255,255)
CloseBtn.BackgroundColor3 = Color3.fromRGB(180, 0, 0)
CloseBtn.BorderSizePixel = 0
Instance.new("UICorner", CloseBtn).CornerRadius = UDim.new(0, 6)

-- CONTAINER
local Container = Instance.new("Frame", Main)
Container.Size = UDim2.new(1, -20, 1, -60)
Container.Position = UDim2.new(0, 10, 0, 50)
Container.BackgroundTransparency = 1

-- Abas (exemplo)
local Tabs = {"Farm","Player","Combat","Fruit","Teleport","ESP","Raid","Settings"}
local CurrentTab

local Pages = {}

local function CreatePage(name)
    local Page = Instance.new("Frame", Container)
    Page.Size = UDim2.new(1,0,1,0)
    Page.Visible = false
    Page.BackgroundTransparency = 1
    Pages[name] = Page
end

for _,tab in ipairs(Tabs) do
    CreatePage(tab)
end

-- Mostrar aba
local function OpenTab(name)
    for _,p in pairs(Pages) do
        p.Visible = false
    end
    Pages[name].Visible = true
    CurrentTab = name
end

OpenTab("Farm")

-- Exemplo de opção
local function CreateToggle(parent, text, y)
    local Btn = Instance.new("TextButton", parent)
    Btn.Size = UDim2.new(0, 250, 0, 35)
    Btn.Position = UDim2.new(0, 20, 0, y)
    Btn.Text = "[ OFF ]  "..text
    Btn.Font = Enum.Font.Gotham
    Btn.TextSize = 14
    Btn.TextColor3 = Color3.fromRGB(255,255,255)
    Btn.BackgroundColor3 = Color3.fromRGB(30,30,30)
    Btn.BorderSizePixel = 0
    Instance.new("UICorner", Btn).CornerRadius = UDim.new(0,6)

    local state = false
    Btn.MouseButton1Click:Connect(function()
        state = not state
        Btn.Text = (state and "[ ON ]  " or "[ OFF ]  ")..text
    end)
end

-- FARM OPTIONS
CreateToggle(Pages.Farm,"Auto Farm Level",10)
CreateToggle(Pages.Farm,"Auto Farm Boss",55)
CreateToggle(Pages.Farm,"Auto Farm Mastery",100)
CreateToggle(Pages.Farm,"Auto Farm Elite",145)

-- PLAYER OPTIONS
CreateToggle(Pages.Player,"Infinite Jump",10)
CreateToggle(Pages.Player,"Fly",55)
CreateToggle(Pages.Player,"God Mode",100)

-- COMBAT
CreateToggle(Pages.Combat,"Fast Attack",10)
CreateToggle(Pages.Combat,"Kill Aura",55)

-- POP-UP
local Popup = Instance.new("ImageButton")
Popup.Size = UDim2.new(0, 80, 0, 80)
Popup.Position = UDim2.new(0, 15, 0.5, -40)
Popup.BackgroundColor3 = Color3.fromRGB(15,15,15)
Popup.BorderSizePixel = 0
Popup.Visible = false
Popup.Parent = ScreenGui
Popup.AutoButtonColor = true
Popup.Image = "" -- pode colocar asset id depois

Instance.new("UICorner", Popup).CornerRadius = UDim.new(0, 12)

local PopupText = Instance.new("TextLabel", Popup)
PopupText.Size = UDim2.new(1,0,1,0)
PopupText.Text = "Souza\nScript"
PopupText.Font = Enum.Font.GothamBold
PopupText.TextSize = 14
PopupText.TextColor3 = Color3.fromRGB(220,20,60)
PopupText.BackgroundTransparency = 1
PopupText.TextWrapped = true

-- Animações
local function Tween(obj, props)
    TweenService:Create(obj, TweenInfo.new(0.25, Enum.EasingStyle.Quad), props):Play()
end

-- FECHAR → POPUP
CloseBtn.MouseButton1Click:Connect(function()
    Tween(Main,{Size = UDim2.new(0,0,0,0)})
    task.wait(0.25)
    Main.Visible = false
    Popup.Visible = true
    Popup.Size = UDim2.new(0,0,0,0)
    Tween(Popup,{Size = UDim2.new(0,80,0,80)})
end)

-- ABRIR PELO POPUP
Popup.MouseButton1Click:Connect(function()
    Tween(Popup,{Size = UDim2.new(0,0,0,0)})
    task.wait(0.2)
    Popup.Visible = false
    Main.Visible = true
    Main.Size = UDim2.new(0,0,0,0)
    Tween(Main,{Size = UDim2.new(0,620,0,380)})
end) 
