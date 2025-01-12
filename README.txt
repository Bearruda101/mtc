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
 
-- Configurações iniciais
local jogador = game.Players.LocalPlayer
local equipe = jogador.Team
local ativado = true -- Variável para ativar/desativar o script
local distMax = 100 -- Distância máxima para marcar inimigos
local fovColor = Color3.fromRGB(255, 0, 0) -- Cor para os inimigos (vermelho)
local minhaEquipeColor = Color3.fromRGB(0, 0, 255) -- Cor para minha equipe (azul)

-- Função para desenhar as setas de direção
function desenharSeta(jogadorInimigo)
    local screenPos = workspace.CurrentCamera:WorldToScreenPoint(jogadorInimigo.Character.HumanoidRootPart.Position)
    
    if screenPos.Z > 0 and (screenPos - Vector3.new(0.5, 0.5, 0)).Magnitude < 1 then
        local seta = Instance.new("Frame")
        seta.Size = UDim2.new(0, 10, 0, 10)
        seta.Position = UDim2.new(0, screenPos.X, 0, screenPos.Y)
        seta.BackgroundColor3 = fovColor
        seta.AnchorPoint = Vector2.new(0.5, 0.5)
        seta.Parent = game.Players.LocalPlayer.PlayerGui
    end
end

-- Função para verificar jogadores da mesma equipe e inimigos
function verificarJogadores()
    for _, player in pairs(game.Players:GetPlayers()) do
        if player ~= jogador then
            local dist = (jogador.Character.HumanoidRootPart.Position - player.Character.HumanoidRootPart.Position).Magnitude

            -- Se a distância for maior que o limite, ignora
            if dist > distMax then
                continue
            end
            
            -- Verifica se é da mesma equipe ou inimiga
            if player.Team == equipe then
                -- Marca com a cor azul
                if player.Character:FindFirstChild("HumanoidRootPart") then
                    desenharSeta(player)
                end
            else
                -- Marca com a cor vermelha
                if player.Character:FindFirstChild("HumanoidRootPart") then
                    desenharSeta(player)
                end
            end
        end
    end
end

-- Função para ativar/desativar o script
function alternarAtivacao()
    ativado = not ativado
    if ativado then
        print("Script ativado.")
    else
        print("Script desativado.")
        for _, obj in pairs(game.Players.LocalPlayer.PlayerGui:GetChildren()) do
            if obj:IsA("Frame") then
                obj:Destroy() -- Remove as setas da tela
            end
        end
    end
end

-- Conectando a tecla para ativar/desativar o script (exemplo: tecla 'M')
game:GetService("UserInputService").InputBegan:Connect(function(input, gameProcessed)
    if gameProcessed then return end
    if input.KeyCode == Enum.KeyCode.M then
        alternarAtivacao()
    end
end)

-- Laço principal
while true do
    if ativado then
        verificarJogadores()
    end
    wait(1)
end
