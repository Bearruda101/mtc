-- abaixo estara a lib da nossa UI

local Lib = loadstring(game:HttpGet("https://raw.githubusercontent.com/7yhx/kwargs_Ui_Library/main/source.lua"))()

-- Variáveis
local player = game.Players.LocalPlayer
local userInputService = game:GetService("UserInputService")

-- Referências para a UI
local screenGui = script.Parent
local frame = screenGui:WaitForChild("Frame")
local toggleButton = frame:WaitForChild("TextButton")

-- Estado inicial
local isUiVisible = true
local isInfiniteJumpActive = false

-- Função para esconder/exibir a UI ao pressionar Ctrl
local function toggleUiVisibility(input)
    if input.UserInputType == Enum.UserInputType.Keyboard and input.KeyCode == Enum.KeyCode.LeftControl then
        isUiVisible = not isUiVisible
        frame.Visible = isUiVisible
    end
end

-- Função para alternar o botão e ativar/desativar o pulo infinito
local function toggleInfiniteJump()
    isInfiniteJumpActive = not isInfiniteJumpActive

    -- Alterar a cor do botão conforme o estado
    if isInfiniteJumpActive then
        toggleButton.BackgroundColor3 = Color3.fromRGB(0, 0, 255)  -- Azul (Ativado)
        toggleButton.Text = "Pulo Infinito Ativado"
    else
        toggleButton.BackgroundColor3 = Color3.fromRGB(169, 169, 169)  -- Cinza (Desativado)
        toggleButton.Text = "Pulo Infinito Desativado"
    end
end

-- Função para implementar o pulo infinito
local function infiniteJump()
    while isInfiniteJumpActive do
        if player.Character and player.Character:FindFirstChild("Humanoid") then
            local humanoid = player.Character.Humanoid
            humanoid.Jump = true
            wait(0.1) -- Aguarda um pouco antes de pular novamente
        end
        wait(0.1)
    end
end

-- Conectar a função de pressionar a tecla Ctrl
userInputService.InputBegan:Connect(toggleUiVisibility)

-- Ação do botão
toggleButton.MouseButton1Click:Connect(function()
    toggleInfiniteJump() -- Alterna o estado do pulo infinito
    if isInfiniteJumpActive then
        -- Começa a pular infinitamente
        infiniteJump()
    end
end)

-- Definir o tamanho e formato da UI (Frame e Botão)
frame.Size = UDim2.new(0, 200, 0, 200)  -- Tamanho quadrado da UI
frame.Position = UDim2.new(0.5, -100, 0.5, -100)  -- Centraliza a UI na tela
frame.BackgroundColor3 = Color3.fromRGB(50, 50, 50)  -- Cor de fundo da UI

toggleButton.Size = UDim2.new(0, 180, 0, 50)  -- Tamanho do botão
toggleButton.Position = UDim2.new(0, 10, 0, 75)  -- Posição do botão dentro da UI
toggleButton.BackgroundColor3 = Color3.fromRGB(169, 169, 169)  -- Cor cinza (desativado)
toggleButton.Text = "Pulo Infinito Desativado"
toggleButton.TextColor3 = Color3.fromRGB(255, 255, 255)  -- Cor do texto
toggleButton.Font = Enum.Font.SourceSans
toggleButton.TextSize = 20
