-- Carrega a Fluent em segundo plano para enviar as notificações visuais bonitas
local Fluent = pcall(function()
    return loadstring(game:HttpGet("https://github.com/dawid-scripts/Fluent/releases/latest/download/main.lua"))()
end)

-- CONFIGURAÇÕES DE KEYS E ACESSOS DO COELHO HUB
local CHAVE_PUBLICA = ""
local CHAVE_TESTER_OFICIAL = "FLYSGASHPAIHSAFSJAKLSHFGHJKLKJHGFGSFGHAGFGGFY"
local ID_IMAGEM_TESTER = "rbxassetid://71227064035550"

-- Pega os dados do jogador atual
local L_1_KEY_ = tostring(getgenv().KeyTester or "")
local L_1_PNAME_ = tostring(game:GetService("Players").LocalPlayer.Name)
local L_1_PNAME_L_ = string.lower(L_1_PNAME_)
local L_1_BYPASS_ = false

-- LISTA DE TESTERS AUTORIZADOS A USAR A CHAVE GIGANTE
local L_1_ADMIN_USERS_ = {
    "amigo_desonesto0"
}

-- 1. VERIFICAÇÃO DE CHAVES VÁLIDAS
if L_1_KEY_ ~= CHAVE_PUBLICA and L_1_KEY_ ~= CHAVE_TESTER_OFICIAL then
    local _kp = game:GetService("Players").LocalPlayer
    local _kt = tick()
    while not _kp:FindFirstChild("DataLoaded") and tick() - _kt < 15 do
        task.wait(0.3)
    end
    task.wait(0.5)
    
    -- Copia o link do seu Discord para a área de transferência do cara
    pcall(function()
        if setclipboard then setclipboard("https://discord.gg/eeTwpuGaVa")
        elseif toclipboard then toclipboard("https://discord.gg/eeTwpuGaVa")
        elseif syn and syn.Clipboard then syn.Clipboard.set("https://discord.gg/eeTwpuGaVa")
        end
    end)
    
    _kp:Kick("Key Invalid, Join In Discord To Get Key, Clipboard https://discord.gg/eeTwpuGaVa")
    task.wait(5)
    return
end

-- 2. SE FOR A CHAVE DE TESTER, VALIDA SE O NICK ESTÁ NA LISTA PERMITIDA
if L_1_KEY_ == CHAVE_TESTER_OFICIAL then
    local L_1_IS_ADMIN_ = false
    for _, L_1_au_ in ipairs(L_1_ADMIN_USERS_) do
        if string.lower(L_1_au_) == L_1_PNAME_L_ then
            L_1_IS_ADMIN_ = true
            break
        end
    end
    
    -- Se um espertinho achar sua chave gigante mas não for tester, ele é auto-banido localmente
    if not L_1_IS_ADMIN_ then
        pcall(function()
            if not isfolder("CoelhoHub Configs") then makefolder("CoelhoHub Configs") end
            if writefile then
                writefile("CoelhoHub Configs/C_BLOCKED_" .. L_1_PNAME_ .. ".txt", L_1_PNAME_ .. "\nUNAUTHORIZED_TESTER_KEY")
            end
        end)
        game:GetService("Players").LocalPlayer:Kick("4C3SS D3N13D PL4Y3R 1NV4L4D - NOT A TESTER")
        return
    end
    L_1_BYPASS_ = true
end

-- 3. VERIFICAÇÕES DE SEGURANÇA (PULADAS SE FOR UM TESTER OFICIAL)
if not L_1_BYPASS_ then
    -- Checa se o cara já foi auto-banido antes
    local L_1_IS_BLOCKED_ = false
    pcall(function()
        if isfolder and isfolder("CoelhoHub Configs")
            and isfile and isfile("CoelhoHub Configs/C_BLOCKED_" .. L_1_PNAME_ .. ".txt") then
            L_1_IS_BLOCKED_ = true
        end
    end)
    if L_1_IS_BLOCKED_ then
        game:GetService("Players").LocalPlayer:Kick("Blacklisted Player F0r3v3r")
        return
    end

    -- Restrição de Mapas (Blox Fruits - Sea 1, Sea 2, Sea 3, etc.)
    local L_1_PLACES_ = {
        [2753915549] = true, [4442272183] = true, [7449423635] = true,
        [85211729168715] = true, [79091703265657] = true, [100117331123089] = true
    }
    if not L_1_PLACES_[game.PlaceId] then
        game:GetService("Players").LocalPlayer:Kick("Unloaded World")
        return
    end

    -- Blacklist manual por Nickname
    local L_1_BLACKLIST_ = { "hdandhaieak" }
    for _, L_1_bn_ in ipairs(L_1_BLACKLIST_) do
        if L_1_PNAME_L_ == string.lower(L_1_bn_) then
            game:GetService("Players").LocalPlayer:Kick("Blacklisted User")
            return
        end
    end
end

-- =====================================================================
-- 4. CARREGAMENTO DOS SCRIPTS (CHAVE ESTÁ OK E VALIDADA)
-- =====================================================================
if L_1_KEY_ == CHAVE_TESTER_OFICIAL then
    -- Abre o Splash Screen com a imagem que você escolheu para os testers legítimos
    pcall(function()
        local CoreGui = game:GetService("CoreGui")
        local TargetGui = CoreGui:FindFirstChild("RobloxGui") or game:GetService("Players").LocalPlayer:WaitForChild("PlayerGui")
        
        local SplashGui = Instance.new("ScreenGui")
        SplashGui.Name = "CoelhoHubSplash"
        SplashGui.Parent = TargetGui
        
        local ImageLabel = Instance.new("ImageLabel")
        ImageLabel.Image = ID_IMAGEM_TESTER
        ImageLabel.BackgroundTransparency = 1
        ImageLabel.AnchorPoint = Vector2.new(0.5, 0.5)
        ImageLabel.Position = UDim2.new(0.5, 0, 0.5, 0)
        ImageLabel.Size = UDim2.new(0, 400, 0, 400)
        ImageLabel.Parent = SplashGui
        
        if type(Fluent) == "table" and Fluent.Notify then
            Fluent:Notify({
                Title = "Coelho Hub | Tester Mode",
                Content = "Acesso de desenvolvedor verificado!",
                Duration = 3
            })
        end
        task.wait(3)
        SplashGui:Destroy()
    end)

    -- EXECUTA O SEU REPOSITÓRIO SECRETO DE TESTES
    pcall(function()
        loadstring(game:HttpGet("https://raw.githubusercontent.com/af6094870-art/TesterCoelho-hub/refs/heads/main/README.md"))()
    end)
else
