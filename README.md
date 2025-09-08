-- Script Hub usando Orion Library
-- Carrega a biblioteca Orion
local ServerUi = loadstring(game:HttpGet("https://raw.githubusercontent.com/ServerSad/UiLib/refs/heads/main/Lib/uilib.lua"))()

-- Configurar cargos
ServerUi:MakeRoles({
    Admin = {
        Color = "#FFFFFF",
        Users = {123456789}
    },
    Player = {
        Color = "#FFFFFF",
        Users = "everyone"
    }
})

-- Criar janela principal
local Window = ServerUi:MakeWindow({
    Name = "Gusta Hub        ",
    SaveConfig = false,
    ConfigFolder = "GustaHub            ",
    IntroText = "Bem-vindo ao Gusta Hub!",
    IntroEnabled = true,
    ShowIcon = true,
    Icon = "rbxassetid://4483362458",
    IntroIcon = "rbxassetid://4483362458",
    KeySystem = false -- Sistema de chave desabilitado
})

-- Criar aba para scripts
local ScriptsTab = Window:MakeTab({
    Name = "Scripts",
    Icon = "rbxassetid://4483362458"
})

-- Função para notificar sucesso
local function NotifySuccess(hubName)
    Window:Notify({
        Title = "Sucesso!",
        Content = hubName .. " foi executado com sucesso!",
        Duration = 3
    })
end

-- Função para notificar erro
local function NotifyError(hubName, errorMsg)
    Window:Notify({
        Title = "Erro!",
        Content = "Falha ao executar " .. hubName .. ": " .. tostring(errorMsg),
        Duration = 5
    })
end

