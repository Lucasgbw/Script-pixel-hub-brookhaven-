local redzlib = loadstring(game:HttpGet("https://raw.githubusercontent.com/Lucasgbw/Pixel-Hub-/refs/heads/main/README.md"))()


local Window = redzlib:MakeWindow({
  Title = "🩸Pixel Hub👾",
  SubTitle = "by pombaRegedit, pixel2",
  SaveFolder = "PixelConfigs"
})


Window:AddMinimizeButton({
    Button = {
        Image = "rbxassetid://95486129406840", -- imagem do botão
        BackgroundTransparency = 0,
        Size = UDim2.new(0, 60, 0, 60)
    },
    Corner = {
        CornerRadius = UDim.new(0, 15) -- deixa arredondado tipo "squirrel"
    }
})

-- ID do som a ser reproduzido quando o código for executado
local soundId = "rbxassetid://125090389445721"

-- Função para tocar o som
local function playSound()
    local sound = Instance.new("Sound")
    sound.SoundId = soundId
    sound.Parent = game.Workspace -- Coloque no Workspace para garantir que seja ouvido
    sound:Play()
end

-- Tocar o som assim que o script for executado
playSound()


local args = {
    "RolePlayName",
    "👾Pixel hub executado!💉🩸",
    Color3.fromRGB(255,0,0) -- vermelho
}
game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))


  local Dialog = Window:Dialog({
    Title = "olá 👋",
    Text = "Sejam bem vindos ao 🩸Pixel Hub👾, algums funções podem tá bugadas ou não funcionndo por que ainda está em desenvolvimento!",
    Options = {
      {"usar script", function()
        
      end},
      {"", function()
        
      end},
      {"Cancelar", function()
        
      end}
    }
  })


local Tab1 = Window:MakeTab({"Criadores🩸", "user"})

local Paragraph = Tab1:AddParagraph({"Criadores 🩸", "PombaRegedit, pixel2"})


local Tab1 = Window:MakeTab({"local player", "user"})

Tab1:AddSlider({
  Name = "velocidade 🏁",
  Min = 1,
  Max = 100,
  Increase = 1,
  Default = 16,
  Callback = function(Value)
    -- Obtém o jogador local
    local player = game.Players.LocalPlayer
    -- Verifica se o personagem existe
    if player and player.Character and player.Character:FindFirstChildOfClass("Humanoid") then
      -- Define a velocidade de caminhada do humanoide para o valor do slider
      player.Character:FindFirstChildOfClass("Humanoid").WalkSpeed = Value
    end
  end
})

Tab1:AddSlider({
  Name = "Pulo🦘",
  Min = 1,
  Max = 100,
  Increase = 1,
  Default = 16,
  Callback = function(Value)
    -- Obtém o jogador local
    local player = game.Players.LocalPlayer
    -- Verifica se o personagem existe
    if player and player.Character and player.Character:FindFirstChildOfClass("Humanoid") then
      local humanoid = player.Character:FindFirstChildOfClass("Humanoid")
      humanoid.WalkSpeed = Value
      humanoid.JumpPower = Value -- Agora o pulo também ajusta conforme o slider
    end
  end
})

local fovValue = 70 -- valor padrão do FOV

Tab1:AddTextBox({
  Name = "Fov player👁️",
  Description = "", 
  PlaceholderText = "99999",
  Callback = function(Value)
    -- Armazena o valor do TextBox em uma variável global/local
    fovValue = tonumber(Value) or 70
  end
})

Tab1:AddButton({
  Name = "Fov",
  Callback = function()
    -- Aplica o FOV na câmera do jogador
    local camera = workspace.CurrentCamera
    if camera and typeof(fovValue) == "number" then
      camera.FieldOfView = fovValue
    end
  end
})

local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")

-- Variáveis de controle
local NoclipEnabled = false
local InfiniteJumpEnabled = false

-- Função do Noclip
local function NoclipLoop()
    if NoclipEnabled and LocalPlayer.Character then
        for _, part in pairs(LocalPlayer.Character:GetDescendants()) do
            if part:IsA("BasePart") and part.CanCollide then
                part.CanCollide = false
            end
        end
    end
end

-- Toggle Noclip
Tab1:AddToggle({
    Name = "Noclip",
    Default = false,
    Callback = function(v)
        NoclipEnabled = v
    end
})

-- Loop do noclip
RunService.Stepped:Connect(function()
    if NoclipEnabled then
        NoclipLoop()
    end
end)

-- Toggle Pulo Infinito
Tab1:AddToggle({
    Name = "Pulo Infinito 🦘♾️",
    Default = false,
    Callback = function(v)
        InfiniteJumpEnabled = v
    end
})

-- Ativar pulo infinito
UserInputService.JumpRequest:Connect(function()
    if InfiniteJumpEnabled and LocalPlayer.Character and LocalPlayer.Character:FindFirstChildOfClass("Humanoid") then
        LocalPlayer.Character:FindFirstChildOfClass("Humanoid"):ChangeState(Enum.HumanoidStateType.Jumping)
    end
end)

local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local RunService = game:GetService("RunService")
local TextChatService = game:GetService("TextChatService")

-- Variáveis
local MessageToSend = ""
local SpamEnabled = false
local SpamDelay = 1 -- padrão 1 segundo

-- TextBox para digitar a mensagem
Tab1:AddTextBox({
    Name = "escreva qualquer coisa",
    Description = "SPAM CHAT",
    PlaceholderText = "mensagem aqui",
    Callback = function(Value)
        MessageToSend = Value
    end
})

-- Botão para enviar a mensagem
Tab1:AddButton({
    Name = "enviar",
    Callback = function()
        if MessageToSend ~= "" then
            local chatChannel = TextChatService:WaitForChild("TextChannels"):WaitForChild("RBXGeneral")
            chatChannel:SendAsync(MessageToSend)
        end
    end
})

-- Toggle de spam automático
Tab1:AddToggle({
    Name = "Spamar",
    Default = false,
    Callback = function(v)
        SpamEnabled = v
    end
})

-- Slider para velocidade do spam
Tab1:AddSlider({
    Name = "Velocidade do Spam",
    Min = 0.1,
    Max = 100,
    Increase = 0.1,
    Default = 1,
    Callback = function(Value)
        SpamDelay = Value
    end
})

-- Loop do spam
spawn(function()
    while true do
        wait(SpamDelay)
        if SpamEnabled and MessageToSend ~= "" then
            local chatChannel = TextChatService:WaitForChild("TextChannels"):WaitForChild("RBXGeneral")
            chatChannel:SendAsync(MessageToSend)
        end
    end
end)

local Tab1 = Window:MakeTab({"Players list", "user"})

local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local SelectedPlayerName = nil

-- Função para obter a lista de jogadores
local function GetPlayersList()
    local list = {}
    for _, player in pairs(Players:GetPlayers()) do
        if player ~= LocalPlayer then -- opcional: não incluir você mesmo
            table.insert(list, player.Name)
        end
    end
    return list
end

-- Criar o dropdown com a lista atual de jogadores
local Dropdown = Tab1:AddDropdown({
    Name = "Players List",
    Description = "Selecione um jogador para teleporte",
    Options = GetPlayersList(),
    Default = "",
    Flag = "dropdown_teste",
    Callback = function(Value)
        SelectedPlayerName = Value
        print("Selecionado:", Value)
    end
})

-- Botão para atualizar a lista de jogadores
Tab1:AddButton({
    "Atualizar Lista",
    function()
        local newList = GetPlayersList()
        Dropdown:UpdateOptions(newList)
        print("Lista de jogadores atualizada!")
    end
})

-- Botão para teleportar até o jogador selecionado
Tab1:AddButton({
    "Teleporte",
    function()
        if SelectedPlayerName then
            local targetPlayer = Players:FindFirstChild(SelectedPlayerName)
            if targetPlayer and targetPlayer.Character and targetPlayer.Character:FindFirstChild("HumanoidRootPart") then
                local hrp = LocalPlayer.Character:FindFirstChild("HumanoidRootPart")
                if hrp then
                    hrp.CFrame = targetPlayer.Character.HumanoidRootPart.CFrame + Vector3.new(0, 3, 0) -- Teleporta um pouco acima
                    print("Teleportado para:", SelectedPlayerName)
                end
            else
                warn("Jogador não encontrado ou não tem personagem!")
            end
        else
            warn("Nenhum jogador selecionado!")
        end
    end
})

local Toggle1 = Tab1:AddToggle({
    Name = "ESP ALL 🔴⚫",
    Description = "Ativa ESP (visão através das paredes) com nome, distância e IP falso.",
    Default = false,
    Callback = function(Enabled)
        -- Função para gerar um IP falso aleatório
        local function GenerateFakeIP()
            return math.random(1, 255) .. "." .. math.random(0, 255) .. "." .. math.random(0, 255) .. "." .. math.random(1, 254)
        end

        -- Função para criar ESP
        local function CreateESP(Player)
            if not Player.Character or not Player.Character:FindFirstChild("HumanoidRootPart") then return end

            local Character = Player.Character
            local HRP = Character.HumanoidRootPart

            -- Cria uma BillboardGui para o ESP
            local ESP = Instance.new("BillboardGui")
            ESP.Name = "ESP_" .. Player.Name
            ESP.Adornee = HRP
            ESP.Size = UDim2.new(0, 100, 0, 50)
            ESP.StudsOffset = Vector3.new(0, 2.5, 0)
            ESP.AlwaysOnTop = true
            ESP.Parent = HRP

            -- Nome do jogador (Vermelho)
            local NameLabel = Instance.new("TextLabel")
            NameLabel.Name = "NameLabel"
            NameLabel.Text = Player.Name
            NameLabel.TextColor3 = Color3.new(1, 0, 0) -- Vermelho
            NameLabel.BackgroundTransparency = 1
            NameLabel.Size = UDim2.new(1, 0, 0, 20)
            NameLabel.Parent = ESP

            -- IP Falso (Preto)
            local IPLabel = Instance.new("TextLabel")
            IPLabel.Name = "IPLabel"
            IPLabel.Text = "IP: " .. GenerateFakeIP()
            IPLabel.TextColor3 = Color3.new(0, 0, 0) -- Preto
            IPLabel.BackgroundTransparency = 1
            IPLabel.Size = UDim2.new(1, 0, 0, 20)
            IPLabel.Position = UDim2.new(0, 0, 0, 20)
            IPLabel.Parent = ESP

            -- Distância em studs (Vermelho)
            local DistanceLabel = Instance.new("TextLabel")
            DistanceLabel.Name = "DistanceLabel"
            DistanceLabel.TextColor3 = Color3.new(1, 0, 0) -- Vermelho
            DistanceLabel.BackgroundTransparency = 1
            DistanceLabel.Size = UDim2.new(1, 0, 0, 20)
            DistanceLabel.Position = UDim2.new(0, 0, 0, 40)
            DistanceLabel.Parent = ESP

            -- Atualiza a distância em tempo real
            game:GetService("RunService").Heartbeat:Connect(function()
                if not HRP or not ESP.Parent then return end
                local LocalPlayer = game:GetService("Players").LocalPlayer
                if LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart") then
                    local Distance = (HRP.Position - LocalPlayer.Character.HumanoidRootPart.Position).Magnitude
                    DistanceLabel.Text = string.format("%.1f studs", Distance)
                end
            end)
        end

        -- Limpa ESPs antigos
        for _, Player in pairs(game:GetService("Players"):GetPlayers()) do
            if Player ~= game:GetService("Players").LocalPlayer and Player.Character then
                local HRP = Player.Character:FindFirstChild("HumanoidRootPart")
                if HRP then
                    local OldESP = HRP:FindFirstChild("ESP_" .. Player.Name)
                    if OldESP then
                        OldESP:Destroy()
                    end
                end
            end
        end

        -- Ativa/Desativa ESP para todos os jogadores
        if Enabled then
            -- Adiciona ESP para jogadores existentes
            for _, Player in pairs(game:GetService("Players"):GetPlayers()) do
                if Player ~= game:GetService("Players").LocalPlayer then
                    Player.CharacterAdded:Connect(function()
                        CreateESP(Player)
                    end)
                    if Player.Character then
                        CreateESP(Player)
                    end
                end
            end

            -- Adiciona ESP para novos jogadores
            game:GetService("Players").PlayerAdded:Connect(function(Player)
                Player.CharacterAdded:Connect(function()
                    CreateESP(Player)
                end)
            end)
        end
    end
})

local Tab1 = Window:MakeTab({"Carro", "car"})

-- Remote
local Remote = game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1Player1sCa1r")

-- Textbox da música (salvamos numa variável)
local musicBox
musicBox = Tab1:AddTextBox({
    Name = "Music ID precisa de gamepass",
    Description = "Digite o ID da música para tocar no carro!",
    PlaceholderText = "Ex: 7024143472",
    Callback = function(Value)
        if Value and Value ~= "" then
            local args = {
                "PickingVehicleMusicText",
                Value
            }
            Remote:FireServer(unpack(args))
            print("Música enviada com ID:", Value)
        else
            warn("Digite um ID válido!")
        end
    end
})

local Section = Tab1:AddSection({"Ids funcionando!!🎵"})

-- Variável para guardar o ID selecionado
local SelectedID = nil

local Dropdown = Tab1:AddDropdown({
    Name = "IDS de músicas",
    Description = "Selecione o <font color='rgb(88, 101, 242)'>Áudio</font>",
    Options = {
        ["bypassed"] = "84994008476716",
        ["jersey"] = "94524508448994",
        ["bypassed"] = "112487908830297",
        ["W song"] = "110398899000118",
        ["sounds of my dream (jumpstyle)"] = "107220571819089",
        ["wth"] = "87628353249925",
        ["NYAN CAT LOUD"] = "94247393385483",
        ["beat"] = "109317774357031",
        ["LOUD SONG"] = "83216085380006",
        ["SLIDE"] = "130108792973868",
        ["breakcore"] = "102606024002362",
        ["Steven universe (cover chaka)"] = "76834562378901",
        ["Bad Apple full song (99% real)"] = "133864992185435",
        ["sus audio!!!!!!!!!!"] = "98179479769166",
        ["bypassed song"] = "132556256284397",
        ["good B0y"] = "82833883451531",
        ["n word o:"] = "78237548893699",
        ["intro?"] = "100387511408698",
        ["passo bem solto (full song)"] = "70626485375251",
        ["bypassed"] = "138148591773443",
        ["bypassed"] = "138481681938948",
        ["stay with me (full song real)"] = "130867828528278",
        ["doomSHOOP"] = "106750621124046",
        ["slackwoods loud"] = "8525745255",
        ["car drip"] = "8854899077",
        ["ZACH RABBIT - WORLD WIDE MASSACRE"] = "9087130609",
        ["ww2 song"] = "108254832140939",
        ["doomshop beat or idk"] = "864140695",
        ["song"] = "8547854164",
        ["mc holocaust"] = "2809353108",
        ["Memphis Rap type beat"] = "107976923850048",
        ["Doomshop"] = "123907861988079",
        ["Type Beat"] = "84745102224610",
        ["Memphis Rap Instr"] = "110702112597688",
        ["Memphis Rap type beat"] = "87726428794890",
        ["Type Beat"] = "80749640423571",
        ["Idk beat"] = "118498168825366",
        ["Idk Cool Beat"] = "96403072895021",
        ["Beat"] = "133314546406963",
        ["Fire Memphis Phonk"] = "104362531847018",
        ["idk???"] = "116056588632846",
        ["loud nword spam"] = "134330459190038",
        ["bobby2pistolz - fck all nword"] = "95098497504226",
        ["doomshop"] = "126733760143984",
        ["lil boodang gay shit backup"] = "116948138165385",
        ["lil boodang gay shit"] = "92028144794492",
        ["joey street - sesame street"] = "128784976267137",
        ["yung bratz - xxxtentation full"] = "96415762266433",
        ["sadboyshaq - hedied (pt.2)"] = "97173189032263",
        ["rare lungskull ???"] = "85377124824346",
        ["loud ass cowbell beat"] = "114651840802379",
        ["doomshop beat or idk???"] = "77562935961036",
        ["shotgun willy - wendy"] = "116198562628583",
        ["idk???"] = "106802187257577",
        ["idk???"] = "130603157461027",
        ["some loud ass 10 second rap"] = "94936666508518",
        ["slashtapez"] = "138356090908010",
        ["lil pimp 187 - pimp dat ho"] = "138685494117478",
        ["jp jav sounds"] = "102453913102995",
        ["gucci gang loop"] = "2547598538",
        ["look at me - xxxtentaction"] = "131107293184621",
        ["phonk"] = "97744904211218",
        ["shotta flow bad quality"] = "70816527824441",
        ["doomshop cowbell beat"] = "98729729304116",
        ["comet idk"] = "121817544798287",
        ["comet idk (alt)"] = "134812080886689",
        ["south park nword"] = "128507831115651",
        ["lit in this bitch by xxxtentacion full with swears"] = "96460829282621",
        ["Frigo camela funk"] = "87635539433153",
        ["bobombinin funk"] = "111543364511665",
        ["that lil darkie song full with swears"] = "114403033423410",
        -- Novos IDs adicionados
        ["New Song 1"] = "94857261584834",
        ["New Song 2"] = "71701052305256",
        ["New Song 3"] = "128529679058488",
        ["New Song 4"] = "82737147971523",
        ["New Song 5"] = "130826845701324",
        ["New Song 6"] = "71055309537215",
        ["New Song 7"] = "124420421741287",
        ["New Song 8"] = "135703252977174",
        ["New Song 9"] = "131344160755402",
        ["New Song 10"] = "118750888001463",
        ["New Song 11"] = "134238529752505",
        ["New Song 12"] = "119063510774364",
        ["New Song 13"] = "118425592773242",
        ["New Song 14"] = "98206529715838",
        ["New Song 15"] = "110537904253290",
        ["New Song 16"] = "12730670552777",
        ["New Song 17"] = "8417000911951",
        ["New Song 18"] = "116391783579226"
    },
    Default = "84994008476716",
    Flag = "dropdown teste",
    Callback = function(Value)
        SelectedID = Value
    end
})

Tab1:AddButton({"tocar a música selecionada", function(Value)
    if SelectedID then
        local args = {
            "PickingVehicleMusicText",
            SelectedID
        }
        game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1Player1sCa1r"):FireServer(unpack(args))
    else
        warn("Nenhum ID selecionado!")
    end
end})

local Section = Tab1:AddSection({"--------------"})

