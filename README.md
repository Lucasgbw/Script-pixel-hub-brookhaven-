local TweenService = game:GetService("TweenService")
local Players = game:GetService("Players")
local player = Players.LocalPlayer

-- IDs
local MUSIC_ID = "rbxassetid://1840684529"
local APITO_ID = "rbxassetid://1388573684"

-- Mensagens
local messages = {
    "pixel hub vai voltar às 15:00 da tarde.",
    "motivo: está tendo agora skins/Bundles korblox, perna de gelo, perna de zumbi, headless!",
    "obrigado pela atenção! :D"
}

-- Main UI
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "PixelHubModernNotice"
screenGui.Parent = player:WaitForChild("PlayerGui")
screenGui.IgnoreGuiInset = true
screenGui.ResetOnSpawn = false

-- Modern Black Frame com borda neon vermelha
local mainFrame = Instance.new("Frame")
mainFrame.Size = UDim2.new(0.7,0,0.30,0)
mainFrame.Position = UDim2.new(0.5,0,0.5,0)
mainFrame.AnchorPoint = Vector2.new(0.5,0.5)
mainFrame.BackgroundColor3 = Color3.fromRGB(10,10,10)
mainFrame.BackgroundTransparency = 1
mainFrame.ZIndex = 10
mainFrame.Parent = screenGui

local corner = Instance.new("UICorner")
corner.CornerRadius = UDim.new(0, 32)
corner.Parent = mainFrame

local neon = Instance.new("UIStroke")
neon.Thickness = 4
neon.Color = Color3.fromRGB(255,0,0)
neon.Transparency = 0.25
neon.Parent = mainFrame

-- Fundo preto (fade in/out)
local background = Instance.new("Frame")
background.Size = UDim2.new(1,0,1,0)
background.Position = UDim2.new(0,0,0,0)
background.BackgroundColor3 = Color3.fromRGB(0,0,0)
background.BackgroundTransparency = 1
background.ZIndex = 9
background.Parent = screenGui

-- Música de fundo
local music = Instance.new("Sound")
music.SoundId = MUSIC_ID
music.Volume = 0.7
music.Looped = false
music.Parent = screenGui

-- Apito
local apito = Instance.new("Sound")
apito.SoundId = APITO_ID
apito.Volume = 1
apito.Looped = false
apito.Parent = screenGui

-- Função para mostrar mensagem com animação
local function showMessage(text, delay, playApito)
    local label = Instance.new("TextLabel")
    label.Size = UDim2.new(0.95,0,0.24,0)
    label.Position = UDim2.new(0.5,0,0.5,0)
    label.AnchorPoint = Vector2.new(0.5,0.5)
    label.BackgroundTransparency = 1
    label.Text = text
    label.Font = Enum.Font.GothamBlack
    label.TextSize = 32
    label.TextColor3 = Color3.fromRGB(255,0,0) -- Neon vermelho
    label.TextTransparency = 1
    label.ZIndex = 11
    label.Parent = mainFrame

    -- Fade in
    TweenService:Create(label, TweenInfo.new(0.6, Enum.EasingStyle.Quad), {TextTransparency = 0}):Play()
    if playApito then apito:Play() end
    wait(delay-0.6)
    -- Fade out
    TweenService:Create(label, TweenInfo.new(0.6, Enum.EasingStyle.Quad), {TextTransparency = 1}):Play()
    wait(0.6)
    label:Destroy()
end

-- Fade in fundo e frame
TweenService:Create(background, TweenInfo.new(0.8, Enum.EasingStyle.Quad), {BackgroundTransparency = 0.15}):Play()
TweenService:Create(mainFrame, TweenInfo.new(0.8, Enum.EasingStyle.Quad), {BackgroundTransparency = 0.09}):Play()

music:Play()
wait(0.8)

-- Mensagem 1 (apito toca junto)
showMessage(messages[1], 2.5, true)
-- Mensagem 2
showMessage(messages[2], 2.5, false)
-- Mensagem 3
showMessage(messages[3], 2.5, false)

wait(2.5)

-- Fade out fundo e frame
TweenService:Create(background, TweenInfo.new(0.8, Enum.EasingStyle.Quad), {BackgroundTransparency = 1}):Play()
TweenService:Create(mainFrame, TweenInfo.new(0.8, Enum.EasingStyle.Quad), {BackgroundTransparency = 1}):Play()
wait(0.8)

screenGui:Destroy()
