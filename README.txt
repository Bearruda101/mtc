-- abaixo estara a lib da nossa UI

local Lib = loadstring(game:HttpGet("https://raw.githubusercontent.com/7yhx/kwargs_Ui_Library/main/source.lua"))()

local UI = Lib:Create{
    Theme = "Dark", -- or any other theme
    Size = UDim2.new(0, 555, 0, 400) -- default
 }
 
 local Main = UI:Tab{
    Name = "inicio"
 }
 
 local Divider = Main:Divider{
    Name = "inicio shit"
 }
 
 local QuitDivider = Main:Divider{
    Name = "sair"
 }
 
local Players = game:GetService("Players")
local localPlayer = Players.LocalPlayer
local userInputService = game:GetService("UserInputService")
local radarGui = script.Parent -- A GUI onde você quer mostrar os jogadores

local isESPActive = false
local radarRange = 50

-- Função para mostrar o marcador de um jogador
local function createPlayerMarker(player)
    local character = player.Character
    if character and character:FindFirstChild("HumanoidRootPart") then
        local distance = (character.HumanoidRootPart.Position - localPlayer.Character.HumanoidRootPart.Position).Magnitude
        if distance <= radarRange then
            local playerMarker = Instance.new("Frame")
            playerMarker.Size = UDim2.new(0, 10, 0, 10)
            playerMarker.Position = UDim2.new(0, (character.HumanoidRootPart.Position.X - localPlayer.Character.HumanoidRootPart.Position.X) / radarRange * 100, 
                                               0, (character.HumanoidRootPart.Position.Z - localPlayer.Character.HumanoidRootPart.Position.Z) / radarRange * 100)
            playerMarker.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
            playerMarker.Parent = radarGui
        end
    end
end

-- Função para ativar/desativar o ESP com base nas teclas M/N
local function toggleESP(input)
    if input.KeyCode == Enum.KeyCode.M then
        isESPActive = true
        print("ESP Ativado!")
    elseif input.KeyCode == Enum.KeyCode.N then
        isESPActive = false
        print("ESP Desativado!")
    end
end

-- Conectar a função de input de teclado
userInputService.InputBegan:Connect(toggleESP)

-- Função para atualizar o radar
local function updateRadar()
    if isESPActive then
        -- Limpar o radar antes de atualizar
        for _, child in pairs(radarGui:GetChildren()) do
            if child:IsA("Frame") then
                child:Destroy()
            end
        end
        
        -- Verificar todos os jogadores
        for _, player in pairs(Players:GetPlayers()) do
            if player ~= localPlayer then
                createPlayerMarker(player)
            end
        end
    end
end

-- Atualizar radar periodicamente
while true do
    updateRadar()
    wait(1) -- Atualiza a cada segundo
end