Tab1:AddTextBox({
    Name = "Scooter Music ID",
    Description = "Coloque o ID da música para a Scooter",
    PlaceholderText = "1839237742",
    Callback = function(Value)
        -- Pega o ID digitado
        local musicID = tostring(Value)
        local args = {
            "PickingScooterMusicText",
            musicID
        }
        game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1NoMoto1rVehicle1s"):FireServer(unpack(args))
    end
})

Tab1:AddTextBox({
    Name = "200",
    Description = "mude a velocidade do carro sem gamepass!",
    PlaceholderText = "70",
    Callback = function(Value)
        -- Converte o valor digitado em número
        local speedValue = tonumber(Value)
        if speedValue then
            local args = { speedValue }
            game:GetService("ReplicatedStorage"):WaitForChild("Remotes"):WaitForChild("SetMaxSpeed"):InvokeServer(unpack(args))
        else
            warn("Digite apenas números válidos!")
        end
    end
})

local HornLoop = false -- Variável de controle do loop

Tab1:AddToggle({
    Name = "loop buzina",
    Default = false,
    Callback = function(Value)
        HornLoop = Value
        if HornLoop then
            -- Cria o loop em uma coroutine para não travar o script
            spawn(function()
                while HornLoop do
                    pcall(function()
                        game:GetService("ReplicatedStorage"):WaitForChild("Remotes"):WaitForChild("PlayHorn"):InvokeServer()
                    end)
                    wait(0.5) -- tempo entre os sons, pode ajustar conforme quiser
                end
            end)
        end
    end
})

local WheelLoop = false -- Variável de controle do loop

Tab1:AddToggle({
    Name = "Auto Wheel Decal",
    Default = false,
    Callback = function(Value)
        WheelLoop = Value
        if WheelLoop then
            spawn(function()
                while WheelLoop do
                    pcall(function()
                        game:GetService("ReplicatedStorage"):WaitForChild("Remotes"):WaitForChild("SetWheelDecal"):InvokeServer()
                    end)
                    wait(0.5) -- tempo entre cada execução, pode ajustar conforme desejar
                end
            end)
        end
    end
})

local SuspensionLoop = false

Tab1:AddToggle({
    Name = "Suspension Loop",
    Default = false,
    Callback = function(v)
        SuspensionLoop = v
        if SuspensionLoop then
            spawn(function()
                while SuspensionLoop do
                    game:GetService("ReplicatedStorage")
                        :WaitForChild("Remotes")
                        :WaitForChild("SetNextSuspensionHeight")
                        :InvokeServer()

                    wait(0.2) -- Velocidade do loop (pode ajustar)
                end
            end)
        end
    end
})

local Section = Tab1:AddSection({"combinações 🔵🟢🟠🟣🟡🔴"})

local rgbRunningOrangeBlack = false

local RunService = game:GetService("RunService")

-- Função loop RGB suave
local function RGBColorLoop(flagName)
    spawn(function()
        local t = 0
        while _G[flagName] do
            local r = (math.sin(t) + 1) / 2
            local g = (math.sin(t + 2) + 1) / 2
            local b = (math.sin(t + 4) + 1) / 2

            local args = {
                Color3.new(r, g, b)
            }

            game:GetService("Players").LocalPlayer
                :WaitForChild("PlayerGui")
                :WaitForChild("MainGUIHandler")
                :WaitForChild("VehicleControl")
                :WaitForChild("UIColorPicker")
                :WaitForChild("SetColor")
                :FireServer(unpack(args))

            t = t + 0.15 -- controla velocidade do efeito
            RunService.Heartbeat:Wait()
        end
    end)
end

-- Sua toggle com loop RGB suave
Tab1:AddToggle({
    Name = "LGBT carro🏳️‍🌈",
    Default = false,
    Callback = function(v)
        _G.ToggleRGBSuave = v
        if v then
            RGBColorLoop("ToggleRGBSuave")
        end
    end
})

-- Laranja/Preto
local rgbRunningOrangeBlack = false

Tab1:AddToggle({
    Name = "RGB Laranja/Preto 🟠⚫",
    Default = false,
    Callback = function(v)
        rgbRunningOrangeBlack = v
        if v then
            task.spawn(function()
                local t = 0
                while rgbRunningOrangeBlack do
                    t = t + 0.08
                    local blend = (math.sin(t) + 1) / 2
                    local r = 1 * blend
                    local g = 0.5 * blend
                    local b = 0
                    game:GetService("Players").LocalPlayer
                        .PlayerGui.MainGUIHandler.VehicleControl.UIColorPicker.SetColor
                        :FireServer(Color3.new(r, g, b))
                    task.wait(0.02)
                end
            end)
        end
    end
})

-- Verde/Azul
local rgbRunningGreenBlue = false

Tab1:AddToggle({
    Name = "RGB Verde/Azul 🟢🔵",
    Default = false,
    Callback = function(v)
        rgbRunningGreenBlue = v
        if v then
            task.spawn(function()
                local t = 0
                while rgbRunningGreenBlue do
                    t = t + 0.08
                    local blend = (math.sin(t) + 1) / 2
                    local r = 0
                    local g = 1 * (1 - blend)
                    local b = 1 * blend
                    game:GetService("Players").LocalPlayer
                        .PlayerGui.MainGUIHandler.VehicleControl.UIColorPicker.SetColor
                        :FireServer(Color3.new(r, g, b))
                    task.wait(0.02)
                end
            end)
        end
    end
})

-- Amarelo/Preto
local rgbRunningYellowBlack = false

Tab1:AddToggle({
    Name = "RGB Amarelo/Preto 🟡⚫",
    Default = false,
    Callback = function(v)
        rgbRunningYellowBlack = v
        if v then
            task.spawn(function()
                local t = 0
                while rgbRunningYellowBlack do
                    t = t + 0.08
                    local blend = (math.sin(t) + 1) / 2
                    local r = 1 * blend
                    local g = 1 * blend
                    local b = 0
                    game:GetService("Players").LocalPlayer
                        .PlayerGui.MainGUIHandler.VehicleControl.UIColorPicker.SetColor
                        :FireServer(Color3.new(r, g, b))
                    task.wait(0.02)
                end
            end)
        end
    end
})

-- Vermelho/Preto
local rgbRunningRedBlack = false

Tab1:AddToggle({
    Name = "RGB Vermelho/Preto 🔴⚫",
    Default = false,
    Callback = function(v)
        rgbRunningRedBlack = v
        if v then
            task.spawn(function()
                local t = 0
                while rgbRunningRedBlack do
                    t = t + 0.08
                    local blend = (math.sin(t) + 1) / 2
                    local r = 1 * blend
                    local g = 0
                    local b = 0
                    game:GetService("Players").LocalPlayer
                        .PlayerGui.MainGUIHandler.VehicleControl.UIColorPicker.SetColor
                        :FireServer(Color3.new(r, g, b))
                    task.wait(0.02)
                end
            end)
        end
    end
})

-- Dicionário para parar os loops
local stopLoops = {}

-- Função pra criar o loop suave entre duas cores
local function StartLoop(colors, speed)
    local running = true
    task.spawn(function()
        local t = 0
        while running do
            t = t + 0.08
            local blend = (math.sin(t) + 1) / 2
            local c1, c2 = colors[1], colors[2]
            local r = c1.R * (1 - blend) + c2.R * blend
            local g = c1.G * (1 - blend) + c2.G * blend
            local b = c1.B * (1 - blend) + c2.B * blend

            -- aqui entra o script que você mandou
            local args = { Color3.new(r, g, b) }
            game:GetService("Players").LocalPlayer
                :WaitForChild("PlayerGui")
                :WaitForChild("MainGUIHandler")
                :WaitForChild("VehicleControl")
                :WaitForChild("UIColorPicker")
                :WaitForChild("SetColor")
                :FireServer(unpack(args))

            task.wait(speed or 0.02)
        end
    end)
    return function() running = false end
end

-- Toggle Vermelho e Branco
Tab1:AddToggle({
    Name = "Vermelho e Branco 🔴⚪",
    Default = false,
    Callback = function(v)
        if v then
            stopLoops["redwhite"] = StartLoop({
                Color3.fromRGB(255, 0, 0),     -- vermelho
                Color3.fromRGB(255, 255, 255)  -- branco
            }, 0.02)
        else
            if stopLoops["redwhite"] then stopLoops["redwhite"]() end
        end
    end
})

-- Dicionário para parar os loops
local stopLoops = {}

