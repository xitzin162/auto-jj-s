-- AUTO J/'S - GLHUB Script para enviar no chat (CORRIGIDO E ACELERADO)
-- Cole este script em StarterPlayerScripts

local Players = game:GetService("Players")
local UserInputService = game:GetService("UserInputService")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local StarterGui = game:GetService("StarterGui")

local player = Players.LocalPlayer
local playerGui = player:WaitForChild("PlayerGui")

-- Variáveis de controle
local isRunning = false
local currentNumber = 0
local config = {
    comecarDe = 0,
    fazerAte = 3,
    finalPrefix = 1,
    pular = true,
    velocidade = 0.5 -- NOVO: Controle de velocidade (segundos entre mensagens)
}

-- Tabela de números por extenso
local numerosExtenso = {
    [0] = "ZERO", [1] = "UM", [2] = "DOIS", [3] = "TRÊS", [4] = "QUATRO",
    [5] = "CINCO", [6] = "SEIS", [7] = "SETE", [8] = "OITO", [9] = "NOVE",
    [10] = "DEZ", [11] = "ONZE", [12] = "DOZE", [13] = "TREZE", [14] = "QUATORZE",
    [15] = "QUINZE", [16] = "DEZESSEIS", [17] = "DEZESSETE", [18] = "DEZOITO", [19] = "DEZENOVE",
    [20] = "VINTE", [21] = "VINTE E UM", [22] = "VINTE E DOIS", [23] = "VINTE E TRÊS", [24] = "VINTE E QUATRO",
    [25] = "VINTE E CINCO", [26] = "VINTE E SEIS", [27] = "VINTE E SETE", [28] = "VINTE E OITO", [29] = "VINTE E NOVE",
    [30] = "TRINTA", [31] = "TRINTA E UM", [32] = "TRINTA E DOIS", [33] = "TRINTA E TRÊS", [34] = "TRINTA E QUATRO",
    [35] = "TRINTA E CINCO", [36] = "TRINTA E SEIS", [37] = "TRINTA E SETE", [38] = "TRINTA E OITO", [39] = "TRINTA E NOVE",
    [40] = "QUARENTA", [41] = "QUARENTA E UM", [42] = "QUARENTA E DOIS", [43] = "QUARENTA E TRÊS", [44] = "QUARENTA E QUATRO",
    [45] = "QUARENTA E CINCO", [46] = "QUARENTA E SEIS", [47] = "QUARENTA E SETE", [48] = "QUARENTA E OITO", [49] = "QUARENTA E NOVE",
    [50] = "CINQUENTA", [60] = "SESSENTA", [70] = "SETENTA", [80] = "OITENTA", [90] = "NOVENTA", [100] = "CEM"
}

-- Função para converter número em extenso
local function getNumeroExtenso(num)
    if numerosExtenso[num] then
        return numerosExtenso[num]
    elseif num > 50 and num < 100 then
        local dezena = math.floor(num / 10) * 10
        local unidade = num % 10
        if unidade == 0 then
            return numerosExtenso[dezena]
        else
            return numerosExtenso[dezena] .. " E " .. numerosExtenso[unidade]
        end
    else
        return tostring(num)
    end
end

-- FUNÇÃO PRINCIPAL PARA ENVIAR NO CHAT
local function enviarNoChat(mensagem)
    -- Método 1: Chat Events (Mais comum)
    local chatEvents = ReplicatedStorage:FindFirstChild("DefaultChatSystemChatEvents")
    if chatEvents then
        local sayEvent = chatEvents:FindFirstChild("SayMessageRequest")
        if sayEvent then
            sayEvent:FireServer(mensagem, "All")
            print("✅ Enviado via ChatEvents:", mensagem)
            return true
        end
    end
    
    -- Método 2: TextChatService (Novo sistema)
    local textChatService = game:GetService("TextChatService")
    local textChannels = textChatService:FindFirstChild("TextChannels")
    if textChannels then
        local rbxtGeneral = textChannels:FindFirstChild("RBXGeneral")
        if rbxtGeneral then
            rbxtGeneral:SendAsync(mensagem)
            print("✅ Enviado via TextChatService:", mensagem)
            return true
        end
    end
    
    -- Método 3: Chat Service direto
    local chatService = game:GetService("Chat")
    if player.Character and player.Character:FindFirstChild("Head") then
        chatService:Chat(player.Character.Head, mensagem, Enum.ChatColor.White)
        print("✅ Enviado via ChatService:", mensagem)
        return true
    end
    
    print("❌ Nenhum método de chat funcionou")
    return false
end

-- Criar GUI principal
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "AutoJsGlhub"
screenGui.Parent = playerGui
screenGui.ResetOnSpawn = false

-- Frame principal
local mainFrame = Instance.new("Frame")
mainFrame.Name = "MainFrame"
mainFrame.Size = UDim2.new(0, 280, 0, 450) -- Aumentado para caber novo controle
mainFrame.Position = UDim2.new(0.02, 0, 0.3, 0)
mainFrame.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
mainFrame.BorderSizePixel = 2
mainFrame.BorderColor3 = Color3.fromRGB(0, 255, 0)
mainFrame.Parent = screenGui

