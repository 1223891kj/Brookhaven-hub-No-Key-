-- Carregar o OrionLib
local OrionLib = loadstring(game:HttpGet(('https://github.com/TemplariosScripts1/OrionLib/raw/refs/heads/main/OrionLib.txt')))()

-- Criar a Janela do Hub
local Window = OrionLib:MakeWindow({Name = "Light of Death Hub (BROOKHAVEN HUB)", HidePremium = false, SaveConfig = true, ConfigFolder = "OrionTest"})

-- Aba Geral
local GeralTab = Window:MakeTab({
    Name = "Geral",
    Icon = "rbxassetid://4483345998",
    PremiumOnly = false
})

-- Aviso (não utilizável)
GeralTab:AddLabel("O Hub ainda está em beta!")

-- Speed do flash (TOGGLE)
GeralTab:AddToggle({
    Name = "Speed do flash",
    Default = false,
    Callback = function(Value)
        local player = game.Players.LocalPlayer
        if Value then
            player.Character.Humanoid.WalkSpeed = 100
        else
            player.Character.Humanoid.WalkSpeed = 16
        end
    end
})

-- Pulo alto (TOGGLE)
GeralTab:AddToggle({
    Name = "Pulo alto",
    Default = false,
    Callback = function(Value)
        local player = game.Players.LocalPlayer
        if Value then
            player.Character.Humanoid.JumpPower = 100
        else
            player.Character.Humanoid.JumpPower = 50
        end
    end
})

-- Olhar players (Gaveta funcional)
local spectateOpen = false
local playersFolder = nil -- Pasta para armazenar os botões dos jogadores

GeralTab:AddButton({
    Name = "Olhar players",
    Callback = function()
        spectateOpen = not spectateOpen
        local player = game.Players.LocalPlayer
        local camera = workspace.CurrentCamera

        if spectateOpen then
            -- Criar uma "pasta" virtual para agrupar os botões
            playersFolder = Instance.new("Folder")
            playersFolder.Name = "PlayersListFolder"
            playersFolder.Parent = game.CoreGui

            -- Criar botões para cada jogador
            for _, v in ipairs(game.Players:GetPlayers()) do
                if v ~= player then
                    local btn = GeralTab:AddButton({
                        Name = v.Name,
                        Callback = function()
                            camera.CameraSubject = v.Character:FindFirstChildWhichIsA("Humanoid")
                        end
                    })
                    btn.Parent = playersFolder
                end
            end
        else
            -- Remover todos os botões criados quando fechar
            if playersFolder then
                for _, child in ipairs(playersFolder:GetChildren()) do
                    pcall(function() child:Destroy() end)
                end
                playersFolder:Destroy()
                playersFolder = nil
            end
        end
    end
})

-- Parar de olhar jogadores
GeralTab:AddButton({
    Name = "Parar de olhar jogadores",
    Callback = function()
        local player = game.Players.LocalPlayer
        workspace.CurrentCamera.CameraSubject = player.Character:FindFirstChildWhichIsA("Humanoid")
        OrionLib:MakeNotification({
            Name = "Spectate desativado",
            Content = "Agora você voltou a olhar seu próprio personagem!",
            Image = "rbxassetid://4483345998",
            Time = 5
        })
    end
})

-- Aba Troll
local TrollTab = Window:MakeTab({
    Name = "Troll",
    Icon = "rbxassetid://4483345998",
    PremiumOnly = false
})

-- Executar a troll completa
TrollTab:AddButton({
    Name = "Executar a troll completa",
    Callback = function()
        game.Players.LocalPlayer:Kick("A troll ainda está sendo feita.")
    end
})

-- Fling GUI (universal)
TrollTab:AddButton({
    Name = "Fling GUI (universal)",
    Callback = function()
        loadstring(game:HttpGet("https://rawscripts.net/raw/Universal-Script-FE-Fling-GUI-10927"))()
    end
})

-- Fly GUI
TrollTab:AddButton({
    Name = "Fly Gui",
    Callback = function()
        loadstring(game:HttpGet("https://rawscripts.net/raw/Universal-Script-Fly-v3-13879"))()
    end
})


-- Infinity yield adm FE 
TrollTab:AddButton({
    Name = "Adm Fe",
    Callback = function()
        loadstring(game:HttpGet('https://raw.githubusercontent.com/EdgeIY/infiniteyield/master/source'))()
    end
})

-- Teleport Players (movido para a aba Troll)
TrollTab:AddTextbox({
    Name = "Teleport players",
    Default = "",
    TextDisappear = false,
    Callback = function(input)
        local player = game.Players.LocalPlayer
        local target = nil

        for _, v in ipairs(game.Players:GetPlayers()) do
            if string.find(string.lower(v.Name), string.lower(input)) then
                target = v
                break
            end
        end

        if target and target.Character and target.Character:FindFirstChild("HumanoidRootPart") then
            player.Character.HumanoidRootPart.CFrame = target.Character.HumanoidRootPart.CFrame + Vector3.new(2, 0, 0)
            OrionLib:MakeNotification({
                Name = "Teleport realizado",
                Content = "Você foi teleportado para " .. target.Name,
                Image = "rbxassetid://4483345998",
                Time = 5
            })
        else
            OrionLib:MakeNotification({
                Name = "Erro",
                Content = "Jogador não encontrado!",
                Image = "rbxassetid://4483345998",
                Time = 5
            })
        end
    end
}) 