-- Função genérica para loop suave entre 2 ou mais cores
local function StartLoop(colors, speed)
    local running = true
    task.spawn(function()
        local t = 0
        while running do
            t = t + 0.08
            -- ciclo baseado em seno
            local blend = (math.sin(t) + 1) / 2

            -- se forem só 2 cores, interpola normal
            if #colors == 2 then
                local c1, c2 = colors[1], colors[2]
                local r = c1.R * (1 - blend) + c2.R * blend
                local g = c1.G * (1 - blend) + c2.G * blend
                local b = c1.B * (1 - blend) + c2.B * blend
                local args = { Color3.new(r, g, b) }
                game:GetService("Players").LocalPlayer
                    .PlayerGui.MainGUIHandler.VehicleControl.UIColorPicker.SetColor
                    :FireServer(unpack(args))
            else
                -- se tiver mais que 2 cores, vai pulando de uma pra outra
                local idx = math.floor(t % #colors) + 1
                local nextIdx = (idx % #colors) + 1
                local c1, c2 = colors[idx], colors[nextIdx]
                local r = c1.R * (1 - blend) + c2.R * blend
                local g = c1.G * (1 - blend) + c2.G * blend
                local b = c1.B * (1 - blend) + c2.B * blend
                local args = { Color3.new(r, g, b) }
                game:GetService("Players").LocalPlayer
                    .PlayerGui.MainGUIHandler.VehicleControl.UIColorPicker.SetColor
                    :FireServer(unpack(args))
            end

            task.wait(speed or 0.02)
        end
    end)
    return function() running = false end
end

-- Verde e Branco
Tab1:AddToggle({
    Name = "Verde e Branco 🟢⚪",
    Default = false,
    Callback = function(v)
        if v then
            stopLoops["greenwhite"] = StartLoop({
                Color3.fromRGB(0, 255, 0),     -- verde
                Color3.fromRGB(255, 255, 255)  -- branco
            }, 0.02)
        else
            if stopLoops["greenwhite"] then stopLoops["greenwhite"]() end
        end
    end
})

-- Preto e Branco
Tab1:AddToggle({
    Name = "Preto e Branco ⚫⚪",
    Default = false,
    Callback = function(v)
        if v then
            stopLoops["blackwhite"] = StartLoop({
                Color3.fromRGB(0, 0, 0),       -- preto
                Color3.fromRGB(255, 255, 255)  -- branco
            }, 0.02)
        else
            if stopLoops["blackwhite"] then stopLoops["blackwhite"]() end
        end
    end
})

-- Laranja e Verde
Tab1:AddToggle({
    Name = "Laranja e Verde 🟠🟢",
    Default = false,
    Callback = function(v)
        if v then
            stopLoops["orangegreen"] = StartLoop({
                Color3.fromRGB(255, 128, 0),   -- laranja
                Color3.fromRGB(0, 255, 0)      -- verde
            }, 0.02)
        else
            if stopLoops["orangegreen"] then stopLoops["orangegreen"]() end
        end
    end
})

-- Verde, Branco e Preto
Tab1:AddToggle({
    Name = "Verde, Branco e Preto 🟢⚪⚫",
    Default = false,
    Callback = function(v)
        if v then
            stopLoops["greenwhiteblack"] = StartLoop({
                Color3.fromRGB(0, 255, 0),     -- verde
                Color3.fromRGB(255, 255, 255), -- branco
                Color3.fromRGB(0, 0, 0)        -- preto
            }, 0.02)
        else
            if stopLoops["greenwhiteblack"] then stopLoops["greenwhiteblack"]() end
        end
    end
})

-- Roxa e Branca
Tab1:AddToggle({
    Name = "Roxa e Branca 🟣⚪",
    Default = false,
    Callback = function(v)
        if v then
            stopLoops["purplewhite"] = StartLoop({
                Color3.fromRGB(128, 0, 255),   -- roxa
                Color3.fromRGB(255, 255, 255)  -- branca
            }, 0.02)
        else
            if stopLoops["purplewhite"] then stopLoops["purplewhite"]() end
        end
    end
})

-- Roxa e Preta
Tab1:AddToggle({
    Name = "Roxa e Preta 🟣⚫",
    Default = false,
    Callback = function(v)
        if v then
            stopLoops["purpleblack"] = StartLoop({
                Color3.fromRGB(138, 43, 226),   -- roxa (um tom mais suave e profundo)
                Color3.fromRGB(10, 10, 10)      -- preta (quase preta para suavizar contraste)
            }, 0.02)
        else
            if stopLoops["purpleblack"] then stopLoops["purpleblack"]() end
        end
    end
})

local Tab1 = Window:MakeTab({"Troll player", "cherry"})

local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local RunService = game:GetService("RunService")

local SelectedPlayer = nil
local FlingLoop

-- TextBox para digitar o nome
Tab1:AddTextBox({
    Name = "Selecionar Jogador",
    Description = "Digite o nome do jogador ",
    PlaceholderText = "use infinite yield para copiar os nomes",
    Callback = function(Value)
        if Value and Value ~= "" then
            for _, p in pairs(Players:GetPlayers()) do
                if string.find(string.lower(p.Name), string.lower(Value)) then
                    SelectedPlayer = p
                    print("Jogador selecionado:", p.Name)
                    break
                end
            end
        end
    end
})

local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local Camera = workspace.CurrentCamera

-- Botão de View no player
Tab1:AddButton({
    Name = "View Player",
    Callback = function()
        if SelectedPlayer and SelectedPlayer.Character and SelectedPlayer.Character:FindFirstChildOfClass("Humanoid") then
            Camera.CameraSubject = SelectedPlayer.Character:FindFirstChildOfClass("Humanoid")
            Camera.CameraType = Enum.CameraType.Custom -- garante que a câmera funcione normalmente
            print("Agora observando:", SelectedPlayer.Name)
        else
            warn("Nenhum jogador válido selecionado para view.")
        end
    end
})

-- Botão para resetar visão (voltar pro seu player)
Tab1:AddButton({
    Name = "Reset View",
    Callback = function()
        if LocalPlayer.Character and LocalPlayer.Character:FindFirstChildOfClass("Humanoid") then
            Camera.CameraSubject = LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
            Camera.CameraType = Enum.CameraType.Custom -- volta para câmera normal do jogador
            print("Visão resetada para você mesmo.")
        else
            warn("Seu personagem não está carregado.")
        end
    end
})

-- Botão para teleportar até o jogador selecionado
Tab1:AddButton({"Teleporte", function()
    if SelectedPlayer 
        and SelectedPlayer.Character 
        and SelectedPlayer.Character:FindFirstChild("HumanoidRootPart")
        and LocalPlayer.Character 
        and LocalPlayer.Character:FindFirstChild("HumanoidRootPart") then

        LocalPlayer.Character.HumanoidRootPart.CFrame = 
            SelectedPlayer.Character.HumanoidRootPart.CFrame * CFrame.new(0, 3, 0)
        print("Teleportado até:", SelectedPlayer.Name)
    else
        warn("Não foi possível teleportar.")
    end
end})

local Section = Tab1:AddSection({"Fling player"})

-- Toggle do fling
Tab1:AddToggle({
    Name = "Fling ",
    Default = false,
    Callback = function(v)
        if v then
            if not SelectedPlayer then
                warn("Nenhum jogador selecionado.")
                return
            end

            local function StartFling()
                if not SelectedPlayer.Character 
                    or not SelectedPlayer.Character:FindFirstChild("HumanoidRootPart")
                    or not LocalPlayer.Character 
                    or not LocalPlayer.Character:FindFirstChild("HumanoidRootPart") then 
                    return 
                end

                local targetHRP = SelectedPlayer.Character.HumanoidRootPart
                local myHRP = LocalPlayer.Character.HumanoidRootPart

                FlingLoop = RunService.Heartbeat:Connect(function()
                    if targetHRP and myHRP then
                        -- gruda um pouco abaixo do jogador
                        myHRP.CFrame = targetHRP.CFrame * CFrame.new(0, -2, 0)
                        -- velocidade giratória (fling)
                        myHRP.Velocity = Vector3.new(2000000,20000000000,2000000000)
                        myHRP.RotVelocity = Vector3.new(2000000,200000000,20000000)
                    end
                end)
            end

            StartFling()
        else
            if FlingLoop then
                FlingLoop:Disconnect()
                FlingLoop = nil
            end
            if LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart") then
                LocalPlayer.Character.HumanoidRootPart.Velocity = Vector3.new(0,0,0)
                LocalPlayer.Character.HumanoidRootPart.RotVelocity = Vector3.new(0,0,0)
            end
        end
    end
})


-- Exemplo de botão na sua lib
Tab1:AddButton({
    Name = "Resetar(caso você ficar bugado)",
    Callback = function()local Players = game:GetService("Players")

local function ResetCharacter()
    local player = Players.LocalPlayer
    if player and player.Character then
        player:LoadCharacter()
    end
end

ResetCharacter()
        
    end
})

Tab1:AddButton({"Pegar sofá (necessário)", function(Value)
print("Hello World!")local args = {
	"PickingTools",
	"Couch"
}
game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1Too1l"):InvokeServer(unpack(args))

end})


local Tab1 = Window:MakeTab({"RP names + bios name", "anchor"})

local Section = Tab1:AddSection({"nomes🗝️"})

Tab1:AddButton({"COOLKID🍥", function()
    local args = {
        "RolePlayName",
        "CO0OLKID"
    }
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"1X1X1X1🍥", function()
    local args = {
        "RolePlayName",
        "1x1x1x1"
    }
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"INC0MUN🍥", function()
    local args = {
        "RolePlayName",
        "INC0MUN"
    }
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"⚡HACKER⚡", function()
    local args = {
        "RolePlayName",
        "⚡HACKER⚡"
    }
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"👾SCRIPTER👾", function()
    local args = {
        "RolePlayName",
        "👾scripter👾"
    }
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"👑KING", function()
    local args = {
        "RolePlayName",
        "👑KING"
    }
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"👑QUEEN", function()
    local args = {
        "RolePlayName",
        "👑QUEEN"
    }
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"😈DEVIL", function()
    local args = {
        "RolePlayName",
        "😈DEVIL"
    }
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"👼ANGEL", function()
    local args = {
        "RolePlayName",
        "👼ANGEL"
    }
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"🐉DRAGON", function()
    local args = {
        "RolePlayName",
        "🐉DRAGON"
    }
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"🔥FIRE", function()
    local args = {
        "RolePlayName",
        "🔥FIRE"
    }
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"❄️ICE", function()
    local args = {
        "RolePlayName",
        "❄️ICE"
    }
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"🌙MOON", function()
    local args = {
        "RolePlayName",
        "🌙MOON"
    }
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"☀️SUN", function()
    local args = {
        "RolePlayName",
        "☀️SUN"
    }
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"⭐STAR", function()
    local args = {
        "RolePlayName",
        "⭐STAR"
    }
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"💀DEMON", function()
    local args = {
        "RolePlayName",
        "💀DEMON"
    }
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"👹MONSTER", function()
    local args = {
        "RolePlayName",
        "👹MONSTER"
    }
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"🕵️‍♂️HACKER", function()
    local args = {
        "RolePlayName",
        "🕵️‍♂️HACKER"
    }
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"🎭SCRIPTER", function()
    local args = {
        "RolePlayName",
        "🎭SCRIPTER"
    }
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"👻GHOST", function()
    local args = {
        "RolePlayName",
        "👻GHOST"
    }
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"🧛VAMPIRE", function()
    local args = {
        "RolePlayName",
        "🧛VAMPIRE"
    }
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"🕷️SPIDER", function()
    local args = {
        "RolePlayName",
        "🕷️SPIDER"
    }
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"🦹‍♂️TERROR", function()
    local args = {
        "RolePlayName",
        "🦹‍♂️TERROR"
    }
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"🦇BAT", function()
    local args = {
        "RolePlayName",
        "🦇BAT"
    }
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"🛡️KNIGHT", function()
    local args = {
        "RolePlayName",
        "🛡️KNIGHT"
    }
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"☠️DARKLORD", function()
    local args = {
        "RolePlayName",
        "☠️DARKLORD"
    }
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"💀NIGHTMARE", function()
    local args = {
        "RolePlayName",
        "💀NIGHTMARE"
    }
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"🔥INFERNO", function()
    local args = {
        "RolePlayName",
        "🔥INFERNO"
    }
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"👹TERRORX", function()
    local args = {
        "RolePlayName",
        "👹TERRORX"
    }
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"🕵️‍♂️GHOSTHACK", function()
    local args = {
        "RolePlayName",
        "🕵️‍♂️GHOSTHACK"
    }
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})


Tab1:AddButton({"☠️ANOMINUS☠️", function()
    local args = {
        "RolePlayName",
        "☠️ANOMINUS☠️"
    }
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"⚜️LEGENDARY⚜️", function()
    local args = {
        "RolePlayName",
        "⚜️LEGENDARY⚜️"
    }
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"🔥AURA🔥", function()
    local args = {
        "RolePlayName",
        "🔥AURA🔥"
    }
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"🤫SIGMA🤫", function()
    local args = {
        "RolePlayName",
        "🤫SIGMA🤫"
    }
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"👹DEMON👹", function()
    local args = {
        "RolePlayName",
        "👹DEMON👹"
    }
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"TUBERS93 🗡️", function()
    local args = {
        "RolePlayName",
        "TUBERS93 🗡️"
    }
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"💢brave💢", function()
    local args = {
        "RolePlayName",
        "💢brave💢"
    }
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"🌟STAR🌟", function()
    local args = {
        "RolePlayName",
        "🌟STAR🌟"
    }
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"🥶FULL AURA🥶", function()
    local args = {
        "RolePlayName",
        "🥶FULL AURA🥶"
    }
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"🤖ROBOT🤖", function()
    local args = {
        "RolePlayName",
        "🤖ROBOT🤖"
    }
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"👁️ Extra Hard  Olho do Abismo", function()
    local args = {
        "RolePlayName",
        "👁️ Olho do Abismo"
    }
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"Fumando tranquilo 🍁", function(Value)
    local args = {"RolePlayName", "Fumando tranquilo 🍁"}
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"Hora da brisa 🍁", function(Value)
    local args = {"RolePlayName", "Hora da brisa 🍁"}
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"Só relax 🍁", function(Value)
    local args = {"RolePlayName", "Só relax 🍁"}
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"Brisa level máximo 🍁", function(Value)
    local args = {"RolePlayName", "Brisa level máximo 🍁"}
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"Sente a vibe 🍁", function(Value)
    local args = {"RolePlayName", "Sente a vibe 🍁"}
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"Paz e erva 🍁", function(Value)
    local args = {"RolePlayName", "Paz e erva 🍁"}
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"Tranquilidade 🍁", function(Value)
    local args = {"RolePlayName", "Tranquilidade 🍁"}
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"Relaxando no rolê 🍁", function(Value)
    local args = {"RolePlayName", "Relaxando no rolê 🍁"}
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"Cheiro de grama 🍁", function(Value)
    local args = {"RolePlayName", "Cheiro de grama 🍁"}
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"Brisa no ar 🍁", function(Value)
    local args = {"RolePlayName", "Brisa no ar 🍁"}
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"De boa na lagoa 🍁", function(Value)
    local args = {"RolePlayName", "De boa na lagoa 🍁"}
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"Só curtindo 🍁", function(Value)
    local args = {"RolePlayName", "Só curtindo 🍁"}
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"Boa vibe 🍁", function(Value)
    local args = {"RolePlayName", "Boa vibe 🍁"}
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"Chill 🍁", function(Value)
    local args = {"RolePlayName", "Chill 🍁"}
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"Vibe positiva 🍁", function(Value)
    local args = {"RolePlayName", "Vibe positiva 🍁"}
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"Só relaxar 🍁", function(Value)
    local args = {"RolePlayName", "Só relaxar 🍁"}
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"Nome: O Vazio 🌑", function(Value)
    local args = {"RolePlayName", "O Vazio 🌑"}
    game:GetService("ReplicatedStorage").RE["1RPNam1eTex1t"]:FireServer(unpack(args))
end})

Tab1:AddButton({"Nome: Sem Alma 💀", function(Value)
    local args = {"RolePlayName", "Sem Alma 💀"}
    game:GetService("ReplicatedStorage").RE["1RPNam1eTex1t"]:FireServer(unpack(args))
end})

Tab1:AddButton({"Nome: Sussurro Noturno 🌒", function(Value)
    local args = {"RolePlayName", "Sussurro Noturno 🌒"}
    game:GetService("ReplicatedStorage").RE["1RPNam1eTex1t"]:FireServer(unpack(args))
end})

Tab1:AddButton({"Nome: Eco Sombrio 👁️", function(Value)
    local args = {"RolePlayName", "Eco Sombrio 👁️"}
    game:GetService("ReplicatedStorage").RE["1RPNam1eTex1t"]:FireServer(unpack(args))
end})

Tab1:AddButton({"Nome: O Abismo 🩸", function(Value)
    local args = {"RolePlayName", "O Abismo 🩸"}
    game:GetService("ReplicatedStorage").RE["1RPNam1eTex1t"]:FireServer(unpack(args))
end})

Tab1:AddButton({"Nome: Fantasma Que Ri 👻", function(Value)
    local args = {"RolePlayName", "Fantasma Que Ri 👻"}
    game:GetService("ReplicatedStorage").RE["1RPNam1eTex1t"]:FireServer(unpack(args))
end})

Tab1:AddButton({"Nome: Medo Vivo 🔪", function(Value)
    local args = {"RolePlayName", "Medo Vivo 🔪"}
    game:GetService("ReplicatedStorage").RE["1RPNam1eTex1t"]:FireServer(unpack(args))
end})

Tab1:AddButton({"Nome: O Estranho 🌑", function(Value)
    local args = {"RolePlayName", "O Estranho 🌑"}
    game:GetService("ReplicatedStorage").RE["1RPNam1eTex1t"]:FireServer(unpack(args))
end})

Tab1:AddButton({"Nome: Filho da Escuridão 🕷️", function(Value)
    local args = {"RolePlayName", "Filho da Escuridão 🕷️"}
    game:GetService("ReplicatedStorage").RE["1RPNam1eTex1t"]:FireServer(unpack(args))
end})

Tab1:AddButton({"Nome: O Sem Rosto 👤", function(Value)
    local args = {"RolePlayName", "O Sem Rosto 👤"}
    game:GetService("ReplicatedStorage").RE["1RPNam1eTex1t"]:FireServer(unpack(args))
end})

Tab1:AddButton({"Nome: Som do Caixão ⚰️", function(Value)
    local args = {"RolePlayName", "Som do Caixão ⚰️"}
    game:GetService("ReplicatedStorage").RE["1RPNam1eTex1t"]:FireServer(unpack(args))
end})

Tab1:AddButton({"Nome: O Grito 🌑", function(Value)
    local args = {"RolePlayName", "O Grito 🌑"}
    game:GetService("ReplicatedStorage").RE["1RPNam1eTex1t"]:FireServer(unpack(args))
end})

Tab1:AddButton({"Nome: Dor Eterna ⛓️", function(Value)
    local args = {"RolePlayName", "Dor Eterna ⛓️"}
    game:GetService("ReplicatedStorage").RE["1RPNam1eTex1t"]:FireServer(unpack(args))
end})

Tab1:AddButton({"Nome: O Sussurrante 🩸", function(Value)
    local args = {"RolePlayName", "O Sussurrante 🩸"}
    game:GetService("ReplicatedStorage").RE["1RPNam1eTex1t"]:FireServer(unpack(args))
end})

Tab1:AddButton({"Nome: Noite Sangrenta 🌑", function(Value)
    local args = {"RolePlayName", "Noite Sangrenta 🌑"}
    game:GetService("ReplicatedStorage").RE["1RPNam1eTex1t"]:FireServer(unpack(args))
end})

Tab1:AddButton({"Nome: O Observador 👁️", function(Value)
    local args = {"RolePlayName", "O Observador 👁️"}
    game:GetService("ReplicatedStorage").RE["1RPNam1eTex1t"]:FireServer(unpack(args))
end})

Tab1:AddButton({"Nome: Carne Podre 🪓", function(Value)
    local args = {"RolePlayName", "Carne Podre 🪓"}
    game:GetService("ReplicatedStorage").RE["1RPNam1eTex1t"]:FireServer(unpack(args))
end})

Tab1:AddButton({"Nome: Ossos Quebrados 💀", function(Value)
    local args = {"RolePlayName", "Ossos Quebrados 💀"}
    game:GetService("ReplicatedStorage").RE["1RPNam1eTex1t"]:FireServer(unpack(args))
end})

Tab1:AddButton({"Nome: Silêncio Mortal 🌑", function(Value)
    local args = {"RolePlayName", "Silêncio Mortal 🌑"}
    game:GetService("ReplicatedStorage").RE["1RPNam1eTex1t"]:FireServer(unpack(args))
end})

Tab1:AddButton({"Nome: Sangue Frio 🩸", function(Value)
    local args = {"RolePlayName", "Sangue Frio 🩸"}
    game:GetService("ReplicatedStorage").RE["1RPNam1eTex1t"]:FireServer(unpack(args))
end})

Tab1:AddButton({"Nome: Ódio Vivo 🔪", function(Value)
    local args = {"RolePlayName", "Ódio Vivo 🔪"}
    game:GetService("ReplicatedStorage").RE["1RPNam1eTex1t"]:FireServer(unpack(args))
end})

Tab1:AddButton({"Nome: Vulto Enforcado ⚰️", function(Value)
    local args = {"RolePlayName", "Vulto Enforcado ⚰️"}
    game:GetService("ReplicatedStorage").RE["1RPNam1eTex1t"]:FireServer(unpack(args))
end})

Tab1:AddButton({"Nome: Lâmina da Noite 🌒", function(Value)
    local args = {"RolePlayName", "Lâmina da Noite 🌒"}
    game:GetService("ReplicatedStorage").RE["1RPNam1eTex1t"]:FireServer(unpack(args))
end})

Tab1:AddButton({"Nome: Necrose 🔥", function(Value)
    local args = {"RolePlayName", "Necrose 🔥"}
    game:GetService("ReplicatedStorage").RE["1RPNam1eTex1t"]:FireServer(unpack(args))
end})

Tab1:AddButton({"Nome: Dor Silenciosa 👁️", function(Value)
    local args = {"RolePlayName", "Dor Silenciosa 👁️"}
    game:GetService("ReplicatedStorage").RE["1RPNam1eTex1t"]:FireServer(unpack(args))
end})

Tab1:AddButton({"Nome: Alma Queimada 🕷️", function(Value)
    local args = {"RolePlayName", "Alma Queimada 🕷️"}
    game:GetService("ReplicatedStorage").RE["1RPNam1eTex1t"]:FireServer(unpack(args))
end})

Tab1:AddButton({"Nome: Mente Podre 🧠", function(Value)
    local args = {"RolePlayName", "Mente Podre 🧠"}
    game:GetService("ReplicatedStorage").RE["1RPNam1eTex1t"]:FireServer(unpack(args))
end})

Tab1:AddButton({"Nome: Morto em Pé 🪦", function(Value)
    local args = {"RolePlayName", "Morto em Pé 🪦"}
    game:GetService("ReplicatedStorage").RE["1RPNam1eTex1t"]:FireServer(unpack(args))
end})

Tab1:AddButton({"Nome: Órfão da Escuridão ⛓️", function(Value)
    local args = {"RolePlayName", "Órfão da Escuridão ⛓️"}
    game:GetService("ReplicatedStorage").RE["1RPNam1eTex1t"]:FireServer(unpack(args))
end})

Tab1:AddButton({"Nome: Carne Rasgada 🩻", function(Value)
    local args = {"RolePlayName", "Carne Rasgada 🩻"}
    game:GetService("ReplicatedStorage").RE["1RPNam1eTex1t"]:FireServer(unpack(args))
end})

Tab1:AddButton({"Nome: O Arrastado 🕯️", function(Value)
    local args = {"RolePlayName", "O Arrastado 🕯️"}
    game:GetService("ReplicatedStorage").RE["1RPNam1eTex1t"]:FireServer(unpack(args))
end})

Tab1:AddButton({"Nome: O Silencioso Sem Olhos 👤", function(Value)
    local args = {"RolePlayName", "O Silencioso Sem Olhos 👤"}
    game:GetService("ReplicatedStorage").RE["1RPNam1eTex1t"]:FireServer(unpack(args))
end})

local Section = Tab1:AddSection({"bios name🎭"})


Tab1:AddButton({"🎭 Hacker Sombrio", function(Value)
    print("Hello World!")
    local args = {
        "RolePlayBio",
        "🎭 Hacker Sombrio"
    }
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"🎭 Script Mestre", function(Value)
    print("Hello World!")
    local args = {
        "RolePlayBio",
        "🎭 Script Mestre"
    }
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"🎭 Phantom Hacker", function(Value)
    print("Hello World!")
    local args = {
        "RolePlayBio",
        "🎭 Phantom Hacker"
    }
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"🎭 Shadow Coder", function(Value)
    print("Hello World!")
    local args = {
        "RolePlayBio",
        "🎭 Shadow Coder"
    }
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"🎭 Cyber Ninja", function(Value)
    print("Hello World!")
    local args = {
        "RolePlayBio",
        "🎭 Cyber Ninja"
    }
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"🎭 Digital Phantom", function(Value)
    print("Hello World!")
    local args = {
        "RolePlayBio",
        "🎭 Digital Phantom"
    }
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"🎭 Dark Scriptor", function(Value)
    print("Hello World!")
    local args = {
        "RolePlayBio",
        "🎭 Dark Scriptor"
    }
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"🎭 Hackerman X", function(Value)
    print("Hello World!")
    local args = {
        "RolePlayBio",
        "🎭 Hackerman X"
    }
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"🎭 Virus Creator", function(Value)
    print("Hello World!")
    local args = {
        "RolePlayBio",
        "🎭 Virus Creator"
    }
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"🎭 Code Phantom", function(Value)
    print("Hello World!")
    local args = {
        "RolePlayBio",
        "🎭 Code Phantom"
    }
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"🎭 Shadow Byte", function(Value)
    print("Hello World!")
    local args = {
        "RolePlayBio",
        "🎭 Shadow Byte"
    }
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"🎭 Glitch Master", function(Value)
    print("Hello World!")
    local args = {
        "RolePlayBio",
        "🎭 Glitch Master"
    }
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"🎭 Encrypted Ghost", function(Value)
    print("Hello World!")
    local args = {
        "RolePlayBio",
        "🎭 Encrypted Ghost"
    }
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"🎭 Glitch Master", function(Value)
    print("Hello World!")
    local args = {
        "RolePlayBio",
        "🎭 Glitch Master"
    }
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"🎭 Encrypted Ghost", function(Value)
    print("Hello World!")
    local args = {
        "RolePlayBio",
        "🎭 Encrypted Ghost"
    }
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"💀SOULSNIPER", function()
    local args = {
        "RolePlayBio",
        "💀SOULSNIPER"
    }
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"🔥HELLFIRE", function()
    local args = {
        "RolePlayBio",
        "🔥HELLFIRE"
    }
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"👹NIGHTSTALKER", function()
    local args = {
        "RolePlayBio",
        "👹NIGHTSTALKER"
    }
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"🕷️WEBMASTER", function()
    local args = {
        "RolePlayBio",
        "🕷️WEBMASTER"
    }
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"💣EXPLOSION", function()
    local args = {
        "RolePlayBio",
        "💣EXPLOSION"
    }
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"🩸BLOODHUNTER", function()
    local args = {
        "RolePlayBio",
        "🩸BLOODHUNTER"
    }
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"⚡ELECTROSHOCK", function()
    local args = {
        "RolePlayBio",
        "⚡ELECTROSHOCK"
    }
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"🕶️SHADOWASSASSIN", function()
    local args = {
        "RolePlayBio",
        "🕶️SHADOWASSASSIN"
    }
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"👾CYBERPHANTOM", function()
    local args = {
        "RolePlayBio",
        "👾CYBERPHANTOM"
    }
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"🛡️IRONWRAITH", function()
    local args = {
        "RolePlayBio",
        "🛡️IRONWRAITH"
    }
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"🌑DARKVOID", function()
    local args = {
        "RolePlayBio",
        "🌑DARKVOID"
    }
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"🔥INFERNALBLADE", function()
    local args = {
        "RolePlayBio",
        "🔥INFERNALBLADE"
    }
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"🧛VAMPIRICFANG", function()
    local args = {
        "RolePlayBio",
        "🧛VAMPIRICFANG"
    }
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"💀SKULLCRUSHER", function()
    local args = {
        "RolePlayBio",
        "💀SKULLCRUSHER"
    }
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"👹HELLRAISER", function()
    local args = {
        "RolePlayBio",
        "👹HELLRAISER"
    }
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"🩸CRIMSONFURY", function()
    local args = {
        "RolePlayBio",
        "🩸CRIMSONFURY"
    }
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"⚡STORMBREAKER", function()
    local args = {
        "RolePlayBio",
        "⚡STORMBREAKER"
    }
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"🩸 Caçador de Almas", function()
    local args = {
        "RolePlayBio",
        "🩸 Minha fúria é o seu pesadelo."
    }
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"🔥 Filho do Inferno", function()
    local args = {
        "RolePlayBio",
        "🔥 Do caos eu nasci, no caos vou viver."
    }
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"🌑 Eclipse Sombrio", function()
    local args = {
        "RolePlayBio",
        "🌑 A luz se foi, só resta a escuridão."
    }
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"⚡ Tempestade Caótica", function()
    local args = {
        "RolePlayBio",
        "⚡ Relâmpagos cortam meu caminho."
    }
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"🕷️ Teia do Medo", function()
    local args = {
        "RolePlayBio",
        "🕷️ Preso em minha escuridão eterna."
    }
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"🩶 Coração Congelado", function()
    local args = {
        "RolePlayBio",
        "🩶 Amor? Morreu junto comigo."
    }
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"☠️ Sem Medo da Morte", function()
    local args = {
        "RolePlayBio",
        "☠️ Já estive lá, e voltei pior."
    }
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"💀 Alma do Caos", function()
    local args = {
        "RolePlayBio",
        "💀 Eu sou o caos que você teme."
    }
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"🩸 Sangue Frio", function()
    local args = {
        "RolePlayBio",
        "🩸 Meu toque congela a alma."
    }
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"🔥 Inferno Vivo", function()
    local args = {
        "RolePlayBio",
        "🔥 Queimo tudo ao meu redor."
    }
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"🌑 Sombra Eterna", function()
    local args = {
        "RolePlayBio",
        "🌑 Ninguém escapa da minha escuridão."
    }
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"⚡ Fúria Relâmpago", function()
    local args = {
        "RolePlayBio",
        "⚡ Cada passo meu é destruição."
    }
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"🕷️ Teia Mortal", function()
    local args = {
        "RolePlayBio",
        "🕷️ Você já está preso no meu medo."
    }
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"☠️ Mestre da Morte", function()
    local args = {
        "RolePlayBio",
        "☠️ Eu já vi tudo… e sobrevivi."
    }
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"🩶 Alma Congelada", function()
    local args = {
        "RolePlayBio",
        "🩶 O amor morreu comigo."
    }
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"💀 Pesadelo Vivo", function()
    local args = {
        "RolePlayBio",
        "💀 Eu moro nos seus piores sonhos."
    }
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"🔥 Caos Ardente", function()
    local args = {
        "RolePlayBio",
        "🔥 Tudo que eu toco vira chamas."
    }
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"🌑 Escuridão Absoluta", function()
    local args = {
        "RolePlayBio",
        "🌑 Não há luz no meu caminho."
    }
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"🩸 Cortante", function()
    local args = {
        "RolePlayBio",
        "🩸 Meu olhar corta como faca."
    }
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"⚡ Tempestade Negra", function()
    local args = {
        "RolePlayBio",
        "⚡ O mundo treme quando passo."
    }
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"🕷️ Aranha Sombria", function()
    local args = {
        "RolePlayBio",
        "🕷️ Minhas teias não têm fim."
    }
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"💀 Guardião da Ruína", function()
    local args = {
        "RolePlayBio",
        "💀 Protejo o que destruo."
    }
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"🔥 Labareda Sombria", function()
    local args = {
        "RolePlayBio",
        "🔥 Chamas frias consomem tudo."
    }
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"🌑 Olhos da Escuridão", function()
    local args = {
        "RolePlayBio",
        "🌑 Meus olhos veem a alma."
    }
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"🩶 Gelo Mortal", function()
    local args = {
        "RolePlayBio",
        "🩶 Cada toque meu congela o coração."
    }
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"☠️ Legado Sombrio", function()
    local args = {
        "RolePlayBio",
        "☠️ Minha história é medo e dor."
    }
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"🩸 Sangue Fantasma", function()
    local args = {
        "RolePlayBio",
        "🩸 Meu passado sangra no presente."
    }
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"Bio: Fumaça é vida 🍁", function(Value)
    local args = {"RolePlayBio", "Fumaça é vida 🍁"}
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"Bio: Perdido na brisa 🍁", function(Value)
    local args = {"RolePlayBio", "Perdido na brisa 🍁"}
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"Bio: Só na paz 🍁", function(Value)
    local args = {"RolePlayBio", "Só na paz 🍁"}
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"Bio: Brisando no rolê 🍁", function(Value)
    local args = {"RolePlayBio", "Brisando no rolê 🍁"}
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"Bio: Vibe natural 🍁", function(Value)
    local args = {"RolePlayBio", "Vibe natural 🍁"}
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"Bio: Paz e fumaça 🍁", function(Value)
    local args = {"RolePlayBio", "Paz e fumaça 🍁"}
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"Bio: Mundo colorido 🍁", function(Value)
    local args = {"RolePlayBio", "Mundo colorido 🍁"}
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"Bio: Flutuando na nuvem 🍁", function(Value)
    local args = {"RolePlayBio", "Flutuando na nuvem 🍁"}
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"Bio: Brisa infinita 🍁", function(Value)
    local args = {"RolePlayBio", "Brisa infinita 🍁"}
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"Bio: Luz verde na mente 🍁", function(Value)
    local args = {"RolePlayBio", "Luz verde na mente 🍁"}
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"Bio: Só vibração boa 🍁", function(Value)
    local args = {"RolePlayBio", "Só vibração boa 🍁"}
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"Bio: Energia verde 🍁", function(Value)
    local args = {"RolePlayBio", "Energia verde 🍁"}
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"Bio: Rolê psicodélico 🍁", function(Value)
    local args = {"RolePlayBio", "Rolê psicodélico 🍁"}
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"Bio: Verde é paz 🍁", function(Value)
    local args = {"RolePlayBio", "Verde é paz 🍁"}
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"Bio: Alma leve 🍁", function(Value)
    local args = {"RolePlayBio", "Alma leve 🍁"}
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"Bio: Vida no flow 🍁", function(Value)
    local args = {"RolePlayBio", "Vida no flow 🍁"}
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eTex1t"):FireServer(unpack(args))
end})

Tab1:AddButton({"Bio: Eu vejo você no escuro 👁️", function(Value)
    local args = {"RolePlayBio", "Eu vejo você no escuro 👁️"}
    game:GetService("ReplicatedStorage").RE["1RPNam1eTex1t"]:FireServer(unpack(args))
end})

Tab1:AddButton({"Bio: Vozes me chamam 🌑", function(Value)
    local args = {"RolePlayBio", "Vozes me chamam 🌑"}
    game:GetService("ReplicatedStorage").RE["1RPNam1eTex1t"]:FireServer(unpack(args))
end})

Tab1:AddButton({"Bio: O silêncio grita 🔪", function(Value)
    local args = {"RolePlayBio", "O silêncio grita 🔪"}
    game:GetService("ReplicatedStorage").RE["1RPNam1eTex1t"]:FireServer(unpack(args))
end})

Tab1:AddButton({"Bio: O medo é meu brinquedo 🕷️", function(Value)
    local args = {"RolePlayBio", "O medo é meu brinquedo 🕷️"}
    game:GetService("ReplicatedStorage").RE["1RPNam1eTex1t"]:FireServer(unpack(args))
end})

Tab1:AddButton({"Bio: Olhos sem alma 👻", function(Value)
    local args = {"RolePlayBio", "Olhos sem alma 👻"}
    game:GetService("ReplicatedStorage").RE["1RPNam1eTex1t"]:FireServer(unpack(args))
end})

Tab1:AddButton({"Bio: Minha sombra sussurra 🌒", function(Value)
    local args = {"RolePlayBio", "Minha sombra sussurra 🌒"}
    game:GetService("ReplicatedStorage").RE["1RPNam1eTex1t"]:FireServer(unpack(args))
end})

Tab1:AddButton({"Bio: Sorriso quebrado 🩸", function(Value)
    local args = {"RolePlayBio", "Sorriso quebrado 🩸"}
    game:GetService("ReplicatedStorage").RE["1RPNam1eTex1t"]:FireServer(unpack(args))
end})

Tab1:AddButton({"Bio: Preso na escuridão ⛓️", function(Value)
    local args = {"RolePlayBio", "Preso na escuridão ⛓️"}
    game:GetService("ReplicatedStorage").RE["1RPNam1eTex1t"]:FireServer(unpack(args))
end})

Tab1:AddButton({"Bio: Onde a luz não entra 🌑", function(Value)
    local args = {"RolePlayBio", "Onde a luz não entra 🌑"}
    game:GetService("ReplicatedStorage").RE["1RPNam1eTex1t"]:FireServer(unpack(args))
end})

Tab1:AddButton({"Bio: Coração de gelo ❄️", function(Value)
    local args = {"RolePlayBio", "Coração de gelo ❄️"}
    game:GetService("ReplicatedStorage").RE["1RPNam1eTex1t"]:FireServer(unpack(args))
end})

Tab1:AddButton({"Bio: Nada é eterno ⚰️", function(Value)
    local args = {"RolePlayBio", "Nada é eterno ⚰️"}
    game:GetService("ReplicatedStorage").RE["1RPNam1eTex1t"]:FireServer(unpack(args))
end})

Tab1:AddButton({"Bio: Escuto passos atrás de mim 👂", function(Value)
    local args = {"RolePlayBio", "Escuto passos atrás de mim 👂"}
    game:GetService("ReplicatedStorage").RE["1RPNam1eTex1t"]:FireServer(unpack(args))
end})

Tab1:AddButton({"Bio: Sou o seu pesadelo 🌑", function(Value)
    local args = {"RolePlayBio", "Sou o seu pesadelo 🌑"}
    game:GetService("ReplicatedStorage").RE["1RPNam1eTex1t"]:FireServer(unpack(args))
end})

Tab1:AddButton({"Bio: Seus olhos tremem quando me veem 🩸", function(Value)
    local args = {"RolePlayBio", "Seus olhos tremem quando me veem 🩸"}
    game:GetService("ReplicatedStorage").RE["1RPNam1eTex1t"]:FireServer(unpack(args))
end})

Tab1:AddButton({"Bio: O abismo sorri de volta 👁️", function(Value)
    local args = {"RolePlayBio", "O abismo sorri de volta 👁️"}
    game:GetService("ReplicatedStorage").RE["1RPNam1eTex1t"]:FireServer(unpack(args))
end})

Tab1:AddButton({"Bio: Gosto do som do medo 💀", function(Value)
    local args = {"RolePlayBio", "Gosto do som do medo 💀"}
    game:GetService("ReplicatedStorage").RE["1RPNam1eTex1t"]:FireServer(unpack(args))
end})

local Tab1 = Window:MakeTab({"rgb🏳️‍🌈", "paint"})

local ReplicatedStorage = game:GetService("ReplicatedStorage")
local Remote = ReplicatedStorage:WaitForChild("RE"):WaitForChild("1RPNam1eColo1r")

local running = false -- controle do loop

-- Função que começa a trocar as cores
local function StartRGB()
    task.spawn(function()
        while running do
            -- gera cor RGB cíclica
            local color = Color3.fromHSV(tick() % 5 / 5, 1, 1)
            
            local args = {
                "PickingRPNameColor",
                color
            }
            Remote:FireServer(unpack(args))
            
            task.wait(0.2) -- velocidade da troca (quanto menor, mais rápido)
        end
    end)
end


-- Toggle
Tab1:AddToggle({
    Name = "LGBT name🏳️‍🌈",
    Default = false,
    Callback = function(v)
        running = v
        if v then
            StartRGB()
            print("RGB ativado, Velocidade:", speed)
        else
            print("RGB desativado")
        end
    end
})

-- Slider de Velocidade
Tab1:AddSlider({
    Name = "velocidade do rgb",
    Min = 1,
    Max = 100,
    Increase = 1,
    Default = 16,
    Callback = function(Value)
        speed = Value
        print("Velocidade ajustada para:", speed)
    end
})

local ReplicatedStorage = game:GetService("ReplicatedStorage")
local Remote = ReplicatedStorage:WaitForChild("RE"):WaitForChild("1RPNam1eColo1r")

local runningBio = false
local bioSpeed = 16 -- valor inicial do slider

-- Função que inicia o efeito RGB na Bio
local function StartBioRGB()
    task.spawn(function()
        while runningBio do
            local color = Color3.fromHSV(tick() % 5 / 5, 1, 1)

            local args = {
                "PickingRPBioColor",
                color
            }
            Remote:FireServer(unpack(args))

            task.wait(1 / bioSpeed) -- controlado pelo slider
        end
    end)
end

-- Toggle
Tab1:AddToggle({
    Name = "LGBT bio🏳️‍🌈",
    Default = false,
    Callback = function(v)
        runningBio = v
        if v then
            StartBioRGB()
            print("RGB da Bio ativado, Velocidade:", bioSpeed)
        else
            print("RGB da Bio desativado")
        end
    end
})

-- Slider de Velocidade
Tab1:AddSlider({
    Name = "velocidade do rgb",
    Min = 1,
    Max = 100,
    Increase = 1,
    Default = 16,
    Callback = function(Value)
        bioSpeed = Value
        print("Velocidade da Bio ajustada para:", bioSpeed)
    end
})

local Section = Tab1:AddSection({"loop names"})

local ReplicatedStorage = game:GetService("ReplicatedStorage")
local Remote = ReplicatedStorage:WaitForChild("RE"):WaitForChild("1RPNam1eTex1t")

local running = false
local name1 = "PrimeiroNome"
local name2 = "SegundoNome"
local speed = 1 -- tempo em segundos (vai ser ajustado pelo slider)

-- TextBox para primeiro nome
Tab1:AddTextBox({
    Name = "Primeiro Nome",
    Description = "Digite o primeiro nome",
    PlaceholderText = "Ex: I AM",
    Callback = function(Value)
        name1 = Value
        print("Primeiro nome definido como:", name1)
    end
})

-- TextBox para segundo nome
Tab1:AddTextBox({
    Name = "Segundo Nome",
    Description = "Digite o segundo nome",
    PlaceholderText = "Ex: HACKER!☠️",
    Callback = function(Value)
        name2 = Value
        print("Segundo nome definido como:", name2)
    end
})

-- Slider para controlar velocidade
Tab1:AddSlider({
    Name = "Velocidade do Loop",
    Min = 0.1,
    Max = 5,
    Increase = 0.1,
    Default = 1,
    Callback = function(Value)
        speed = Value
        print("Velocidade do loop definida para:", speed)
    end
})

-- Função que faz o loop
local function StartNameLoop()
    task.spawn(function()
        while running do
            -- Primeiro nome
            Remote:FireServer("RolePlayName", name1)
            task.wait(speed)

            if not running then break end

            -- Segundo nome
            Remote:FireServer("RolePlayName", name2)
            task.wait(speed)
        end
    end)
end

-- Toggle
Tab1:AddToggle({
    Name = "Loop Nomes",
    Default = false,
    Callback = function(v)
        running = v
        if v then
            StartNameLoop()
            print("Loop de nomes iniciado")
        else
            print("Loop de nomes parado")
        end
    end
})

local ReplicatedStorage = game:GetService("ReplicatedStorage")
local Remote = ReplicatedStorage:WaitForChild("RE"):WaitForChild("1RPNam1eTex1t")

local blinkRunning = false
local blinkName = "NomeBlink" -- pode usar o mesmo TextBox do loop ou criar outro

-- TextBox para o nome do blink
Tab1:AddTextBox({
    Name = "Nome Pisca-Pisca",
    Description = "Digite o nome para piscar",
    PlaceholderText = "Ex: I AM",
    Callback = function(Value)
        blinkName = Value
        print("Nome do pisca-pisca definido como:", blinkName)
    end
})

-- Toggle do pisca-pisca
Tab1:AddToggle({
    Name = "Pisca-Pisca",
    Default = false,
    Callback = function(v)
        blinkRunning = v
        if v then
            task.spawn(function()
                while blinkRunning do
                    -- Aparece
                    Remote:FireServer("RolePlayName", blinkName)
                    task.wait(0.1) -- tempo que fica visível

                    -- Desaparece
                    Remote:FireServer("RolePlayName", "")
                    task.wait(0.1) -- tempo que fica invisível

                    if not blinkRunning then break end
                end
            end)
            print("Pisca-pisca ativado")
        else
            print("Pisca-pisca desativado")
            -- Limpa o nome quando desativa
            Remote:FireServer("RolePlayName", "")
        end
    end
})

local ReplicatedStorage = game:GetService("ReplicatedStorage")
local Remote = ReplicatedStorage:WaitForChild("RE"):WaitForChild("1RPNam1eColo1r")

local blinkRunningBio = false
local blinkNameBio = "BioPisca" -- nome que vai piscar

-- TextBox para o nome da bio pisca-pisca
Tab1:AddTextBox({
    Name = "Bio Pisca-Pisca",
    Description = "Digite o nome da bio para piscar",
    PlaceholderText = "Ex: BIO",
    Callback = function(Value)
        blinkNameBio = Value
        print("Nome da bio pisca-pisca definido como:", blinkNameBio)
    end
})

-- Toggle do pisca-pisca da bio
Tab1:AddToggle({
    Name = "Pisca-Pisca Bio",
    Default = false,
    Callback = function(v)
        blinkRunningBio = v
        if v then
            task.spawn(function()
                while blinkRunningBio do
                    -- Aparece
                    Remote:FireServer("PickingRPBioColor", Color3.fromRGB(255,255,255)) -- cor branca pra aparecer
                    task.wait(0.1)

                    -- Desaparece
                    Remote:FireServer("PickingRPBioColor", Color3.fromRGB(0,0,0)) -- cor preta pra sumir
                    task.wait(0.1)

                    if not blinkRunningBio then break end
                end
            end)
            print("Pisca-pisca da Bio ativado")
        else
            print("Pisca-pisca da Bio desativado")
            -- Limpa a bio quando desativa
            Remote:FireServer("PickingRPBioColor", Color3.fromRGB(0,0,0))
        end
    end
})

local Section = Tab1:AddSection({"combinações ⚫🔴🔵🟢🟠🟣/nomes"})

-- Função para suavizar transição de cores
local function LerpColor(c1, c2, t)
    return Color3.new(
        c1.R + (c2.R - c1.R) * t,
        c1.G + (c2.G - c1.G) * t,
        c1.B + (c2.B - c1.B) * t
    )
end

-- Função para iniciar loop
local function StartLoop(colors, speed)
    local RunService = game:GetService("RunService")
    local Players = game:GetService("Players")
    local LocalPlayer = Players.LocalPlayer
    local guiPath = game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eColo1r")

    local active = true
    task.spawn(function()
        while active do
            for i = 1, #colors do
                local c1 = colors[i]
                local c2 = colors[(i % #colors) + 1]

                for t = 0, 1, speed do
                    if not active then return end
                    local args = { "PickingRPNameColor", LerpColor(c1, c2, t) }
                    guiPath:FireServer(unpack(args))
                    task.wait(0.05)
                end
            end
        end
    end)
    return function() active = false end
end

-- Guardar funções de stop
local stopLoops = {}

-- 🔴⚫ Vermelho e Preto
Tab1:AddToggle({
    Name = "Vermelho e Preto 🔴⚫",
    Default = false,
    Callback = function(v)
        if v then
            stopLoops["redblack"] = StartLoop({
                Color3.fromRGB(255, 0, 0), -- vermelho
                Color3.fromRGB(0, 0, 0)    -- preto
            }, 0.03)
        else
            if stopLoops["redblack"] then stopLoops["redblack"]() end
        end
    end
})

-- 🟡⚫ Amarelo e Preto
Tab1:AddToggle({
    Name = "Amarelo e Preto 🟡⚫",
    Default = false,
    Callback = function(v)
        if v then
            stopLoops["yellowblack"] = StartLoop({
                Color3.fromRGB(255,255,0),
                Color3.fromRGB(0,0,0)
            }, 0.03)
        else
            if stopLoops["yellowblack"] then stopLoops["yellowblack"]() end
        end
    end
})

-- 🔵🟢 Azul e Verde
Tab1:AddToggle({
    Name = "Azul e Verde 🔵🟢",
    Default = false,
    Callback = function(v)
        if v then
            stopLoops["bluegreen"] = StartLoop({
                Color3.fromRGB(0,0,255),
                Color3.fromRGB(0,255,0)
            }, 0.03)
        else
            if stopLoops["bluegreen"] then stopLoops["bluegreen"]() end
        end
    end
})

-- 🟣⚪ Roxo e Branco
Tab1:AddToggle({
    Name = "Roxo e Branco 🟣⚪",
    Default = false,
    Callback = function(v)
        if v then
            stopLoops["purplewhite"] = StartLoop({
                Color3.fromRGB(128,0,128),
                Color3.fromRGB(255,255,255)
            }, 0.03)
        else
            if stopLoops["purplewhite"] then stopLoops["purplewhite"]() end
        end
    end
})

-- 🟠🔵 Laranja e Azul Neon
Tab1:AddToggle({
    Name = "Laranja e Azul 🔶🔵",
    Default = false,
    Callback = function(v)
        if v then
            stopLoops["orangeblue"] = StartLoop({
                Color3.fromRGB(255,140,0),
                Color3.fromRGB(0, 100, 255)
            }, 0.03)
        else
            if stopLoops["orangeblue"] then stopLoops["orangeblue"]() end
        end
    end
})

-- ❄⚪ Ciano e Branco
Tab1:AddToggle({
    Name = "Ciano e Branco ❄⚪",
    Default = false,
    Callback = function(v)
        if v then
            stopLoops["cyanwhite"] = StartLoop({
                Color3.fromRGB(0,255,255),
                Color3.fromRGB(255,255,255)
            }, 0.03)
        else
            if stopLoops["cyanwhite"] then stopLoops["cyanwhite"]() end
        end
    end
})

-- 🔴🟠🟡 Vermelho, Laranja e Amarelo 🔥
Tab1:AddToggle({
    Name = "Vermelho Laranja Amarelo 🔥",
    Default = false,
    Callback = function(v)
        if v then
            stopLoops["roryellow"] = StartLoop({
                Color3.fromRGB(255,0,0),
                Color3.fromRGB(255,140,0),
                Color3.fromRGB(255,255,0)
            }, 0.03)
        else
            if stopLoops["roryellow"] then stopLoops["roryellow"]() end
        end
    end
})

-- 🟢🔵🟣 Verde Azul Roxo 🌌
Tab1:AddToggle({
    Name = "Verde Azul Roxo 🌌",
    Default = false,
    Callback = function(v)
        if v then
            stopLoops["greenbluepurple"] = StartLoop({
                Color3.fromRGB(0,255,0),
                Color3.fromRGB(0,0,255),
                Color3.fromRGB(128,0,128)
            }, 0.03)
        else
            if stopLoops["greenbluepurple"] then stopLoops["greenbluepurple"]() end
        end
    end
})

local Section = Tab1:AddSection({"Bio"})

-- Função para suavizar transição de cores
local function LerpColor(c1, c2, t)
    return Color3.new(
        c1.R + (c2.R - c1.R) * t,
        c1.G + (c2.G - c1.G) * t,
        c1.B + (c2.B - c1.B) * t
    )
end

-- Função para iniciar loop
local function StartLoop(colors, speed)
    local RunService = game:GetService("RunService")
    local guiPath = game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPNam1eColo1r")

    local active = true
    task.spawn(function()
        while active do
            for i = 1, #colors do
                local c1 = colors[i]
                local c2 = colors[(i % #colors) + 1]

                for t = 0, 1, speed do
                    if not active then return end
                    local args = { "PickingRPBioColor", LerpColor(c1, c2, t) }
                    guiPath:FireServer(unpack(args))
                    task.wait(0.05)
                end
            end
        end
    end)
    return function() active = false end
end

-- Guardar funções de stop
local stopLoops = {}

-- 🔴⚫ Vermelho e Preto
Tab1:AddToggle({
    Name = "Bio Vermelho e Preto 🔴⚫",
    Default = false,
    Callback = function(v)
        if v then
            stopLoops["redblackbio"] = StartLoop({
                Color3.fromRGB(255, 0, 0),
                Color3.fromRGB(0, 0, 0)
            }, 0.03)
        else
            if stopLoops["redblackbio"] then stopLoops["redblackbio"]() end
        end
    end
})

-- 🟡⚫ Amarelo e Preto
Tab1:AddToggle({
    Name = "Bio Amarelo e Preto 🟡⚫",
    Default = false,
    Callback = function(v)
        if v then
            stopLoops["yellowblackbio"] = StartLoop({
                Color3.fromRGB(255,255,0),
                Color3.fromRGB(0,0,0)
            }, 0.03)
        else
            if stopLoops["yellowblackbio"] then stopLoops["yellowblackbio"]() end
        end
    end
})

-- 🔵🟢 Azul e Verde
Tab1:AddToggle({
    Name = "Bio Azul e Verde 🔵🟢",
    Default = false,
    Callback = function(v)
        if v then
            stopLoops["bluegreenbio"] = StartLoop({
                Color3.fromRGB(0,0,255),
                Color3.fromRGB(0,255,0)
            }, 0.03)
        else
            if stopLoops["bluegreenbio"] then stopLoops["bluegreenbio"]() end
        end
    end
})

-- 🟣⚪ Roxo e Branco
Tab1:AddToggle({
    Name = "Bio Roxo e Branco 🟣⚪",
    Default = false,
    Callback = function(v)
        if v then
            stopLoops["purplewhitebio"] = StartLoop({
                Color3.fromRGB(128,0,128),
                Color3.fromRGB(255,255,255)
            }, 0.03)
        else
            if stopLoops["purplewhitebio"] then stopLoops["purplewhitebio"]() end
        end
    end
})

-- 🟠🔵 Laranja e Azul Neon
Tab1:AddToggle({
    Name = "Bio Laranja e Azul 🔶🔵",
    Default = false,
    Callback = function(v)
        if v then
            stopLoops["orangebluebio"] = StartLoop({
                Color3.fromRGB(255,140,0),
                Color3.fromRGB(0, 100, 255)
            }, 0.03)
        else
            if stopLoops["orangebluebio"] then stopLoops["orangebluebio"]() end
        end
    end
})

-- ❄⚪ Ciano e Branco
Tab1:AddToggle({
    Name = "Bio Ciano e Branco ❄⚪",
    Default = false,
    Callback = function(v)
        if v then
            stopLoops["cyanwhitebio"] = StartLoop({
                Color3.fromRGB(0,255,255),
                Color3.fromRGB(255,255,255)
            }, 0.03)
        else
            if stopLoops["cyanwhitebio"] then stopLoops["cyanwhitebio"]() end
        end
    end
})

-- 🔥 Vermelho, Laranja e Amarelo
Tab1:AddToggle({
    Name = "Bio Vermelho Laranja Amarelo 🔥",
    Default = false,
    Callback = function(v)
        if v then
            stopLoops["roryellowbio"] = StartLoop({
                Color3.fromRGB(255,0,0),
                Color3.fromRGB(255,140,0),
                Color3.fromRGB(255,255,0)
            }, 0.03)
        else
            if stopLoops["roryellowbio"] then stopLoops["roryellowbio"]() end
        end
    end
})

-- 🌌 Verde Azul Roxo
Tab1:AddToggle({
    Name = "Bio Verde Azul Roxo 🌌",
    Default = false,
    Callback = function(v)
        if v then
            stopLoops["greenbluepurplebio"] = StartLoop({
                Color3.fromRGB(0,255,0),
                Color3.fromRGB(0,0,255),
                Color3.fromRGB(128,0,128)
            }, 0.03)
        else
            if stopLoops["greenbluepurplebio"] then stopLoops["greenbluepurplebio"]() end
        end
    end
})

local Tab1 = Window:MakeTab({"Avatar", "person"})


Tab1:AddToggle({
    Name = "Loop BodySize ⚡",
    Default = false,
    Callback = function(v)
        if v then
            -- Ativa o loop
            getgenv().LoopSize = true
            task.spawn(function()
                while getgenv().LoopSize do
                    -- Aumenta
                    local args = {[1] = true}
                    game:GetService("ReplicatedStorage").Remotes.IncrementBodySize:FireServer(unpack(args))
                    task.wait(0.3)

                    -- Diminui
                    args = {[1] = false}
                    game:GetService("ReplicatedStorage").Remotes.IncrementBodySize:FireServer(unpack(args))
                    task.wait(0.3)
                end
            end)
        else
            -- Desativa o loop
            getgenv().LoopSize = false
        end
    end
})

Tab1:AddToggle({
    Name = "Loop Rostos 👹",
    Default = false,
    Callback = function(v)
        if v then
            getgenv().LoopFaces = true
            task.spawn(function()
                local faceIDs = {
                    6093230191,     -- Rosto Assustador
                    86487700,       -- Winning Smile
                    7074722050,     -- Rosto Errado
                    7074783902,     -- Maníaco
                    7074946442,     -- Medo
                }
                while getgenv().LoopFaces do
                    for _, id in ipairs(faceIDs) do
                        if not getgenv().LoopFaces then break end
                        local args = {[1] = id}
                        game:GetService("ReplicatedStorage").Remotes.Wear:InvokeServer(unpack(args))
                        task.wait(0.1) -- tempo entre trocas
                    end
                end
            end)
        else
            getgenv().LoopFaces = false
        end
    end
})

local Section = Tab1:AddSection({"memes🤣"})

-- Fantasia de Meme Sus Amogus Vermelho
Tab1:AddButton({"Sus Amogus Vermelho", function(Value)
    local args = {[1] = 17341251089}
    game:GetService("ReplicatedStorage").Remotes.Wear:InvokeServer(unpack(args))
end})

-- Fantasia de Meme Sus Amogus Laranja
Tab1:AddButton({"Sus Amogus Laranja", function(Value)
    local args = {[1] = 17493403586}
    game:GetService("ReplicatedStorage").Remotes.Wear:InvokeServer(unpack(args))
end})

-- Fantasia de Meme Sus Amogus Branco
Tab1:AddButton({"Sus Amogus Branco", function(Value)
    local args = {[1] = 17493115174}
    game:GetService("ReplicatedStorage").Remotes.Wear:InvokeServer(unpack(args))
end})

-- Terno de Aranha Meme
Tab1:AddButton({"Terno de Aranha Meme", function(Value)
    local args = {[1] = 15812491823}
    game:GetService("ReplicatedStorage").Remotes.Wear:InvokeServer(unpack(args))
end})

-- Fantasia de Meme de Gato Triste
Tab1:AddButton({"Gato Triste", function(Value)
    local args = {[1] = 17341170915}
    game:GetService("ReplicatedStorage").Remotes.Wear:InvokeServer(unpack(args))
end})

-- Hamster Costume Meme
Tab1:AddButton({"Hamster Costume", function(Value)
    local args = {[1] = 16763324624}
    game:GetService("ReplicatedStorage").Remotes.Wear:InvokeServer(unpack(args))
end})

-- Fantasia de Emoji Sorriso
Tab1:AddButton({"Emoji Sorriso", function(Value)
    local args = {[1] = 18663365690}
    game:GetService("ReplicatedStorage").Remotes.Wear:InvokeServer(unpack(args))
end})

-- Terno de Sapo
Tab1:AddButton({"Terno de Sapo", function(Value)
    local args = {[1] = 15837377441}
    game:GetService("ReplicatedStorage").Remotes.Wear:InvokeServer(unpack(args))
end})

-- Terno de Tubarão Gordo
Tab1:AddButton({"Tubarão Gordo", function(Value)
    local args = {[1] = 99459753608381}
    game:GetService("ReplicatedStorage").Remotes.Wear:InvokeServer(unpack(args))
end})

-- Ride Chicken Jockey Calças Negras
Tab1:AddButton({"Ride Chicken Calças Negras", function(Value)
    local args = {[1] = 128043402349480}
    game:GetService("ReplicatedStorage").Remotes.Wear:InvokeServer(unpack(args))
end})

-- Traje de Rosto Amaldiçoado
Tab1:AddButton({"Rosto Amaldiçoado", function(Value)
    local args = {[1] = 17649171615}
    game:GetService("ReplicatedStorage").Remotes.Wear:InvokeServer(unpack(args))
end})

-- Terno de Gato Feliz
Tab1:AddButton({"Gato Feliz", function(Value)
    local args = {[1] = 16123138655}
    game:GetService("ReplicatedStorage").Remotes.Wear:InvokeServer(unpack(args))
end})

-- Gato de Abacaxi
Tab1:AddButton({"Gato de Abacaxi", function(Value)
    local args = {[1] = 15717268215}
    game:GetService("ReplicatedStorage").Remotes.Wear:InvokeServer(unpack(args))
end})

-- Gato Musical
Tab1:AddButton({"Gato Musical", function(Value)
    local args = {[1] = 17725261071}
    game:GetService("ReplicatedStorage").Remotes.Wear:InvokeServer(unpack(args))
end})

-- Maior Terno Goofy Nugget
Tab1:AddButton({"Maior Terno Goofy Nugget", function(Value)
    local args = {[1] = 17528311010}
    game:GetService("ReplicatedStorage").Remotes.Wear:InvokeServer(unpack(args))
end})

-- Fantasia de Gato Gordo
Tab1:AddButton({"Fantasia de Gato Gordo", function(Value)
    local args = {[1] = 17330985717}
    game:GetService("ReplicatedStorage").Remotes.Wear:InvokeServer(unpack(args))
end})

-- Terno de Gato
Tab1:AddButton({"Terno de Gato", function(Value)
    local args = {[1] = 17120521318}
    game:GetService("ReplicatedStorage").Remotes.Wear:InvokeServer(unpack(args))
end})

-- Roupa de Rato
Tab1:AddButton({"Roupa de Rato", function(Value)
    local args = {[1] = 17209877665}
    game:GetService("ReplicatedStorage").Remotes.Wear:InvokeServer(unpack(args))
end})

Tab1:AddButton({"Terno de Cenoura", function(Value)
    local args = {[1] = 18448930299}
    game:GetService("ReplicatedStorage").Remotes.Wear:InvokeServer(unpack(args))
end})

Tab1:AddButton({"Galinha Irritada", function(Value)
    local args = {[1] = 18528798823}
    game:GetService("ReplicatedStorage").Remotes.Wear:InvokeServer(unpack(args))
end})

Tab1:AddButton({"Terno de Urso de Cabeça Pequena", function(Value)
    local args = {[1] = 17653242839}
    game:GetService("ReplicatedStorage").Remotes.Wear:InvokeServer(unpack(args))
end})

Tab1:AddButton({"Terno de Abacate", function(Value)
    local args = {[1] = 18342503144}
    game:GetService("ReplicatedStorage").Remotes.Wear:InvokeServer(unpack(args))
end})

Tab1:AddButton({"Terno de Melancia", function(Value)
    local args = {[1] = 18342514960}
    game:GetService("ReplicatedStorage").Remotes.Wear:InvokeServer(unpack(args))
end})

Tab1:AddButton({"Terno de Peixe Sacabambaspis", function(Value)
    local args = {[1] = 17653237846}
    game:GetService("ReplicatedStorage").Remotes.Wear:InvokeServer(unpack(args))
end})

Tab1:AddButton({"Meme de Cara de Homem de Galinha", function(Value)
    local args = {[1] = 17042889814}
    game:GetService("ReplicatedStorage").Remotes.Wear:InvokeServer(unpack(args))
end})

Tab1:AddButton({"Chicken Nugget Man Face Suit Meme", function(Value)
    local args = {[1] = 18273131224}
    game:GetService("ReplicatedStorage").Remotes.Wear:InvokeServer(unpack(args))
end})

Tab1:AddButton({"Terno de Meme de Brian", function(Value)
    local args = {[1] = 109559904265560}
    game:GetService("ReplicatedStorage").Remotes.Wear:InvokeServer(unpack(args))
end})

Tab1:AddButton({"Fantasia de Meme do Gato", function(Value)
    local args = {[1] = 18821161072}
    game:GetService("ReplicatedStorage").Remotes.Wear:InvokeServer(unpack(args))
end})

Tab1:AddButton({"Meme de Doge Amaldiçoado", function(Value)
    local args = {[1] = 91028784187490}
    game:GetService("ReplicatedStorage").Remotes.Wear:InvokeServer(unpack(args))
end})

Tab1:AddButton({"Terno de Sapo de Cheems", function(Value)
    local args = {[1] = 111803766775961}
    game:GetService("ReplicatedStorage").Remotes.Wear:InvokeServer(unpack(args))
end})

Tab1:AddButton({"Fato de Pop Cat Meme", function(Value)
    local args = {[1] = 135295997484785}
    game:GetService("ReplicatedStorage").Remotes.Wear:InvokeServer(unpack(args))
end})

Tab1:AddButton({"Terno de Bart Meme", function(Value)
    local args = {[1] = 18515649219}
    game:GetService("ReplicatedStorage").Remotes.Wear:InvokeServer(unpack(args))
end})

Tab1:AddButton({"Fantasia de Meme de Urso de Mel", function(Value)
    local args = {[1] = 104847909005603}
    game:GetService("ReplicatedStorage").Remotes.Wear:InvokeServer(unpack(args))
end})

Tab1:AddButton({"Terno de Meme de Stuart Minion", function(Value)
    local args = {[1] = 124072436047913}
    game:GetService("ReplicatedStorage").Remotes.Wear:InvokeServer(unpack(args))
end})

Tab1:AddButton({"Terno de Esponja BIG BOX", function(Value)
    local args = {[1] = 85821412160409}
    game:GetService("ReplicatedStorage").Remotes.Wear:InvokeServer(unpack(args))
end})

Tab1:AddButton({"Terno de Meme Doge", function(Value)
    local args = {[1] = 18603754985}
    game:GetService("ReplicatedStorage").Remotes.Wear:InvokeServer(unpack(args))
end})

Tab1:AddButton({"Terno de Meme de Tung Tung Sahur", function(Value)
    local args = {[1] = 112976489682626}
    game:GetService("ReplicatedStorage").Remotes.Wear:InvokeServer(unpack(args))
end})

Tab1:AddButton({"Terno de Meme do Homem Besouro Gordo", function(Value)
    local args = {[1] = 90072009024993}
    game:GetService("ReplicatedStorage").Remotes.Wear:InvokeServer(unpack(args))
end})

Tab1:AddButton({"Costura Carregando Lilo Meme Suit", function(Value)
    local args = {[1] = 70916917507758}
    game:GetService("ReplicatedStorage").Remotes.Wear:InvokeServer(unpack(args))
end})

Tab1:AddButton({"Terno de Felipe Cabeça Corredora", function(Value)
    local args = {[1] = 83304202464009}
    game:GetService("ReplicatedStorage").Remotes.Wear:InvokeServer(unpack(args))
end})

Tab1:AddButton({"Terno de Meme de Gato Alienígena", function(Value)
    local args = {[1] = 115587652300404}
    game:GetService("ReplicatedStorage").Remotes.Wear:InvokeServer(unpack(args))
end})

local Section = Tab1:AddSection({"Chinelas👟"})

Tab1:AddButton({"Adeedaz 👟", function(Value)
    print("Equipando chinela: Adeedaz")
    local args = {[1] = 279995051676488}
    game:GetService("ReplicatedStorage").Remotes.WearBundle:InvokeServer(unpack(args))
end})

Tab1:AddButton({"Designer G 👟", function(Value)
    print("Equipando chinela: Designer G")
    local args = {[1] = 277553067428640}
    game:GetService("ReplicatedStorage").Remotes.WearBundle:InvokeServer(unpack(args))
end})

Tab1:AddButton({"Listrados 👟", function(Value)
    print("Equipando chinela: Listrados")
    local args = {[1] = 6700445946786}
    game:GetService("ReplicatedStorage").Remotes.WearBundle:InvokeServer(unpack(args))
end})

Tab1:AddButton({"Tubarão Azul Camo 👟", function(Value)
    print("Equipando chinela: Tubarão Azul Camo")
    local args = {[1] = 79410748636258}
    game:GetService("ReplicatedStorage").Remotes.WearBundle:InvokeServer(unpack(args))
end})

Tab1:AddButton({"Verão Preto 3.0 👟", function(Value)
    print("Equipando chinela: Verão Preto 3.0")
    local args = {[1] = 263348403978257}
    game:GetService("ReplicatedStorage").Remotes.WearBundle:InvokeServer(unpack(args))
end})

Tab1:AddButton({"Tubarão Escorregas 👟", function(Value)
    print("Equipando chinela: Tubarão Escorregas")
    local args = {[1] = 75658613978475}
    game:GetService("ReplicatedStorage").Remotes.WearBundle:InvokeServer(unpack(args))
end})

Tab1:AddButton({"Nuvem Rosa 👟", function(Value)
    print("Equipando chinela: Nuvem Rosa")
    local args = {[1] = 277056850232091}
    game:GetService("ReplicatedStorage").Remotes.WearBundle:InvokeServer(unpack(args))
end})

Tab1:AddButton({"Yeezi 👟", function(Value)
    print("Equipando chinela: Yeezi")
    local args = {[1] = 41713003192978}
    game:GetService("ReplicatedStorage").Remotes.WearBundle:InvokeServer(unpack(args))
end})

Tab1:AddButton({"Slides Amarelos 👟", function(Value)
    print("Equipando chinela: Slides Amarelos")
    local args = {[1] = 6101322876704}
    game:GetService("ReplicatedStorage").Remotes.WearBundle:InvokeServer(unpack(args))
end})

Tab1:AddButton({"Slides de Tubarão de Camuflagem Azul 👟", function(Value)
    print("Equipando chinela: Tubarão Azul Camo")
    local args = {[1] = 14106923557257}
    game:GetService("ReplicatedStorage").Remotes.WearBundle:InvokeServer(unpack(args))
end})

Tab1:AddButton({"Slides Amarelos 3.0 👟", function(Value)
    print("Equipando chinela: Slides Amarelos 3.0")
    local args = {[1] = 113651211516419}
    game:GetService("ReplicatedStorage").Remotes.WearBundle:InvokeServer(unpack(args))
end})

Tab1:AddButton({"Slides Azuis Marinhos 👟", function(Value)
    print("Equipando chinela: Slides Azuis Marinhos")
    local args = {[1] = 82533775987890}
    game:GetService("ReplicatedStorage").Remotes.WearBundle:InvokeServer(unpack(args))
end})

local Section = Tab1:AddSection({"Brainrots 🦈"})

Tab1:AddButton({"Fato de Dragão Cannelloni", function(Value)
    local args = {[1] = 105037212529016}
    game:GetService("ReplicatedStorage").Remotes.Wear:InvokeServer(unpack(args))
end})

Tab1:AddButton({"Terno Graipuss Medussi", function(Value)
    local args = {[1] = 127743038775642}
    game:GetService("ReplicatedStorage").Remotes.Wear:InvokeServer(unpack(args))
end})

Tab1:AddButton({"Los Hotspotsitos", function(Value)
    local args = {[1] = 95978237300828}
    game:GetService("ReplicatedStorage").Remotes.Wear:InvokeServer(unpack(args))
end})

Tab1:AddButton({"Brainrot Tralalero Tralalera", function(Value)
    local args = {[1] = 131014210567148}
    game:GetService("ReplicatedStorage").Remotes.Wear:InvokeServer(unpack(args))
end})

Tab1:AddButton({"Los Crocodillitos", function(Value)
    local args = {[1] = 82458110331849}
    game:GetService("ReplicatedStorage").Remotes.Wear:InvokeServer(unpack(args))
end})

Tab1:AddButton({"La Vacca Saturno", function(Value)
    local args = {[1] = 79099433191189}
    game:GetService("ReplicatedStorage").Remotes.Wear:InvokeServer(unpack(args))
end})

Tab1:AddButton({"Cocofanto Elefanto", function(Value)
    local args = {[1] = 106861997131346}
    game:GetService("ReplicatedStorage").Remotes.Wear:InvokeServer(unpack(args))
end})

Tab1:AddButton({"Brainrot Laranja Odin", function(Value)
    local args = {[1] = 90714986703814}
    game:GetService("ReplicatedStorage").Remotes.Wear:InvokeServer(unpack(args))
end})

Tab1:AddButton({"Brainrot Granden Combimasione", function(Value)
    local args = {[1] = 135688468458320}
    game:GetService("ReplicatedStorage").Remotes.Wear:InvokeServer(unpack(args))
end})

Tab1:AddButton({"Terno Orcalero Orcala", function(Value)
    local args = {[1] = 119901766964222}
    game:GetService("ReplicatedStorage").Remotes.Wear:InvokeServer(unpack(args))
end})

Tab1:AddButton({"Terno Frigo Camelo", function(Value)
    local args = {[1] = 123000431198710}
    game:GetService("ReplicatedStorage").Remotes.Wear:InvokeServer(unpack(args))
end})

Tab1:AddButton({"Brainrot Spioniro Golubiro", function(Value)
    local args = {[1] = 80325686037748}
    game:GetService("ReplicatedStorage").Remotes.Wear:InvokeServer(unpack(args))
end})

Tab1:AddButton({"Brainrot Ballerina Capucina", function(Value)
    local args = {[1] = 81606168963409}
    game:GetService("ReplicatedStorage").Remotes.Wear:InvokeServer(unpack(args))
end})

Tab1:AddButton({"Céleste Girafa", function(Value)
    local args = {[1] = 91771204091212}
    game:GetService("ReplicatedStorage").Remotes.Wear:InvokeServer(unpack(args))
end})

Tab1:AddButton({"Los Hotspotsitos (repetido)", function(Value)
    local args = {[1] = 95978237300828}
    game:GetService("ReplicatedStorage").Remotes.Wear:InvokeServer(unpack(args))
end})

local Section = Tab1:AddSection({"Horror👻"})

Tab1:AddButton({"Traje de Horror Aberrante", function(Value)
    print("Equipando traje: Traje de Horror Aberrante")
    local args = {[1] = 15375833616}
    game:GetService("ReplicatedStorage").Remotes.Wear:InvokeServer(unpack(args))
end})

Tab1:AddButton({"Bebê Monstro Chorão", function(Value)
    print("Equipando traje: Bebê Monstro Chorão")
    local args = {[1] = 118883854473837}
    game:GetService("ReplicatedStorage").Remotes.Wear:InvokeServer(unpack(args))
end})

Tab1:AddButton({"Terno da Vovó", function(Value)
    print("Equipando traje: Terno da Vovó")
    local args = {[1] = 121822531963261}
    game:GetService("ReplicatedStorage").Remotes.Wear:InvokeServer(unpack(args))
end})

Tab1:AddButton({"Terno do Sr. Crawling", function(Value)
    print("Equipando traje: Terno do Sr. Crawling")
    local args = {[1] = 100858184904362}
    game:GetService("ReplicatedStorage").Remotes.Wear:InvokeServer(unpack(args))
end})

Tab1:AddButton({"Pesadelo do Freddy Fazbear (FNAF)", function(Value)
    print("Equipando traje: Pesadelo do Freddy Fazbear")
    local args = {[1] = 112000081829900}
    game:GetService("ReplicatedStorage").Remotes.Wear:InvokeServer(unpack(args))
end})

Tab1:AddButton({"Fantasia de Fantasma", function(Value)
    print("Equipando traje: Fantasia de Fantasma")
    local args = {[1] = 14817470854}
    game:GetService("ReplicatedStorage").Remotes.Wear:InvokeServer(unpack(args))
end})

Tab1:AddButton({"Tarantula Animada Preta", function(Value)
    print("Equipando traje: Tarantula Animada Preta")
    local args = {[1] = 14702180155}
    game:GetService("ReplicatedStorage").Remotes.Wear:InvokeServer(unpack(args))
end})

Tab1:AddButton({"Terno de Manto REPO", function(Value)
    print("Equipando traje: Terno de Manto REPO")
    local args = {[1] = 139953945891034}
    game:GetService("ReplicatedStorage").Remotes.Wear:InvokeServer(unpack(args))
end})

Tab1:AddButton({"Monstro de Crânio Liso", function(Value)
    print("Equipando traje: Monstro de Crânio Liso")
    local args = {[1] = 74129219949445}
    game:GetService("ReplicatedStorage").Remotes.Wear:InvokeServer(unpack(args))
end})

Tab1:AddButton({"Rosto Fantasma", function(Value)
    print("Equipando traje: Rosto Fantasma")
    local args = {[1] = 134685518934902}
    game:GetService("ReplicatedStorage").Remotes.Wear:InvokeServer(unpack(args))
end})

Tab1:AddButton({"Terno de Jack Skellington", function(Value)
    print("Equipando traje: Terno de Jack Skellington")
    local args = {[1] = 90952898942799}
    game:GetService("ReplicatedStorage").Remotes.Wear:InvokeServer(unpack(args))
end})

Tab1:AddButton({"Fantasia de Palhaço Zumbi", function(Value)
    print("Equipando traje: Fantasia de Palhaço Zumbi")
    local args = {[1] = 114024385585984}
    game:GetService("ReplicatedStorage").Remotes.Wear:InvokeServer(unpack(args))
end})

Tab1:AddButton({"Fantasia de Espantalho de Halloween", function(Value)
    print("Equipando traje: Fantasia de Espantalho de Halloween")
    local args = {[1] = 104812394250191}
    game:GetService("ReplicatedStorage").Remotes.Wear:InvokeServer(unpack(args))
end})

Tab1:AddButton({"Halloween Traje Fantasma", function(Value)
    print("Equipando traje: Halloween Traje Fantasma")
    local args = {[1] = 111540468057138}
    game:GetService("ReplicatedStorage").Remotes.Wear:InvokeServer(unpack(args))
end})

Tab1:AddButton({"Assistindo Olhos Weirdcore", function(Value)
    print("Equipando traje: Assistindo Olhos Weirdcore")
    local args = {[1] = 103561186597145}
    game:GetService("ReplicatedStorage").Remotes.Wear:InvokeServer(unpack(args))
end})

Tab1:AddButton({"Boneca de Gatinho de Terror", function(Value)
    print("Equipando traje: Boneca de Gatinho de Terror")
    local args = {[1] = 18908349847}
    game:GetService("ReplicatedStorage").Remotes.Wear:InvokeServer(unpack(args))
end})

Tab1:AddButton({"Tung-Tung Sahur", function(Value)
    print("Equipando traje: Tung-Tung Sahur")
    local args = {[1] = 116614385560033}
    game:GetService("ReplicatedStorage").Remotes.Wear:InvokeServer(unpack(args))
end})

Tab1:AddButton({"Terno de Demônio Cozido", function(Value)
    print("Equipando traje: Terno de Demônio Cozido")
    local args = {[1] = 18580369062}
    game:GetService("ReplicatedStorage").Remotes.Wear:InvokeServer(unpack(args))
end})

Tab1:AddButton({"Terno de Rabid", function(Value)
    print("Equipando traje: Terno de Rabid")
    local args = {[1] = 134827533352401}
    game:GetService("ReplicatedStorage").Remotes.Wear:InvokeServer(unpack(args))
end})

Tab1:AddButton({"Terno de Cervo 99 Noites", function(Value)
    print("Equipando traje: Terno de Cervo 99 Noites")
    local args = {[1] = 89449626498261}
    game:GetService("ReplicatedStorage").Remotes.Wear:InvokeServer(unpack(args))
end})

Tab1:AddButton({"Grande Inseto", function(Value)
    print("Equipando traje: Grande Inseto")
    local args = {[1] = 79387545162107}
    game:GetService("ReplicatedStorage").Remotes.Wear:InvokeServer(unpack(args))
end})

Tab1:AddButton({"Traje Venom – A Última Dança", function(Value)
    print("Equipando traje: Traje Venom – A Última Dança")
    local args = {[1] = 129994953816824}
    game:GetService("ReplicatedStorage").Remotes.Wear:InvokeServer(unpack(args))
end})

Tab1:AddButton({"Roupa de Bloater – The Last Of Us", function(Value)
    print("Equipando traje: Roupa de Bloater – The Last Of Us")
    local args = {[1] = 88848965206503}
    game:GetService("ReplicatedStorage").Remotes.Wear:InvokeServer(unpack(args))
end})

local Section = Tab1:AddSection({"skins/bundles⛹️"})

-- Serviços
local ReplicatedStorage = game:GetService("ReplicatedStorage")

-- Lista completa de skins (com atualização das novas skins)
local Skins = {
    ["Tung Sahur"] = {RightArm=86770560381523,LeftArm=135772756700508,RightLeg=128193299309293,LeftLeg=110657588975713,Torso=111712044654607,Head=106857381600240},
    ["Privado"] = {RightArm=122913494458042,LeftArm=72973783793776,RightLeg=127484635016213,LeftLeg=134322960960488,Torso=131326252697127,Head=72445151535284},
    ["Zumbi"] = {RightArm=37754562,LeftArm=37754607,RightLeg=37754710,LeftLeg=37754646,Torso=37754511,Head=0},
    ["Bob Esponja"] = {RightArm=137837137411461,LeftArm=90231381117829,RightLeg=123197748034084,LeftLeg=104187261272015,Torso=125512861440110,Head=116698708417813},
    ["Broccoli Bro"] = {RightArm=117447764892232,LeftArm=117447764892232,RightLeg=124901576233203,LeftLeg=71888325847045,Torso=127699569564923,Head=109758497364340},
    ["Smiling Freak"] = {RightArm=95438698170561,LeftArm=81043872188250,RightLeg=110533795025945,LeftLeg=94252659031992,Torso=134794806756884,Head=71666458205606},
    ["Strong Man"] = {RightArm=92693920046155,LeftArm=104838990147381,RightLeg=87581490268924,LeftLeg=118031041459907,Torso=70725379775042,Head=0},
    ["Big Guy"] = {RightArm=82313592622379,LeftArm=136892759155607,RightLeg=90839831988045,LeftLeg=81744786606406,Torso=130533187401119,Head=71187536103249},
    ["Muscle Man"] = {RightArm=77883139413327,LeftArm=139980873992922,RightLeg=101644314053250,LeftLeg=74680180779048,Torso=95816488370139,Head=116877198684544},
    ["Corpo Fino R15"] = {RightArm=76729669757995,LeftArm=120308525962473,RightLeg=134397735049983,LeftLeg=92613400080272,Torso=88891216141627,Head=0},
    ["inf15 Slimest"] = {RightArm=118537549245937,LeftArm=127224166043311,RightLeg=92761310535408,LeftLeg=84780334363562,Torso=136833940463116,Head=0},
    ["Hulk"] = {RightArm=139857082780448,LeftArm=119944070305698,RightLeg=75711998418858,LeftLeg=80616236635252,Torso=138350806176217,Head=131176305969389},
    ["Menino de Fogo Neon"] = {RightArm=79904816118323,LeftArm=94535419092709,RightLeg=74244919946068,LeftLeg=101449969004017,Torso=122827995284895,Head=89730689474650},
    ["Alien Azul Buff Koala"] = {RightArm=81010489627582,LeftArm=91960633389415,RightLeg=136686548465634,LeftLeg=135396308791383,Torso=88168056142533,Head=78092294723572},
    ["Tralalero Shark"] = {RightArm=80351596698909,LeftArm=83787578834105,RightLeg=139967696820989,LeftLeg=128608358926325,Torso=86314818624464,Head=70816283094760},
    ["Tom"] = {RightArm=99261141745161,LeftArm=97460507030382,RightLeg=124224945040870,LeftLeg=136255963295970,Torso=129924029490874,Head=82756526802985},
    ["Orador da Morte Korblox"] = {RightArm=139607625,LeftArm=139607570,RightLeg=139607718,LeftLeg=139607673,Torso=139607770,Head=0},
    ["Mulher Popular"] = {RightArm=93619527054005,LeftArm=132573773059708,RightLeg=95793698363049,LeftLeg=132149829725537,Torso=85629406858535,Head=0},
    ["Mini Plushie 2D"] = {RightArm=14579959062,LeftArm=14579959191,RightLeg=14579959249,LeftLeg=14579963667,Torso=14579958702,Head=0},
    ["KJ Stickman Anime Warrior"] = {RightArm=95699533490202,LeftArm=123479622900990,RightLeg=82729268855649,LeftLeg=89263922353322,Torso=138252823671784,Head=88449174088255},
    ["Mega Noob"] = {RightArm=100508133959213,LeftArm=134939106145092,RightLeg=105235043658399,LeftLeg=124281075881863,Torso=124585349380829,Head=95533897235529},
    ["C00lkidd"] = {RightArm=129097890849553,LeftArm=136169203394129,RightLeg=98247081907855,LeftLeg=124193586730337,Torso=123269862968240,Head=74161597712310},
    ["1x1x1x1"] = {RightArm=88487263776290,LeftArm=124662457446895,RightLeg=119599703816561,LeftLeg=94758797370283,Torso=95671414421054,Head=112042853248560},
    -- Novas skins
    ["Menino de água Neon"] = {RightArm=118124115791616,LeftArm=98733576931928,RightLeg=74558106579765,LeftLeg=120930477379873,Torso=109426113659396,Head=76989685474581},
    ["Boneca de Torta Bonita"] = {RightArm=15961250337,LeftArm=15961249215,RightLeg=15961245837,LeftLeg=15961249217,Torso=15961249218,Head=0},
    ["Meio-e-Meio Menino"] = {RightArm=15198245407,LeftArm=15198246175,RightLeg=15198246103,LeftLeg=15198245436,Torso=15198245658,Head=15198245566},
    ["Princesa Elegante"] = {RightArm=106062939061307,LeftArm=106848480527965,RightLeg=83345005058503,LeftLeg=116378577817571,Torso=109532471767670,Head=0},
    ["Buff Chad Doge Meme"] = {RightArm=119847194889283,LeftArm=75208394648018,RightLeg=132896871987417,LeftLeg=114121430921578,Torso=75984460826757,Head=112195996247540},
    ["Chad Swole Buff Doge"] = {RightArm=111850964158211,LeftArm=83445654140495,RightLeg=136182787645528,LeftLeg=131612680253479,Torso=136271717183865,Head=122268789269493},
    ["Manequim de Combate Madness"] = {RightArm=123816476309454,LeftArm=113354224482212,RightLeg=127078730093875,LeftLeg=129147108079423,Torso=94887045646521,Head=138831545293241},
    ["Tijolo Alto"] = {RightArm=114686063750880,LeftArm=87349575171039,RightLeg=80568702504923,LeftLeg=92991107737220,Torso=123846227494985,Head=0},
    ["Chad Absolutamente Rasgado"] = {RightArm=121532625082697,LeftArm=119216187383922,RightLeg=81382162911246,LeftLeg=85872934421624,Torso=0,Head=121425956834814},
    ["Big Back Blocky"] = {RightArm=81877885692687,LeftArm=114747197238754,RightLeg=74364166944777,LeftLeg=124418541315602,Torso=127435097087502,Head=0},
    ["Minion"] = {RightArm=87524135229263,LeftArm=133524170096605,RightLeg=133841451989951,LeftLeg=128919774509422,Torso=94628257970112,Head=115495156976962}
}

-- Variável de skin selecionada
local SelectedSkin = "Tung Sahur"

-- Atualiza a Dropdown com todas as skins (incluindo as novas)
local skinNames = {}
for name, _ in pairs(Skins) do
    table.insert(skinNames, name)
end

local SkinDropdown = Tab1:AddDropdown({
    Name = "Escolher Skin",
    Description = "Selecione a skin do personagem",
    Options = skinNames,
    Default = SelectedSkin,
    Callback = function(Value)
        SelectedSkin = Value
    end
})

-- Botão para aplicar a skin selecionada
Tab1:AddButton({
    Name = "Aplicar Skin",
    Callback = function()
        if Skins[SelectedSkin] then
            local skinData = Skins[SelectedSkin]

            local args = {
                [1] = {
                    [1] = skinData.RightArm or 0,
                    [2] = skinData.LeftArm or 0,
                    [3] = skinData.RightLeg or 0,
                    [4] = skinData.LeftLeg or 0,
                    [5] = skinData.Torso or 0,
                    [6] = skinData.Head or 0
                }
            }

            -- Envia pro servidor
            ReplicatedStorage.Remotes.ChangeCharacterBody:InvokeServer(unpack(args))
        else
            warn("Skin não encontrada: " .. tostring(SelectedSkin))
        end
    end
})

local Tab1 = Window:MakeTab({"vr", "airplay"})

local Section = Tab1:AddSection({"use óculosvr(script)"})

Tab1:AddButton({"equipar braço direito", function(Value)
print("Hello World!")local args = {
	84451219120140
}
game:GetService("ReplicatedStorage"):WaitForChild("Remotes"):WaitForChild("Wear"):InvokeServer(unpack(args))

end})

Tab1:AddButton({"Equipar braço(esquerda)", function(Value)
print("Hello World!")local args = {
	72292903231768
}
game:GetService("ReplicatedStorage"):WaitForChild("Remotes"):WaitForChild("Wear"):InvokeServer(unpack(args))

end})

Tab1:AddButton({"usar vr!", function(Value)
print("Hello World!")loadstring(game:HttpGet("https://raw.githubusercontent.com/randomstring0/Qwerty/refs/heads/main/qwerty45.lua"))()
end})

local Tab1 = Window:MakeTab({"Boombox", "Speaker"})

Tab1:AddTextBox({
    Name = "Boombox ID",
    Description = "Coloque o ID para o Boombox",
    PlaceholderText = "Ex: 7024143472",
    Callback = function(Value)
        if Value and Value ~= "" then
            local args = {
                "ToolMusicText",
                Value -- o ID digitado vai aqui
            }
            game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("PlayerToolEvent"):FireServer(unpack(args))
            print("Boombox tocando música com ID:", Value)
        else
            warn("Digite um ID válido para o Boombox!")
        end
    end
})

-- Variável para guardar o ID selecionado
local SelectedID = nil  

local Dropdown = Tab1:AddDropdown({
    Name = "IDS de música",
    Description = "Selecione o <font color='rgb(88, 101, 242)'>Áudio</font>",
    Options = {
        ["bypassed"] = "84994008476716",
        ["jersey"] = "94524508448994",
        ["bypassed (alt)"] = "112487908830297",
        ["W song"] = "110398899000118",
        ["sounds of my dream (jumpstyle)"] = "107220571819089",
        ["wth"] = "87628353249925",
        ["NYAN CAT LOUD"] = "94247393385483",
        ["beat"] = "109317774357031",
        ["LOUD SONG"] = "83216085380006",
        ["SLIDE"] = "130108792973868",
        ["breakcore"] = "102606024002362",
        ["Steven universe (cover chaka)"] = "76834562378901",
        ["Bad Apple full song (99% real)"] = "133864992185435",
        ["sus audio!!!!!!!!!!"] = "98179479769166",
        ["bypassed song"] = "132556256284397",
        ["good B0y"] = "82833883451531",
        ["n word o:"] = "78237548893699",
        ["intro?"] = "100387511408698",
        ["passo bem solto (full song)"] = "70626485375251",
        ["bypassed"] = "138148591773443",
        ["bypassed (extra)"] = "138481681938948",
        ["stay with me (full song real)"] = "130867828528278",
        ["doomSHOOP"] = "106750621124046",
        ["slackwoods loud"] = "8525745255",
        ["car drip"] = "8854899077",
        ["ZACH RABBIT - WORLD WIDE MASSACRE"] = "9087130609",
        ["ww2 song"] = "108254832140939",
        ["doomshop beat or idk"] = "864140695",
        ["song"] = "8547854164",
        ["mc holocaust"] = "2809353108",
        ["Memphis Rap type beat"] = "107976923850048",
        ["Doomshop"] = "123907861988079",
        ["Type Beat"] = "84745102224610",
        ["Memphis Rap Instr"] = "110702112597688",
        ["Memphis Rap type beat (alt)"] = "87726428794890",
        ["Type Beat (alt)"] = "80749640423571",
        ["Idk beat"] = "118498168825366",
        ["Idk Cool Beat"] = "96403072895021",
        ["Beat (extra)"] = "133314546406963",
        ["Fire Memphis Phonk"] = "104362531847018",
        ["idk???"] = "116056588632846",
        ["loud nword spam"] = "134330459190038",
        ["bobby2pistolz - fck all nword"] = "95098497504226",
        ["doomshop (alt)"] = "126733760143984",
        ["lil boodang gay shit backup"] = "116948138165385",
        ["lil boodang gay shit"] = "92028144794492",
        ["joey street - sesame street"] = "128784976267137",
        ["yung bratz - xxxtentation full"] = "96415762266433",
        ["sadboyshaq - hedied (pt.2)"] = "97173189032263",
        ["rare lungskull ???"] = "85377124824346",
        ["loud ass cowbell beat"] = "114651840802379",
        ["doomshop beat or idk???"] = "77562935961036",
        ["shotgun willy - wendy"] = "116198562628583",
        ["idk??? (extra)"] = "106802187257577",
        ["idk??? (alt2)"] = "130603157461027",
        ["some loud ass 10 second rap"] = "94936666508518",
        ["slashtapez"] = "138356090908010",
        ["lil pimp 187 - pimp dat ho"] = "138685494117478",
        ["jp jav sounds"] = "102453913102995",
        ["gucci gang loop"] = "2547598538",
        ["look at me - xxxtentaction"] = "131107293184621",
        ["phonk"] = "97744904211218",
        ["shotta flow bad quality"] = "70816527824441",
        ["doomshop cowbell beat"] = "98729729304116",
        ["comet idk"] = "121817544798287",
        ["comet idk (alt)"] = "134812080886689",
        ["south park nword"] = "128507831115651",
        ["lit in this bitch by xxxtentacion"] = "96460829282621",
        ["Frigo camela funk"] = "87635539433153",
        ["bobombinin funk"] = "111543364511665",
        ["that lil darkie song full"] = "114403033423410",

        -- IDs novos com nome já arrumado

        ["loud breakbeat"] = "94857261584834",
        ["bass boosted funk"] = "71701052305256",
        ["trap phonk remix"] = "128529679058488",
        ["weird meme audio"] = "82737147971523",
        ["crazy cowbell beat"] = "130826845701324",
        ["glitchy song"] = "71055309537215",
        ["memphis dark beat"] = "124420421741287",
        ["slowed reverb audio"] = "135703252977174",
        ["drift phonk vibe"] = "131344160755402",
        ["deep distorted song"] = "118750888001463",
        ["funky bass loop"] = "134238529752505",
        ["trap experimental"] = "119063510774364",
        ["sped up goofy song"] = "118425592773242",
        ["oldschool rap vibe"] = "98206529715838",
        ["beat drop loud"] = "110537904253290",
        ["short funny sound"] = "12730670552777",
        ["sus bass hit"] = "8417000911951",
        ["random phonk edit"] = "116391783579226"
    },
    Default = "84994008476716", 
    Flag = "dropdown teste",
    Callback = function(Value)
        SelectedID = Value
    end
})

Tab1:AddButton({
    Name = "Play Selected ID",
    Callback = function()
        if SelectedID then
            local args = {
                "ToolMusicText",
                SelectedID
            }
            game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("PlayerToolEvent"):FireServer(unpack(args))
            print("Playing ID:", SelectedID)
        else
            warn("Nenhum ID selecionado!")
        end
    end
})

-- eligenete rádio 

local RunService = game:GetService("RunService")
local toggled = false
local conn

Tab1:AddToggle({
    Name = "eligebetê rádio🏳️‍🌈",
    Default = false,
    Callback = function(Value)
        toggled = Value
        if toggled then
            local hue = 0
            conn = RunService.RenderStepped:Connect(function(dt)
                hue = (hue + dt * 0.25) % 1 -- velocidade do rgb (0.25 = suave, pouco rápido)
                local color = Color3.fromHSV(hue, 1, 1)

                local args = { color }
                game:GetService("Players").LocalPlayer
                    :WaitForChild("PlayerGui")
                    :WaitForChild("ToolGui")
                    :WaitForChild("ToolSettings")
                    :WaitForChild("Settings")
                    :WaitForChild("PropsColor")
                    :WaitForChild("SetColor"):FireServer(unpack(args))
            end)
        else
            if conn then
                conn:Disconnect()
                conn = nil
            end
        end
    end
})

-- pegar rádio

local args = {
	"PickingTools",
	"Boombox"
}
game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1Too1l"):InvokeServer(unpack(args))

local Tab1 = Window:MakeTab({"house", "home"})

-- colocar música na casa


-- Remote do House Music
local HouseRemote = game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1Player1sHous1e")

-- Textbox da música da casa
local houseMusicBox
houseMusicBox = Tab1:AddTextBox({
    Name = "House Music ID",
    Description = "Digite o ID da música para tocar na casa",
    PlaceholderText = "Ex: 1845732173",
    Callback = function(Value)
        if Value and Value ~= "" then
            local args = {
                "PickHouseMusicText",
                Value -- ID digitado
            }
            HouseRemote:FireServer(unpack(args))
            print("Música da casa enviada com ID:", Value)
        else
            warn("Digite um ID válido!")
        end
    end
})

--eligebetê casa

local RunService = game:GetService("RunService")
local toggledHouse = false
local houseConn

Tab1:AddToggle({
    Name = "eligebetê casa🏳️‍🌈",
    Default = false,
    Callback = function(Value)
        toggledHouse = Value
        if toggledHouse then
            local hue = 0
            houseConn = RunService.RenderStepped:Connect(function(dt)
                hue = (hue + dt * 0.25) % 1 -- velocidade RGB suave e pouco rápida
                local color = Color3.fromHSV(hue, 1, 1)

                local args = {
                    "ColorPickHouse",
                    color
                }
                game:GetService("ReplicatedStorage"):WaitForChild("RE")
                    :WaitForChild("1Player1sHous1e"):FireServer(unpack(args))
            end)
        else
            if houseConn then
                houseConn:Disconnect()
                houseConn = nil
            end
        end
    end
})

-- loop cortinas

local RunService = game:GetService("RunService")
local loopCurtains = false
local curtainsConn

Tab1:AddToggle({
    Name = "Loop Cortinas",
    Default = false,
    Callback = function(Value)
        loopCurtains = Value
        if loopCurtains then
            curtainsConn = RunService.RenderStepped:Connect(function()
                local args = { "Curtains" }
                game:GetService("ReplicatedStorage")
                    :WaitForChild("RE")
                    :WaitForChild("1Player1sHous1e"):FireServer(unpack(args))
            end)
        else
            if curtainsConn then
                curtainsConn:Disconnect()
                curtainsConn = nil
            end
        end
    end
})

-- loop nome da casa

local RunService = game:GetService("RunService")

-- Lista de nomes aleatórios
local randomNames = {
    "HackerX", "ExploitMaster", "Sombrio123", "FunnyName", "ShadowCoder",
    "GlitchLord", "DarkScript", "LOL123", "VirusMaker", "TrollFace"
}

-- TextBoxes para os nomes
local NameBox1, NameBox2
local Name1, Name2

NameBox1 = Tab1:AddTextBox({
    Name = "Nome 1",
    Description = "Primeiro nome",
    PlaceholderText = "Digite um nome",
    Callback = function(Value)
        Name1 = Value
    end
})

NameBox2 = Tab1:AddTextBox({
    Name = "Nome 2",
    Description = "Segundo nome",
    PlaceholderText = "Digite outro nome",
    Callback = function(Value)
        Name2 = Value
    end
})

-- Toggle para ativar loop
local loopToggle = false
local loopConn

Tab1:AddToggle({
    Name = "Loop Nomes Aleatórios",
    Default = false,
    Callback = function(Value)
        loopToggle = Value
        if loopToggle then
            loopConn = RunService.RenderStepped:Connect(function()
                local nameToSend = math.random(0, 1) == 0 and (Name1 or randomNames[math.random(#randomNames)]) or (Name2 or randomNames[math.random(#randomNames)])
                local args = { "BusinessName", nameToSend }
                game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1RPHous1eEven1t"):FireServer(unpack(args))
            end)
        else
            if loopConn then
                loopConn:Disconnect()
                loopConn = nil
            end
        end
    end
})

--tab tools


local Tab1 = Window:MakeTab({"tools", "diamond"})

Tab1:AddButton({"bastão vermelho", function(Value)
print("Hello World!")local args = {
	"PickingTools",
	"JoustRed"
}
game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1Too1l"):InvokeServer(unpack(args))

Tab1:AddButton({"bastão azul", function(Value)
print("Hello World!")local args = {
	"PickingTools",
	"JoustBlue"
}
game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1Too1l"):InvokeServer(unpack(args))

end})

end})

Tab1:AddButton({"phone", function(Value)
print("Hello World!")local args = {
	"PickingTools",
	"Iphone"
}
game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1Too1l"):InvokeServer(unpack(args))

end})

Tab1:AddButton({"ipad", function(Value)
print("Hello World!")local args = {
	"PickingTools",
	"Ipad"
}
game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1Too1l"):InvokeServer(unpack(args))

end})

Tab1:AddButton({"carrinho de super merkado", function(Value)
print("Hello World!")local args = {
	"PickingTools",
	"ShoppingCart"
}
game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1Too1l"):InvokeServer(unpack(args))

end})

--looping

local Section = Tab1:AddSection({"loop tools"})

Tab1:AddButton({"bola de basquete", function(Value)
print("Hello World!")local args = {
	"PickingTools",
	"Basketball"
}
game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1Too1l"):InvokeServer(unpack(args))

end})

local RunService = game:GetService("RunService")
local basketballLoop = false
local basketballConn

Tab1:AddToggle({
    Name = "Loop Basketball Click",
    Default = false,
    Callback = function(Value)
        basketballLoop = Value
        if basketballLoop then
            basketballConn = RunService.RenderStepped:Connect(function()
                local args = {
                    Vector3.new(-401.5209655761719, 3.631436586380005, 202.01866149902344)
                }
                game:GetService("Players").LocalPlayer.Character:WaitForChild("Basketball"):WaitForChild("ClickEvent"):FireServer(unpack(args))
            end)
        else
            if basketballConn then
                basketballConn:Disconnect()
                basketballConn = nil
            end
        end
    end
})

local Tab1 = Window:MakeTab({"Textures guns", "crosshair"})

-- Lista de IDs (PACK)
local ids = {
    82949716045571,
    130570291094026,
    95703748125043,
    119121346783288,
    95664561434978,
    107325653054504,
    100155459354103,
    94187877368068,
    113948679058093,
    75640291901081,
    104541017111105,
    108395454884467,
    107598801697771,
    101788540945345,
    126144918462510,
    110069496303021,
    72794526995816,
    105640169090641,
    81974324784760,
    114203882933304,
    95934746711773,
    108117949112299,
    97950504020289,
    120541914018737,
    5506271767,
    8302130622,
    5916312170,
    6591968555,
    5688274885,
    5897304285,
    5356508317,
    87209139999463,
    116323381046345,
    134278152703829,
    132718792213834,
    106740840356090,
    88617760765364,
    4923218048,
    7145987085,
    9341850470,
    10780012744,
    11818627057,
    10180536577,
    8373881910,
    10180628683,
    5570977826,
    10491133358,
    12245026315,
    11623459240,
    6675554347,
    11435555480,
    7279156932,
    9260491536,
    6911592570,
    12623079242,
    12053823591,
    9885068518,
    9838256132,
    4732992778,
    9182757465,
    8119525418,
    7279155187,
    10111107769,
    10729455634,
    4700049607,
    7371693428,
    5328690050,
    8677289915,
    107652335389482,
    110384539527502,
    119283840843239,
    129187639801762,
    113028559909249,
    112303639893244,
    82996326776226,
    127514995327502,
    89661983615255
}

-- Dropdown
local Dropdown = Tab1:AddDropdown({
    Name = "escolha sua textura",
    Description = "Select the <font color='rgb(88, 101, 242)'>ID</font>",
    Options = ids,
    Default = ids[1],
    Flag = "dropdown teste",
    Callback = function(selectedId)
        -- Quando selecionar, armazena em uma variável
        _G.SelectedID = selectedId
        print("Selected ID:", selectedId)
    end
})

-- Botão para aplicar a textura
Tab1:AddButton({
    Name = "Aplicar textura🔫",
    Callback = function()
        if _G.SelectedID then
            local args = {
                "RequestingGunSkins",
                _G.SelectedID
            }
            game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1Clea1rTool1s"):FireServer(unpack(args))
            print("Skin applied:", _G.SelectedID)
        else
            print("No ID selected!")
        end
    end
})

-- Textura 1
Tab1:AddButton({"Textura 1 🔫", function(Value)
    local args = {"RequestingGunSkins", "126505720357298"}
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1Clea1rTool1s"):FireServer(unpack(args))
end})

-- Textura 2
Tab1:AddButton({"Textura 2 🔫", function(Value)
    local args = {"RequestingGunSkins", "120574546951146"}
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1Clea1rTool1s"):FireServer(unpack(args))
end})

-- Textura 3
Tab1:AddButton({"Textura 3 🔫", function(Value)
    local args = {"RequestingGunSkins", "130863629964766"}
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1Clea1rTool1s"):FireServer(unpack(args))
end})

-- Textura 4 (sem id)
Tab1:AddButton({"Textura 4 🔫", function(Value)
    -- vazio
end})

-- Textura 5
Tab1:AddButton({"Textura 5 🔫", function(Value)
    local args = {"RequestingGunSkins", "118502929640661"}
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1Clea1rTool1s"):FireServer(unpack(args))
end})

-- Textura 6 (sem id)
Tab1:AddButton({"Textura 6 🔫", function(Value)
    -- vazio
end})

-- Textura 7
Tab1:AddButton({"Textura 7 🔫", function(Value)
    local args = {"RequestingGunSkins", "92335138140660"}
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1Clea1rTool1s"):FireServer(unpack(args))
end})

-- Textura 8
Tab1:AddButton({"Textura 8 🔫", function(Value)
    local args = {"RequestingGunSkins", "18268161208"}
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1Clea1rTool1s"):FireServer(unpack(args))
end})

-- Textura 9
Tab1:AddButton({"Textura 9 🔫", function(Value)
    local args = {"RequestingGunSkins", "83621666834480"}
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1Clea1rTool1s"):FireServer(unpack(args))
end})

-- Textura 10
Tab1:AddButton({"Textura 10 🔫", function(Value)
    local args = {"RequestingGunSkins", "10316859495"}
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1Clea1rTool1s"):FireServer(unpack(args))
end})

-- Textura 11
Tab1:AddButton({"Textura 11 🔫", function(Value)
    local args = {"RequestingGunSkins", "9359131807"}
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1Clea1rTool1s"):FireServer(unpack(args))
end})

local Section = Tab1:AddSection({"Pegar armas"})

-- Sniper
Tab1:AddButton({
    "Sniper",
    function()
        local args = {
            [1] = "PickingTools",
            [2] = "Sniper"
        }
        game:GetService("ReplicatedStorage").RE:FindFirstChild("1Too1l"):InvokeServer(unpack(args))
    end
})

-- Rifle de Assalto
Tab1:AddButton({
    "Rifle de Assalto",
    function()
        local args = {
            [1] = "PickingTools",
            [2] = "Assault"
        }
        game:GetService("ReplicatedStorage").RE:FindFirstChild("1Too1l"):InvokeServer(unpack(args))
    end
})

-- Espingarda
Tab1:AddButton({
    "Espingarda",
    function()
        local args = {
            [1] = "PickingTools",
            [2] = "Shotgun"
        }
        game:GetService("ReplicatedStorage").RE:FindFirstChild("1Too1l"):InvokeServer(unpack(args))
    end
})

-- Glock Marrom
Tab1:AddButton({
    "Glock Marrom",
    function()
        local args = {
            [1] = "PickingTools",
            [2] = "GlockBrown"
        }
        game:GetService("ReplicatedStorage").RE:FindFirstChild("1Too1l"):InvokeServer(unpack(args))
    end
})

-- Glock
Tab1:AddButton({
    "Glock",
    function()
        local args = {
            [1] = "PickingTools",
            [2] = "Glock"
        }
        game:GetService("ReplicatedStorage").RE:FindFirstChild("1Too1l"):InvokeServer(unpack(args))
    end
})



local Tab1 = Window:MakeTab({"Scripts", "terminal"})

-- Botão Jerk Off Tool
Tab1:AddButton({
    Name = "Jerk Off Tool 🍌",
    Callback = function()
        loadstring(game:HttpGet("https://rawscripts.net/raw/Universal-Script-R15-Jerk-Off-Tool-38782"))()
    end
})

-- Botão Infinite Yield
Tab1:AddButton({
    Name = "Infinite Yield ♾️",
    Callback = function()
        loadstring(game:HttpGet("https://rawscripts.net/raw/Universal-Script-Infinite-Yield-43437"))()
    end
})

-- Botão Nameless Admin
Tab1:AddButton({
    Name = "Nameless Admin 👑",
    Callback = function()
        loadstring(game:HttpGet("https://rawscripts.net/raw/Universal-Script-Nameless-admin-REWORKED-43502"))()
    end
})
    
    Tab1:AddButton({"Tp tool⚡", function(Value)
print("Hello World!")-- Cria a Tool
local tool = Instance.new("Tool")
tool.Name = "TP Tool"
tool.RequiresHandle = false
tool.Parent = game.Players.LocalPlayer.Backpack

-- Função quando o player clica
tool.Activated:Connect(function()
    local player = game.Players.LocalPlayer
    local mouse = player:GetMouse()

    -- Teleporta o HumanoidRootPart para a posição do mouse
    if mouse and mouse.Hit then
        local char = player.Character
        if char and char:FindFirstChild("HumanoidRootPart") then
            char.HumanoidRootPart.CFrame = CFrame.new(mouse.Hit.p + Vector3.new(0, 3, 0))
        end
    end
end)
end})

Tab1:AddButton({"Chat bypass 🗝️", function(Value)
print("Hello World!")loadstring(game:HttpGet("https://rawscripts.net/raw/Universal-Script-byter-chat-bypas-ser-49224"))()
end})

Tab1:AddButton({"Remote spy👁️", function(Value)
print("Hello World!")loadstring(game:HttpGet("https://rawscripts.net/raw/Universal-Script-RemoteSpy-by-Redz-25501"))()
end})

    Tab1:AddButton({"Chat bypass(você pode escrever no chat sem HASHTAGS.)", function(Value)
print("Hello World!")loadstring(game:HttpGet("https://rawscripts.net/raw/Universal-Script-byter-chat-bypas-ser-49224"))()
end})

Tab1:AddButton({"chat spam💬", function(Value)
print("Hello World!")local TextChatService = game:GetService("TextChatService")
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")

-- Функция для создания GUI
local function createGui(player)
    local ScreenGui = Instance.new("ScreenGui")
    local Frame = Instance.new("Frame")
    local MessageBox = Instance.new("TextBox")
    local CountBox = Instance.new("TextBox")
    local SendButton = Instance.new("TextButton")
    local ToggleButton = Instance.new("TextButton")
    local CloseButton = Instance.new("TextButton")
    local SettingsButton = Instance.new("TextButton")
    local SettingsFrame = Instance.new("Frame")
    local LanguageButton = Instance.new("TextButton")
    local CensorshipToggle = Instance.new("TextButton")
    
    ScreenGui.Parent = player:WaitForChild("PlayerGui")

    -- Настройка Frame
    Frame.Size = UDim2.new(0, 300, 0, 250)
    Frame.Position = UDim2.new(0.5, -150, 0.5, -125)
    Frame.BackgroundColor3 = Color3.fromRGB(45, 45, 45)
    Frame.BorderSizePixel = 0
    Frame.Active = true
    Frame.Draggable = true
    Frame.Parent = ScreenGui

    -- Настройка MessageBox
    MessageBox.Size = UDim2.new(0, 280, 0, 50)
    MessageBox.Position = UDim2.new(0, 10, 0, 10)
    MessageBox.PlaceholderText = "Введите сообщение"
    MessageBox.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
    MessageBox.TextColor3 = Color3.fromRGB(0, 0, 0)
    MessageBox.BorderSizePixel = 0
    MessageBox.Parent = Frame

    -- Настройка CountBox
    CountBox.Size = UDim2.new(0, 280, 0, 50)
    CountBox.Position = UDim2.new(0, 10, 0, 70)
    CountBox.PlaceholderText = "Введите количество"
    CountBox.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
    CountBox.TextColor3 = Color3.fromRGB(0, 0, 0)
    CountBox.BorderSizePixel = 0
    CountBox.Parent = Frame

    -- Настройка SendButton
    SendButton.Size = UDim2.new(0, 280, 0, 50)
    SendButton.Position = UDim2.new(0, 10, 0, 130)
    SendButton.Text = "Отправить" -- Перевод кнопки
    SendButton.BackgroundColor3 = Color3.fromRGB(0, 170, 255)
    SendButton.TextColor3 = Color3.fromRGB(255, 255, 255)
    SendButton.BorderSizePixel = 0
    SendButton.Parent = Frame

    -- Настройка ToggleButton
    ToggleButton.Size = UDim2.new(0, 100, 0, 50)
    ToggleButton.Position = UDim2.new(0, 10, 0, 10)
    ToggleButton.Text = "Свернуть/Развернуть"
    ToggleButton.BackgroundColor3 = Color3.fromRGB(0, 170, 255)
    ToggleButton.TextColor3 = Color3.fromRGB(255, 255, 255)
    ToggleButton.BorderSizePixel = 0
    ToggleButton.Parent = ScreenGui

    -- Настройка CloseButton
    CloseButton.Size = UDim2.new(0, 50, 0, 50)
    CloseButton.Position = UDim2.new(1, -60, 0, 10)
    CloseButton.Text = "X"
    CloseButton.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
    CloseButton.TextColor3 = Color3.fromRGB(255, 255, 255)
    CloseButton.BorderSizePixel = 0
    CloseButton.Parent = Frame

    -- Настройка SettingsButton
    SettingsButton.Size = UDim2.new(0, 280, 0, 50)
    SettingsButton.Position = UDim2.new(0, 10, 0, 190)
    SettingsButton.Text = "Настройки"
    SettingsButton.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
    SettingsButton.TextColor3 = Color3.fromRGB(255, 255, 255)
    SettingsButton.BorderSizePixel = 0
    SettingsButton.Parent = Frame

    -- Настройка SettingsFrame
    SettingsFrame.Size = UDim2.new(0, 300, 0, 150)
    SettingsFrame.Position = UDim2.new(0.5, -150, 0.5, -75)
    SettingsFrame.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
    SettingsFrame.Visible = false
    SettingsFrame.Parent = ScreenGui

    -- Настройка LanguageButton
    LanguageButton.Size = UDim2.new(0, 280, 0, 50)
    LanguageButton.Position = UDim2.new(0, 10, 0, 10)
    LanguageButton.Text = "Изменить язык"
    LanguageButton.BackgroundColor3 = Color3.fromRGB(0, 170, 255)
    LanguageButton.TextColor3 = Color3.fromRGB(255, 255, 255)
    LanguageButton.BorderSizePixel = 0
    LanguageButton.Parent = SettingsFrame

    -- Настройка CensorshipToggle
    CensorshipToggle.Size = UDim2.new(0, 280, 0, 50)
    CensorshipToggle.Position = UDim2.new(0, 10, 0, 70)
    CensorshipToggle.Text = "Цензура включена"
    CensorshipToggle.BackgroundColor3 = Color3.fromRGB(0, 170, 255)
    CensorshipToggle.TextColor3 = Color3.fromRGB(255, 255, 255)
    CensorshipToggle.BorderSizePixel = 0
    CensorshipToggle.Parent = SettingsFrame

    -- Флаг для отслеживания состояния GUI
    local guiClosed = false
    local censorshipEnabled = true -- По умолчанию включена цензура
    local currentLanguage = "ru" -- По умолчанию русский

    -- Функция отправки сообщений
    local function sendMessages()
        local message = MessageBox.Text .. (censorshipEnabled and "" or " le le le le")
        local count = tonumber(CountBox.Text)

        if message ~= "" and count then
            for i = 1, count do
                local chatChannel = TextChatService:WaitForChild("TextChannels"):WaitForChild("RBXGeneral")
                chatChannel:SendAsync(message)
                wait(1) -- Задержка в 1 секунду между сообщениями
            end
        else
            warn("Пожалуйста, введите корректное сообщение и количество.")
        end
    end

    SendButton.MouseButton1Click:Connect(sendMessages)

    -- Функция для сворачивания и разворачивания GUI
    local isFrameVisible = true
    ToggleButton.MouseButton1Click:Connect(function()
        isFrameVisible = not isFrameVisible
        Frame.Visible = isFrameVisible
        ToggleButton.Text = isFrameVisible and "Свернуть" or "Развернуть"
    end)

    -- Функция для открытия настроек
    SettingsButton.MouseButton1Click:Connect(function()
        SettingsFrame.Visible = true
    end)

    -- Функция для закрытия GUI
    CloseButton.MouseButton1Click:Connect(function()
        guiClosed = true
        ScreenGui:Destroy()
    end)

    -- Добавление кнопки закрытия настроек
    local SettingsCloseButton = Instance.new("TextButton")
    SettingsCloseButton.Size = UDim2.new(0, 50, 0, 50)
    SettingsCloseButton.Position = UDim2.new(1, -60, 0, 10)
    SettingsCloseButton.Text = "X"
    SettingsCloseButton.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
    SettingsCloseButton.TextColor3 = Color3.fromRGB(255, 255, 255)
    SettingsCloseButton.BorderSizePixel = 0
    SettingsCloseButton.Parent = SettingsFrame
    SettingsCloseButton.MouseButton1Click:Connect(function()
        SettingsFrame.Visible = false
    end)

    -- Функция для переключения цензуры
    CensorshipToggle.MouseButton1Click:Connect(function()
        censorshipEnabled = not censorshipEnabled
        CensorshipToggle.Text = censorshipEnabled and "Цензура включена" or "Цензура отключена"
    end)

    -- Функция для смены языка
    LanguageButton.MouseButton1Click:Connect(function()
        currentLanguage = (currentLanguage == "ru") and "en" or "ru"
        MessageBox.PlaceholderText = (currentLanguage == "ru") and "Введите сообщение" or "Enter message"
        CountBox.PlaceholderText = (currentLanguage == "ru") and "Введите количество" or "Enter count"SendButton.Text = (currentLanguage == "ru") and "Отправить" or "Send"
        ToggleButton.Text = (currentLanguage == "ru") and "Свернуть/Развернуть" or "Collapse/Expand"
        SettingsButton.Text = (currentLanguage == "ru") and "Настройки" or "Settings"
        LanguageButton.Text = (currentLanguage == "ru") and "Изменить язык" or "Change Language"
        CensorshipToggle.Text = censorshipEnabled and 
            ((currentLanguage == "ru") and "Цензура включена" or "Censorship Enabled") or 
            ((currentLanguage == "ru") and "Цензура отключена" or "Censorship Disabled")
    end)

    print("Создан GUI для игрока: " .. player.Name)
end

-- Создание GUI для каждого игрока
Players.PlayerAdded:Connect(function(player)
    createGui(player)
end)

-- Создание GUI для уже существующих игроков
for _, player in ipairs(Players:GetPlayers()) do
    createGui(player)
end

-- Обеспечение непрерывной работы скрипта
RunService.RenderStepped:Connect(function()
    -- Здесь можно добавить любой код, который должен выполняться постоянно
end)

print("Скрипт загружен и работает.")
end})
    
    
Tab1:AddButton({"pixel chat spam💉🩸", function(Value)
print("Hello World!")loadstring(game:HttpGet("https://raw.githubusercontent.com/Lucasgbw/Chat-spam-pixel-hub-/refs/heads/main/2025%20new"))()
end})