-- Título
local titleLabel = Instance.new("TextLabel")
titleLabel.Name = "TitleLabel"
titleLabel.Size = UDim2.new(1, 0, 0, 30)
titleLabel.Position = UDim2.new(0, 0, 0, 0)
titleLabel.BackgroundColor3 = Color3.fromRGB(0, 255, 0)
titleLabel.BorderSizePixel = 0
titleLabel.Text = "AUTO J/'S - GLHUB (RÁPIDO)"
titleLabel.TextColor3 = Color3.fromRGB(0, 0, 0)
titleLabel.TextScaled = true
titleLabel.Font = Enum.Font.Code
titleLabel.Parent = mainFrame

-- Função para criar linha de configuração
local function criarLinhaConfig(nome, posY, valorInicial)
    local label = Instance.new("TextLabel")
    label.Size = UDim2.new(0, 100, 0, 30)
    label.Position = UDim2.new(0, 10, 0, posY)
    label.BackgroundTransparency = 1
    label.Text = nome .. ":"
    label.TextColor3 = Color3.fromRGB(0, 255, 0)
    label.TextScaled = true
    label.Font = Enum.Font.Code
    label.TextXAlignment = Enum.TextXAlignment.Left
    label.Parent = mainFrame
    
    local textBox = Instance.new("TextBox")
    textBox.Size = UDim2.new(0, 80, 0, 25)
    textBox.Position = UDim2.new(0, 120, 0, posY + 2)
    textBox.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
    textBox.BorderColor3 = Color3.fromRGB(0, 255, 0)
    textBox.BorderSizePixel = 1
    textBox.Text = tostring(valorInicial)
    textBox.TextColor3 = Color3.fromRGB(0, 255, 0)
    textBox.TextScaled = true
    textBox.Font = Enum.Font.Code
    textBox.Parent = mainFrame
    
    return textBox
end

-- Criar campos de configuração
local comecarDeBox = criarLinhaConfig("Começar de", 50, 0)
local fazerAteBox = criarLinhaConfig("Fazer até", 90, 3)
local finalPrefixBox = criarLinhaConfig("Final Prefix", 130, 1)
local velocidadeBox = criarLinhaConfig("Velocidade", 170, 0.5) -- NOVO: Controle de velocidade

-- Campo Pular (Sim/Não)
local pularLabel = Instance.new("TextLabel")
pularLabel.Size = UDim2.new(0, 60, 0, 30)
pularLabel.Position = UDim2.new(0, 10, 0, 210)
pularLabel.BackgroundTransparency = 1
pularLabel.Text = "Pular:"
pularLabel.TextColor3 = Color3.fromRGB(0, 255, 0)
pularLabel.TextScaled = true
pularLabel.Font = Enum.Font.Code
pularLabel.TextXAlignment = Enum.TextXAlignment.Left
pularLabel.Parent = mainFrame

-- Botões Sim/Não para Pular
local simButton = Instance.new("TextButton")
simButton.Size = UDim2.new(0, 35, 0, 25)
simButton.Position = UDim2.new(0, 120, 0, 212)
simButton.BackgroundColor3 = Color3.fromRGB(0, 100, 0)
simButton.BorderColor3 = Color3.fromRGB(0, 255, 0)
simButton.BorderSizePixel = 1
simButton.Text = "sim"
simButton.TextColor3 = Color3.fromRGB(255, 255, 255)
simButton.TextScaled = true
simButton.Font = Enum.Font.Code
simButton.Parent = mainFrame

local naoButton = Instance.new("TextButton")
naoButton.Size = UDim2.new(0, 35, 0, 25)
naoButton.Position = UDim2.new(0, 165, 0, 212)
naoButton.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
naoButton.BorderColor3 = Color3.fromRGB(0, 255, 0)
naoButton.BorderSizePixel = 1
naoButton.Text = "não"
naoButton.TextColor3 = Color3.fromRGB(255, 255, 255)
naoButton.TextScaled = true
naoButton.Font = Enum.Font.Code
naoButton.Parent = mainFrame

-- Linha decorativa
local linha = Instance.new("TextLabel")
linha.Size = UDim2.new(1, -20, 0, 20)
linha.Position = UDim2.new(0, 10, 0, 250)
linha.BackgroundTransparency = 1
linha.Text = "----------//----------//----------//----------"
linha.TextColor3 = Color3.fromRGB(0, 255, 0)
linha.TextScaled = true
linha.Font = Enum.Font.Code
linha.Parent = mainFrame

-- Botão Iniciar
local iniciarButton = Instance.new("TextButton")
iniciarButton.Size = UDim2.new(0, 200, 0, 40)
iniciarButton.Position = UDim2.new(0, 40, 0, 290)
iniciarButton.BackgroundColor3 = Color3.fromRGB(0, 80, 0)
iniciarButton.BorderColor3 = Color3.fromRGB(0, 255, 0)
iniciarButton.BorderSizePixel = 2
iniciarButton.Text = "Iniciar"
iniciarButton.TextColor3 = Color3.fromRGB(255, 255, 255)
iniciarButton.TextScaled = true
iniciarButton.Font = Enum.Font.Code
iniciarButton.Parent = mainFrame