-- Botão para Fps Killer
ScriptsTab:AddButton({
    Name = "Fps Killer - PRECISA DE AURA!",
    Callback = function()
        local success, err = pcall(function()
            -- GUSTA FPS KILLER Script Inline
            local Players = game:GetService("Players")
            local TweenService = game:GetService("TweenService")
            local UserInputService = game:GetService("UserInputService")
            local RunService = game:GetService("RunService")

            local player = Players.LocalPlayer
            local playerGui = player:WaitForChild("PlayerGui")

            -- ========== REMOÇÃO DE ACESSÓRIOS ==========
            local function removeAllAccessories()
                local character = player.Character
                if not character then return end
                
                for _, item in ipairs(character:GetChildren()) do
                    if item:IsA("Accessory") or item:IsA("LayeredClothing") or 
                       item:IsA("Shirt") or item:IsA("ShirtGraphic") or 
                       item:IsA("Pants") or item:IsA("BodyColors") or 
                       item:IsA("CharacterMesh") then
                        pcall(function() item:Destroy() end)
                    end
                end
            end

            player.CharacterAdded:Connect(function()
                task.wait(0.2)
                removeAllAccessories()
            end)

            if player.Character then
                task.defer(removeAllAccessories)
            end

            -- ========== FPS DEVOURER ==========
            local FPSDevourer = {
                running = false,
                clickCount = 0
            }

            local function equipTool()
                local character = player.Character
                local backpack = player:FindFirstChild("Backpack")
                if not character or not backpack then return false end
                
                local tool = backpack:FindFirstChild("Tung Bat")
                if tool then 
                    tool.Parent = character 
                    FPSDevourer.clickCount = FPSDevourer.clickCount + 1
                    return true 
                end
                return false
            end

            local function unequipTool()
                local character = player.Character
                local backpack = player:FindFirstChild("Backpack")
                if not character or not backpack then return false end
                
                local tool = character:FindFirstChild("Tung Bat")
                if tool then tool.Parent = backpack return true end
                return false
            end

            function FPSDevourer:Start()
                if self.running then return end
                self.running = true
                
                task.spawn(function()
                    while self.running do
                        equipTool()
                        task.wait(0.035)
                        unequipTool()
                        task.wait(0.035)
                    end
                end)
            end

            function FPSDevourer:Stop()
                self.running = false
                unequipTool()
            end

            player.CharacterAdded:Connect(function()
                FPSDevourer:Stop()
                FPSDevourer.clickCount = 0
            end)

            -- Remove GUI antiga
            local oldGui = playerGui:FindFirstChild("GustaFpsKillerPanel")
            if oldGui then oldGui:Destroy() end

            -- ========== CRIAÇÃO DA GUI ESTILO MKZ ==========
            local gui = Instance.new("ScreenGui")
            gui.Name = "GustaFpsKillerPanel"
            gui.ResetOnSpawn = false
            gui.Parent = playerGui

            -- Container principal
            local mainFrame = Instance.new("Frame")
            mainFrame.Name = "MainFrame"
            mainFrame.Size = UDim2.new(0, 300, 0, 160)
            mainFrame.Position = UDim2.new(0.5, -150, 0.5, -80)
            mainFrame.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
            mainFrame.BorderSizePixel = 0
            mainFrame.Active = true
            mainFrame.Parent = gui

            -- Cantos arredondados
            Instance.new("UICorner", mainFrame).CornerRadius = UDim.new(0, 12)

            -- Borda vermelha
            local redBorder = Instance.new("Frame")
            redBorder.Name = "RedBorder"
            redBorder.Size = UDim2.new(1, 6, 1, 6)
            redBorder.Position = UDim2.new(0, -3, 0, -3)
            redBorder.BackgroundColor3 = Color3.fromRGB(220, 50, 50)
            redBorder.BorderSizePixel = 0
            redBorder.ZIndex = mainFrame.ZIndex - 1
            redBorder.Parent = mainFrame
            Instance.new("UICorner", redBorder).CornerRadius = UDim.new(0, 15)

            -- Título principal
            local titleMain = Instance.new("TextLabel")
            titleMain.Name = "TitleMain"
            titleMain.Size = UDim2.new(1, -20, 0, 30)
            titleMain.Position = UDim2.new(0, 10, 0, 10)
            titleMain.BackgroundTransparency = 1
            titleMain.Text = "GUSTA FPS KILLER"
            titleMain.Font = Enum.Font.GothamBlack
            titleMain.TextSize = 18
            titleMain.TextColor3 = Color3.fromRGB(220, 50, 50)
            titleMain.TextXAlignment = Enum.TextXAlignment.Center
            titleMain.TextYAlignment = Enum.TextYAlignment.Center
            titleMain.Parent = mainFrame

            -- Subtítulo
            local subtitle = Instance.new("TextLabel")
            subtitle.Name = "Subtitle"
            subtitle.Size = UDim2.new(1, -20, 0, 20)
            subtitle.Position = UDim2.new(0, 10, 0, 35)
            subtitle.BackgroundTransparency = 1
            subtitle.Text = "FPS Killer - By Gusta"
            subtitle.Font = Enum.Font.Gotham
            subtitle.TextSize = 11
            subtitle.TextColor3 = Color3.fromRGB(180, 180, 180)
            subtitle.TextXAlignment = Enum.TextXAlignment.Center
            subtitle.TextYAlignment = Enum.TextYAlignment.Center
            subtitle.Parent = mainFrame

            -- Contador de cliques
            local clickCounter = Instance.new("TextLabel")
            clickCounter.Name = "ClickCounter"
            clickCounter.Size = UDim2.new(1, -20, 0, 18)
            clickCounter.Position = UDim2.new(0, 10, 0, 60)
            clickCounter.BackgroundTransparency = 1
            clickCounter.Text = "Cliques: 0"
            clickCounter.Font = Enum.Font.GothamMedium
            clickCounter.TextSize = 12
            clickCounter.TextColor3 = Color3.fromRGB(200, 200, 200)
            clickCounter.TextXAlignment = Enum.TextXAlignment.Center
            clickCounter.TextYAlignment = Enum.TextYAlignment.Center
            clickCounter.Parent = mainFrame

            -- Botão START KILLER
            local startButton = Instance.new("TextButton")
            startButton.Name = "StartButton"
            startButton.Size = UDim2.new(1, -40, 0, 30)
            startButton.Position = UDim2.new(0, 20, 0, 85)
            startButton.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
            startButton.BorderSizePixel = 0
            startButton.Text = "START FPS KILLER"
            startButton.Font = Enum.Font.GothamBold
            startButton.TextSize = 14
            startButton.TextColor3 = Color3.fromRGB(220, 50, 50)
            startButton.AutoButtonColor = false
            startButton.Parent = mainFrame
            Instance.new("UICorner", startButton).CornerRadius = UDim.new(0, 6)

            -- Botão STOP KILLER
            local stopButton = Instance.new("TextButton")
            stopButton.Name = "StopButton"
            stopButton.Size = UDim2.new(1, -40, 0, 30)
            stopButton.Position = UDim2.new(0, 20, 0, 120)
            stopButton.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
            stopButton.BorderSizePixel = 0
            stopButton.Text = "STOP FPS KILLER"
            stopButton.Font = Enum.Font.GothamBold
            stopButton.TextSize = 14
            stopButton.TextColor3 = Color3.fromRGB(220, 50, 50)
            stopButton.AutoButtonColor = false
            stopButton.Parent = mainFrame
            Instance.new("UICorner", stopButton).CornerRadius = UDim.new(0, 6)

            -- ========== SISTEMA DE DRAG ==========
            do
                local dragging = false
                local dragStart, startPos
                
                mainFrame.InputBegan:Connect(function(input)
                    if input.UserInputType == Enum.UserInputType.MouseButton1 or 
                       input.UserInputType == Enum.UserInputType.Touch then
                        dragging = true
                        dragStart = input.Position
                        startPos = mainFrame.Position
                    end
                end)
                
                UserInputService.InputChanged:Connect(function(input)
                    if dragging and (input.UserInputType == Enum.UserInputType.MouseMovement or 
                       input.UserInputType == Enum.UserInputType.Touch) then
                        local delta = input.Position - dragStart
                        mainFrame.Position = UDim2.new(
                            startPos.X.Scale, 
                            startPos.X.Offset + delta.X, 
                            startPos.Y.Scale, 
                            startPos.Y.Offset + delta.Y
                        )
                    end
                end)
                
                UserInputService.InputEnded:Connect(function(input)
                    if input.UserInputType == Enum.UserInputType.MouseButton1 or 
                       input.UserInputType == Enum.UserInputType.Touch then
                        dragging = false
                    end
                end)
            end

            -- ========== LÓGICA PRINCIPAL ==========
            local isActive = false

            local function updateStatus()
                -- Atualizar contador
                clickCounter.Text = "Cliques: " .. tostring(FPSDevourer.clickCount)
                
                -- Atualizar visual dos botões (mantém as cores RGB animadas)
                if isActive then
                    startButton.BackgroundColor3 = Color3.fromRGB(60, 25, 25)
                    stopButton.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
                else
                    startButton.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
                    stopButton.BackgroundColor3 = Color3.fromRGB(60, 25, 25)
                end
            end

            -- Click do botão START
            startButton.MouseButton1Click:Connect(function()
                if not isActive then
                    isActive = true
                    FPSDevourer:Start()
                    updateStatus()
                    
                    -- Animação de clique
                    TweenService:Create(startButton, TweenInfo.new(0.1), 
                        {Size = UDim2.new(1, -42, 0, 28)}):Play()
                    task.wait(0.1)
                    TweenService:Create(startButton, TweenInfo.new(0.1), 
                        {Size = UDim2.new(1, -40, 0, 30)}):Play()
                end
            end)

            -- Click do botão STOP
            stopButton.MouseButton1Click:Connect(function()
                if isActive then
                    isActive = false
                    FPSDevourer:Stop()
                    updateStatus()
                    
                    -- Animação de clique
                    TweenService:Create(stopButton, TweenInfo.new(0.1), 
                        {Size = UDim2.new(1, -42, 0, 28)}):Play()
                    task.wait(0.1)
                    TweenService:Create(stopButton, TweenInfo.new(0.1), 
                        {Size = UDim2.new(1, -40, 0, 30)}):Play()
                end
            end)

            -- Hover effects
            startButton.MouseEnter:Connect(function()
                if not isActive then
                    startButton.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
                end
            end)

            startButton.MouseLeave:Connect(function()
                if not isActive then
                    startButton.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
                end
            end)

            stopButton.MouseEnter:Connect(function()
                if not isActive then
                    stopButton.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
                end
            end)

            stopButton.MouseLeave:Connect(function()
                if not isActive then
                    stopButton.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
                end
            end)

            -- Atualização do contador em tempo real
            RunService.Heartbeat:Connect(function()
                if FPSDevourer.running then
                    clickCounter.Text = "Cliques: " .. tostring(FPSDevourer.clickCount)
                end
            end)

            -- Reset ao trocar personagem
            player.CharacterAdded:Connect(function()
                isActive = false
                FPSDevourer:Stop()
                FPSDevourer.clickCount = 0
                task.wait(0.3)
                removeAllAccessories()
                updateStatus()
            end)

            -- Inicialização
            updateStatus()
        end)
        
        if success then
            NotifySuccess("Fps Killer")
            print("Fps Killer executado com sucesso!")
        else
            NotifyError("Fps Killer", err)
            print("Erro ao executar Fps Killer:", err)
        end
    end,
})

