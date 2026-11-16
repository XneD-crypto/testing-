-- ===== ВЫБОР ИНТЕРФЕЙСА (250x200) =====
local TweenService = game:GetService("TweenService")
local UserInputService = game:GetService("UserInputService")
local player = game.Players.LocalPlayer

local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "InterfaceChooser"
ScreenGui.Parent = player:WaitForChild("PlayerGui")
ScreenGui.ResetOnSpawn = false
ScreenGui.DisplayOrder = 999

-- Цвета
local ColorBg = Color3.fromRGB(10, 10, 10)
local ColorBtn = Color3.fromRGB(35, 35, 35)
local ColorBtnHover = Color3.fromRGB(55, 55, 55)
local ColorStroke = Color3.fromRGB(70, 70, 70)
local ColorNeon = Color3.fromRGB(180, 140, 255)
local ColorText = Color3.fromRGB(240, 240, 240)
local ColorSub = Color3.fromRGB(160, 160, 160)

-- Главное окно 250x200
local Main = Instance.new("Frame")
Main.Size = UDim2.new(0, 1, 0, 1)
Main.Position = UDim2.new(0.5, 0, 0.5, 0)
Main.BackgroundColor3 = ColorBg
Main.BorderSizePixel = 0
Main.ClipsDescendants = true
Main.Parent = ScreenGui

local MainCorner = Instance.new("UICorner")
MainCorner.CornerRadius = UDim.new(0, 12)
MainCorner.Parent = Main

-- Плавное появление из точки
TweenService:Create(Main, TweenInfo.new(0.45, Enum.EasingStyle.Back, Enum.EasingDirection.Out), {
    Size = UDim2.new(0, 250, 0, 200),
    Position = UDim2.new(0.5, -125, 0.5, -100)
}):Play()

-- Заголовок (слева)
local Title = Instance.new("TextLabel")
Title.Size = UDim2.new(1, -60, 0, 30)
Title.Position = UDim2.new(0, 15, 0, 10)
Title.BackgroundTransparency = 1
Title.Text = "Selecting an interface"
Title.TextColor3 = ColorText
Title.Font = Enum.Font.GothamBold
Title.TextSize = 14
Title.TextXAlignment = Enum.TextXAlignment.Left
Title.Parent = Main

-- Кастомный крестик (твой)
local CloseBtn = Instance.new("ImageButton")
CloseBtn.Size = UDim2.new(0, 22, 0, 22)
CloseBtn.Position = UDim2.new(1, -32, 0, 12)
CloseBtn.BackgroundTransparency = 1
CloseBtn.Image = "rbxassetid://83277910885129"
CloseBtn.ImageColor3 = Color3.fromRGB(200, 200, 200)
CloseBtn.Parent = Main

CloseBtn.MouseEnter:Connect(function()
    TweenService:Create(CloseBtn, TweenInfo.new(0.15), {ImageColor3 = Color3.fromRGB(255, 80, 80)}):Play()
end)
CloseBtn.MouseLeave:Connect(function()
    TweenService:Create(CloseBtn, TweenInfo.new(0.15), {ImageColor3 = Color3.fromRGB(200, 200, 200)}):Play()
end)
CloseBtn.MouseButton1Click:Connect(function()
    ScreenGui:Destroy()
end)

-- Линия-разделитель
local Line = Instance.new("Frame")
Line.Size = UDim2.new(1, -30, 0, 1)
Line.Position = UDim2.new(0, 15, 0, 42)
Line.BackgroundColor3 = ColorStroke
Line.BorderSizePixel = 0
Line.Parent = Main