-- Botão Parar
local pararButton = Instance.new("TextButton")
pararButton.Size = UDim2.new(0, 200, 0, 40)
pararButton.Position = UDim2.new(0, 40, 0, 340)
pararButton.BackgroundColor3 = Color3.fromRGB(80, 0, 0)
pararButton.BorderColor3 = Color3.fromRGB(0, 255, 0)
pararButton.BorderSizePixel = 2
pararButton.Text = "parar"
pararButton.TextColor3 = Color3.fromRGB(255, 255, 255)
pararButton.TextScaled = true
pararButton.Font = Enum.Font.Code
pararButton.Parent = mainFrame

-- Status atual
local statusLabel = Instance.new("TextLabel")
statusLabel.Size = UDim2.new(1, -20, 0, 30)
statusLabel.Position = UDim2.new(0, 10, 0, 390)
statusLabel.BackgroundTransparency = 1
statusLabel.Text = "Status: Parado"
statusLabel.TextColor3 = Color3.fromRGB(0, 255, 0)
statusLabel.TextScaled = true
statusLabel.Font = Enum.Font.Code
statusLabel.Parent = mainFrame

-- Função para atualizar configurações
local function atualizarConfigs()
    config.comecarDe = tonumber(comecarDeBox.Text) or 0
    config.fazerAte = tonumber(fazerAteBox.Text) or 3
    config.finalPrefix = tonumber(finalPrefixBox.Text) or 1
    config.velocidade = tonumber(velocidadeBox.Text) or 0.5 -- NOVO: Atualizar velocidade
end

-- Função para alternar pular
simButton.MouseButton1Click:Connect(function()
    config.pular = true
    simButton.BackgroundColor3 = Color3.fromRGB(0, 100, 0)
    naoButton.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
end)

naoButton.MouseButton1Click:Connect(function()
    config.pular = false
    naoButton.BackgroundColor3 = Color3.fromRGB(100, 0, 0)
    simButton.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
end)

-- Função principal do loop (CORRIGIDA E ACELERADA)
local function executarLoop()
    atualizarConfigs()
    currentNumber = config.comecarDe
    isRunning = true
    
    statusLabel.Text = "Status: Executando..."
    print("🚀 Iniciando AUTO J/'S - GLHUB (RÁPIDO)")
    print("📊 Configurações:")
    print("   - Começar de:", config.comecarDe)
    print("   - Fazer até:", config.fazerAte)
    print("   - Final Prefix:", config.finalPrefix)
    print("   - Pular:", config.pular and "Sim" or "Não")
    print("   - Velocidade:", config.velocidade, "segundos") -- NOVO: Log de velocidade
    
    spawn(function()
        while isRunning and currentNumber <= config.fazerAte do
            -- Verificar se deve pular este número
            if config.pular and currentNumber % 2 == 0 then
                print("⏭️ Pulando número:", currentNumber)
                statusLabel.Text = "Status: Pulando " .. getNumeroExtenso(currentNumber)
            else
                local numeroTexto = getNumeroExtenso(currentNumber)
                local mensagem = numeroTexto .. " !"
                
                -- Adicionar prefix final se configurado
                if config.finalPrefix > 1 then
                    for i = 2, config.finalPrefix do
                        mensagem = mensagem .. " !"
                    end
                end
                
                -- ENVIAR NO CHAT
                enviarNoChat(mensagem)
                statusLabel.Text = "Status: Enviando " .. numeroTexto
            end
            
            -- Incrementar número (SEMPRE incrementa 1, independente da opção pular)
            currentNumber = currentNumber + 1
            
            wait(config.velocidade) -- ALTERADO: Usar velocidade configurada em vez de 2 segundos fixos
        end
        
        if isRunning then
            statusLabel.Text = "Status: Concluído! ✅"
            print("✅ AUTO J/'S concluído!")
        end
        isRunning = false
    end)
end

-- Função para parar
local function pararLoop()
    isRunning = false
    statusLabel.Text = "Status: Parado"
    print("⏹️ AUTO J/'S parado pelo usuário")
end

-- Conectar botões
iniciarButton.MouseButton1Click:Connect(function()
    if not isRunning then
        executarLoop()
    end
end)

pararButton.MouseButton1Click:Connect(function()
    pararLoop()
end)

-- Tornar a GUI arrastável
local dragging = false
local dragStart = nil
local startPos = nil

local function updateInput(input)
    local delta = input.Position - dragStart
    mainFrame.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
end

titleLabel.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        dragging = true
        dragStart = input.Position
        startPos = mainFrame.Position
        
        input.Changed:Connect(function()
            if input.UserInputState == Enum.UserInputState.End then
                dragging = false
            end
        end)
    end
end)

titleLabel.InputChanged:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseMovement and dragging then
        updateInput(input)
    end
end)

print("🎯 AUTO J/'S - GLHUB (RÁPIDO) carregado!")
print("📝 Abra o console (F9) para ver os logs")
print("💬 Configure os valores e clique 'Iniciar'")
print("⚡ Use valores menores em 'Velocidade' para escrever mais rápido")