-- Botão para Chilli Hub
ScriptsTab:AddButton({
    Name = "Chilli Hub",
    Callback = function()
        local success, err = pcall(function()
            loadstring(game:HttpGet("https://raw.githubusercontent.com/tienkhanh1/spicy/main/Chilli.lua"))()
        end)
        
        if success then
            NotifySuccess("Chilli Hub")
            print("Chilli Hub executado com sucesso!")
        else
            NotifyError("Chilli Hub", err)
            print("Erro ao executar Chilli Hub:", err)
        end
    end,
})

-- Botão para Ugly Hub
ScriptsTab:AddButton({
    Name = "Ugly Hub",
    Callback = function()
        local success, err = pcall(function()
            loadstring(game:HttpGet("https://api.luarmor.net/files/v3/loaders/53325754de16c11fbf8bf78101c1c881.lua"))()
        end)
        
        if success then
            NotifySuccess("Ugly Hub")
            print("Ugly Hub executado com sucesso!")
        else
            NotifyError("Ugly Hub", err)
            print("Erro ao executar Ugly Hub:", err)
        end
    end,
})

-- Botão para Ugly Pet Finder
ScriptsTab:AddButton({
    Name = "Ugly Pet Finder",
    Callback = function()
        local success, err = pcall(function()
            loadstring(game:HttpGet("https://api.luarmor.net/files/v3/loaders/77d72e34c893b67ea49b8d62d1a18485.lua"))()
        end)
        
        if success then
            NotifySuccess("Ugly Pet Finder")
            print("Ugly Pet Finder executado com sucesso!")
        else
            NotifyError("Ugly Pet Finder", err)
            print("Erro ao executar Ugly Pet Finder:", err)
        end
    end,
})

