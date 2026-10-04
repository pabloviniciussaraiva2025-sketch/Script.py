-- Criando a Interface Principal (GUI)
local ScreenGui = Instance.new("ScreenGui")
local MainFrame = Instance.new("Frame")
local Title = Instance.new("TextLabel")
local UIListLayout = Instance.new("UIListLayout")

-- Configurando a Tela
ScreenGui.Name = "MarretaoButtonResizer"
ScreenGui.Parent = game:GetService("Players").LocalPlayer:WaitForChild("PlayerGui")
ScreenGui.ResetOnSpawn = false

-- Configurando o Menu Preto
MainFrame.Name = "MainFrame"
MainFrame.Parent = ScreenGui
MainFrame.BackgroundColor3 = Color3.fromRGB(20, 20, 20) -- Preto Escuro
MainFrame.BorderSizePixel = 0
MainFrame.Position = UDim2.new(0.05, 0, 0.3, 0)
MainFrame.Size = UDim2.new(0, 250, 0, 220)
MainFrame.Active = true
MainFrame.Draggable = true -- Permite arrastar o menu pela tela

-- Título do Menu
Title.Name = "Title"
Title.Parent = MainFrame
Title.BackgroundTransparency = 1
Title.Size = UDim2.new(1, 0, 0, 40)
Title.Font = Enum.Font.SourceSansBold
Title.Text = "Ajustar Botões (Marretão)"
Title.TextColor3 = Color3.fromRGB(255, 255, 255)
Title.TextSize = 18

-- Organização dos botões em lista
UIListLayout.Parent = MainFrame
UIListLayout.SortOrder = Enum.SortOrder.LayoutOrder
UIListLayout.Padding = UDim.new(0, 10)
UIListLayout.HorizontalAlignment = Enum.HorizontalAlignment.Center

-- Função para encontrar os botões reais do jogo Marretão (Flee the Facility)
local function getGameButton(buttonName)
    local localPlayer = game:GetService("Players").LocalPlayer
    if localPlayer then
        -- Tenta localizar na interface padrão de celulares/controles do jogo
        local mobileGui = localPlayer:FindFirstChild("PlayerGui") and localPlayer.PlayerGui:FindFirstChild("ScreenGui")
        if mobileGui then
            -- Procura por botões com o texto ou nome correspondente
            for _, v in pairs(mobileGui:GetDescendants()) do
                if v:IsA("TextButton") or v:IsA("ImageButton") then
                    if v.Name:lower() == buttonName:lower() or (v:IsA("TextButton") and v.Text:lower() == buttonName:lower()) then
                        return v
                    end
                end
            end
        end
    end
    return nil
end

-- Função para criar a linha com o Ícone e a Barra (Slider)
local function createSlider(buttonLetter)
    local Container = Instance.new("Frame")
    local Label = Instance.new("TextLabel")
    local SliderBackground = Instance.new("Frame")
    local SliderBar = Instance.new("Frame")
    local SliderButton = Instance.new("TextButton")
    local ValueLabel = Instance.new("TextLabel")

    Container.Name = buttonLetter .. "_Container"
    Container.Parent = MainFrame
    Container.BackgroundTransparency = 1
    Container.Size = UDim2.new(0.9, 0, 0, 40)

    -- Ícone / Texto do Botão (C, E ou Q)
    Label.Name = "ButtonLabel"
    Label.Parent = Container
    Label.BackgroundTransparency = 1
    Label.Position = UDim2.new(0, 0, 0, 0)
    Label.Size = UDim2.new(0, 30, 1, 0)
    Label.Font = Enum.Font.SourceSansBold
    Label.Text = buttonLetter
    Label.TextColor3 = Color3.fromRGB(255, 255, 255)
    Label.TextSize = 22

    -- Fundo da Barra de 0 a 100
    SliderBackground.Name = "SliderBackground"
    SliderBackground.Parent = Container
    SliderBackground.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
    SliderBackground.BorderSizePixel = 0
    SliderBackground.Position = UDim2.new(0, 40, 0.4, 0)
    SliderBackground.Size = UDim2.new(0, 130, 0, 8)

    -- Preenchimento da Barra
    SliderBar.Name = "SliderBar"
    SliderBar.Parent = SliderBackground
    SliderBar.BackgroundColor3 = Color3.fromRGB(0, 150, 255) -- Azul para destacar
    SliderBar.BorderSizePixel = 0
    SliderBar.Size = UDim2.new(0.5, 0, 1, 0) -- Começa no meio (50)

    -- Botão arrastável do Slider
    SliderButton.Name = "SliderButton"
    SliderButton.Parent = SliderBackground
    SliderButton.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
    SliderButton.Position = UDim2.new(0.5, -5, 0.5, -7)
    SliderButton.Size = UDim2.new(0, 10, 0, 14)
    SliderButton.Text = ""

    -- Texto mostrando o valor atual (0 a 100)
    ValueLabel.Name = "ValueLabel"
    ValueLabel.Parent = Container
    ValueLabel.BackgroundTransparency = 1
    ValueLabel.Position = UDim2.new(0, 180, 0, 0)
    ValueLabel.Size = UDim2.new(0, 40, 1, 0)
    ValueLabel.Font = Enum.Font.SourceSans
    ValueLabel.Text = "50"
    ValueLabel.TextColor3 = Color3.fromRGB(200, 200, 200)
    ValueLabel.TextSize = 16

    -- Guardar o tamanho original do botão do jogo quando ele for encontrado
    local targetButton = getGameButton(buttonLetter)
    local originalSize = targetButton and targetButton.Size or UDim2.new(0, 50, 0, 50)

    -- Lógica do Slider (Arrastar de 0 a 100)
    local dragging = false

    local function updateSlider(input)
        local originalX = SliderBackground.AbsolutePosition.X
        local width = SliderBackground.AbsoluteSize.X
        local percentage = math.clamp((input.Position.X - originalX) / width, 0, 1)
        
        SliderBar.Size = UDim2.new(percentage, 0, 1, 0)
        SliderButton.Position = UDim2.new(percentage, -5, 0.5, -7)
        
        -- Valor de 0 a 100
        local finalValue = math.round(percentage * 100)
        ValueLabel.Text = tostring(finalValue)

        -- Atualiza o tamanho do botão do Marretão proporcionalmente
        targetButton = targetButton or getGameButton(buttonLetter)
        if targetButton then
            -- Multiplicador baseado no valor de 0 a 100 (onde 50 é o tamanho normal)
            local scaleMultiplier = finalValue / 50
            if scaleMultiplier == 0 then scaleMultiplier = 0.01 end -- Evita sumir totalmente
            
            targetButton.Size = UDim2.new(
                originalSize.X.Scale, originalSize.X.Offset * scaleMultiplier,
                originalSize.Y.Scale, originalSize.Y.Offset * scaleMultiplier
            )
        end
    end

    SliderButton.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
            dragging = true
        end
    end)

    game:GetService("UserInputService").InputChanged:Connect(function(input)
        if dragging and (input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch) then
            updateSlider(input)
        end
    end)

    game:GetService("UserInputService").InputEnded:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
            dragging = false
        end
    end)
end

-- Criando os sliders específicos solicitados
createSlider("E")
createSlider("C")
createSlider("Q")
