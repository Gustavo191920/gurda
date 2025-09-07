-- Script Hub usando Array Field UI Library
-- Carrega a biblioteca Array Field UI
local ArrayField = loadstring(game:HttpGet("https://raw.githubusercontent.com/Hosvile/Refinement/main/MC%3AArrayfield%20Library"))()

-- Cria a janela principal
local Window = ArrayField:CreateWindow({
    Name = "Gusta Hub",
    LoadingTitle = "Carregando...",
    LoadingSubtitle = "by Gusta",
    ConfigurationSaving = {
        Enabled = false,
        FolderName = "ScriptHub",
        FileName = "Config"
    },
    Discord = {
        Enabled = false,
        Invite = "",
        RememberJoins = false
    },
    KeySystem = false,
    KeySettings = {
        Title = "Gusta Hub",
        Subtitle = "Key System",
        Note = "Sem sistema de key",
        FileName = "ScriptHubKey",
        SaveKey = false,
        GrabKeyFromSite = false,
        Key = ""
    }
})

-- Cria uma aba para os scripts
local Tab = Window:CreateTab("Scripts", 4483362458) -- Ícone de script

-- Adiciona notificação de sucesso
local function NotifySuccess(hubName)
    ArrayField:Notify({
        Title = "Sucesso!",
        Content = hubName .. " foi executado com sucesso!",
        Duration = 3,
        Image = 4483362458,
        Actions = {
            Ignore = {
                Name = "Ok",
                Callback = function()
                    print("Notificação fechada")
                end
            }
        }
    })
end

-- Adiciona notificação de erro
local function NotifyError(hubName, errorMsg)
    ArrayField:Notify({
        Title = "Erro!",
        Content = "Falha ao executar " .. hubName .. ": " .. tostring(errorMsg),
        Duration = 5,
        Image = 4483362458,
        Actions = {
            Ignore = {
                Name = "Ok",
                Callback = function()
                    print("Notificação de erro fechada")
                end
            }
        }
    })
end

-- Botão para Chilli Hub
Tab:CreateButton({
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
Tab:CreateButton({
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

-- Botão para Miranda Hub
Tab:CreateButton({
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
Tab:CreateButton({
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
ArrayField:LoadConfiguration()
print("Script Hub carregado com sucesso!")