-- Botão para NSLX x MKZ Pet Finder
ScriptsTab:AddButton({
    Name = "NSLX x MKZ Pet Finder",
    Callback = function()
        local success, err = pcall(function()
            getgenv().NSLXKEY = "FREE"
            loadstring(game:HttpGet("https://raw.githubusercontent.com/murilolol/nslx-autojoiner/refs/heads/main/nslx%20x%20mkz.lua"))()
        end)
        
        if success then
            NotifySuccess("NSLX x MKZ Pet Finder")
            print("NSLX x MKZ Pet Finder executado com sucesso!")
        else
            NotifyError("NSLX x MKZ Pet Finder", err)
            print("Erro ao executar NSLX x MKZ Pet Finder:", err)
        end
    end,
})

-- Botão para Kurd Hub
ScriptsTab:AddButton({
    Name = "Kurd Hub",
    Callback = function()
        local success, err = pcall(function()
            loadstring(game:HttpGet("https://raw.githubusercontent.com/Ninja10908/S4/refs/heads/main/Kurdhub"))()
        end)
        
        if success then
            NotifySuccess("Kurd Hub")
            print("Kurd Hub executado com sucesso!")
        else
            NotifyError("Kurd Hub", err)
            print("Erro ao executar Kurd Hub:", err)
        end
    end,
})

-- Botão para Miranda Hub
ScriptsTab:AddButton({
    Name = "Miranda Hub",
    Callback = function()
        local success, err = pcall(function()
            loadstring(game:HttpGet("https://pastefy.app/JJVhs3rK/raw"))()
        end)
        
        if success then
            NotifySuccess("Miranda Hub")
            print("Miranda Hub executado com sucesso!")
        else
            NotifyError("Miranda Hub", err)
            print("Erro ao executar Miranda Hub:", err)
        end
    end,
})

-- Botão para Lennon Hub
ScriptsTab:AddButton({
    Name = "Lennon Hub",
    Callback = function()
        local success, err = pcall(function()
            loadstring(game:HttpGet("https://pastefy.app/MJw2J4T6/raw"))()
        end)
        
        if success then
            NotifySuccess("Lennon Hub")
            print("Lennon Hub executado com sucesso!")
        else
            NotifyError("Lennon Hub", err)
            print("Erro ao executar Lennon Hub:", err)
        end
    end,
})

-- Inicialização completa
print("Gusta Hub (Orion) carregado com sucesso!")