-- Функция создания кнопки
local function createChoiceButton(text, tooltip, yPos)
    local Btn = Instance.new("TextButton")
    Btn.Size = UDim2.new(1, -30, 0, 50)
    Btn.Position = UDim2.new(0, 15, 0, yPos)
    Btn.BackgroundColor3 = ColorBtn
    Btn.Text = ""
    Btn.BorderSizePixel = 0
    Btn.AutoButtonColor = false
    Btn.Parent = Main

    local Corner = Instance.new("UICorner")
    Corner.CornerRadius = UDim.new(0, 8)
    Corner.Parent = Btn

    local Stroke = Instance.new("UIStroke")
    Stroke.Color = ColorStroke
    Stroke.Thickness = 1
    Stroke.Parent = Btn

    local BtnLabel = Instance.new("TextLabel")
    BtnLabel.Size = UDim2.new(0.7, 0, 0.55, 0)
    BtnLabel.Position = UDim2.new(0, 12, 0.15, 0)
    BtnLabel.BackgroundTransparency = 1
    BtnLabel.Text = text
    BtnLabel.TextColor3 = ColorText
    BtnLabel.Font = Enum.Font.GothamSemibold
    BtnLabel.TextSize = 14
    BtnLabel.TextXAlignment = Enum.TextXAlignment.Left
    BtnLabel.Parent = Btn

    local SubLabel = Instance.new("TextLabel")
    SubLabel.Size = UDim2.new(0.7, 0, 0.3, 0)
    SubLabel.Position = UDim2.new(0, 12, 0.6, 0)
    SubLabel.BackgroundTransparency = 1
    SubLabel.Text = tooltip
    SubLabel.TextColor3 = ColorSub
    SubLabel.Font = Enum.Font.Gotham
    SubLabel.TextSize = 9
    SubLabel.TextXAlignment = Enum.TextXAlignment.Left
    SubLabel.Parent = Btn

    Btn.MouseEnter:Connect(function()
        TweenService:Create(Btn, TweenInfo.new(0.15), {BackgroundColor3 = ColorBtnHover}):Play()
        TweenService:Create(Stroke, TweenInfo.new(0.15), {Color = ColorNeon, Thickness = 2}):Play()
    end)
    Btn.MouseLeave:Connect(function()
        TweenService:Create(Btn, TweenInfo.new(0.15), {BackgroundColor3 = ColorBtn}):Play()
        TweenService:Create(Stroke, TweenInfo.new(0.15), {Color = ColorStroke, Thickness = 1}):Play()
    end)

    return Btn
end

-- Кнопки выбора
local ClassicBtn = createChoiceButton("Classic", "Официальный интерфейс Premium Power", 55)
local RayfieldBtn = createChoiceButton("Rayfield", "Загрузка через библиотеку Rayfield", 115)

-- Скрытие меню и загрузка скрипта
local function hideAndLoad(url)
    local closeTween = TweenService:Create(Main, TweenInfo.new(0.3, Enum.EasingStyle.Quart, Enum.EasingDirection.In), {
        Size = UDim2.new(0, 250, 0, 1),
        Position = UDim2.new(0.5, -125, 0.5, 0),
        BackgroundTransparency = 1
    })
    closeTween:Play()
    closeTween.Completed:Connect(function()
        Main.Visible = false
        pcall(function()
            loadstring(game:HttpGet(url))()
        end)
    end)
end

ClassicBtn.MouseButton1Click:Connect(function()
    hideAndLoad("https://raw.githubusercontent.com/XneD-crypto/premium-power/refs/heads/main/officialPPP.lua")
end)

RayfieldBtn.MouseButton1Click:Connect(function()
    hideAndLoad("https://raw.githubusercontent.com/XneD-crypto/premium-power/refs/heads/main/Rayfiled%26lua")
end)

-- Перетаскивание
local dragging, dragInput, dragStart, startPos
Main.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.Touch then
        dragging = true
        dragStart = input.Position
        startPos = Main.Position
        input.Changed:Connect(function()
            if input.UserInputState == Enum.UserInputState.End then
                dragging = false
            end
        end)
    end
end)
Main.InputChanged:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.Touch and dragging then
        dragInput = input
    end
end)
UserInputService.InputChanged:Connect(function(input)
    if dragging and input == dragInput then
        local delta = input.Position - dragStart
        Main.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
    end
end)

print("[SelectingUI] Интерфейс 250x200 загружен.")
