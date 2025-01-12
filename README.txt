-- abaixo estara a lib da nossa UI

local Lib = loadstring(game:HttpGet("https://raw.githubusercontent.com/7yhx/kwargs_Ui_Library/main/source.lua"))()


-- Variáveis
local player = game.Players.LocalPlayer
local mouse = player:GetMouse()
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

-- Função para ativar/desativar o pulo infinito
local function toggleInfiniteJump()
    isInfiniteJumpActive = not isInfiniteJumpActive
    
    -- Alterar cor do botão conforme o estado
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

