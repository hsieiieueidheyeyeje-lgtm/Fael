--[[
    PRO HUB - Metrópole RP
    Funções: ESP | Aimbot | Noclip | Revistar
    Executor: Delta / Fluxus / Codex / Mobile + PC
]]

task.wait(0.5)

-- ============================================
-- SERVIÇOS
-- ============================================
local Players = game:GetService("Players")
local UIS = game:GetService("UserInputService")
local RunService = game:GetService("RunService")
local LP = Players.LocalPlayer
local PG = LP:WaitForChild("PlayerGui")
local Camera = workspace.CurrentCamera

-- ============================================
-- CONFIGURAÇÕES (todas)
-- ============================================
local CFG = {
    -- ESP
    ESP = false,
    Box = true,
    Line = true,
    Nome = true,
    Dist = true,
    Chams = true,
    -- Aimbot
    Aim = false,
    Silent = false,
    TeamCheck = true,
    FOV = 200,
    Smooth = 0.25,
    -- Noclip
    Noclip = false,
}

-- ============================================
-- CORES DO TEMA
-- ============================================
local C = {
    Fundo = Color3.fromRGB(20, 20, 28),
    Card = Color3.fromRGB(32, 32, 44),
    Botao = Color3.fromRGB(45, 45, 58),
    Accent = Color3.fromRGB(140, 80, 255),
    Texto = Color3.fromRGB(240, 240, 250),
    Sub = Color3.fromRGB(160, 160, 180),
    Verde = Color3.fromRGB(50, 200, 100),
    Vermelho = Color3.fromRGB(220, 60, 60),
}

-- ============================================
-- SCREEN GUI
-- ============================================
local SG = Instance.new("ScreenGui")
SG.Name = "ProHubFinal"
SG.ResetOnSpawn = false
SG.IgnoreGuiInset = true
SG.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
pcall(function() SG.Parent = PG end)
if not SG.Parent then SG.Parent = game:GetService("CoreGui") end

-- ============================================
-- LOADING SCREEN
-- ============================================
local Loading = Instance.new("Frame")
Loading.Size = UDim2.new(0, 340, 0, 140)
Loading.Position = UDim2.new(0.5, -170, 0.5, -70)
Loading.BackgroundColor3 = C.Fundo
Loading.BorderSizePixel = 0
Loading.Parent = SG
Instance.new("UICorner", Loading).CornerRadius = UDim.new(0, 10)

local LStroke = Instance.new("UIStroke", Loading)
LStroke.Color = C.Accent
LStroke.Thickness = 2

local LTitle = Instance.new("TextLabel", Loading)
LTitle.Size = UDim2.new(1, 0, 0, 30)
LTitle.Position = UDim2.new(0, 0, 0, 25)
LTitle.BackgroundTransparency = 1
LTitle.Text = "PRO HUB"
LTitle.TextColor3 = C.Texto
LTitle.TextSize = 22
LTitle.Font = Enum.Font.GothamBold

local LSub = Instance.new("TextLabel", Loading)
LSub.Size = UDim2.new(1, 0, 0, 20)
LSub.Position = UDim2.new(0, 0, 0, 58)
LSub.BackgroundTransparency = 1
LSub.Text = "Carregando..."
LSub.TextColor3 = C.Sub
LSub.TextSize = 12
LSub.Font = Enum.Font.Gotham

local LBarBg = Instance.new("Frame", Loading)
LBarBg.Size = UDim2.new(0, 280, 0, 6)
LBarBg.Position = UDim2.new(0.5, -140, 0, 100)
LBarBg.BackgroundColor3 = C.Botao
LBarBg.BorderSizePixel = 0
Instance.new("UICorner", LBarBg).CornerRadius = UDim.new(1, 0)

local LBar = Instance.new("Frame", LBarBg)
LBar.Size = UDim2.new(0, 0, 1, 0)
LBar.BackgroundColor3 = C.Accent
LBar.BorderSizePixel = 0
Instance.new("UICorner", LBar).CornerRadius = UDim.new(1, 0)

-- Animação
task.spawn(function()
    for i = 1, 25 do
        LBar.Size = UDim2.new(i/25, 0, 1, 0)
        task.wait(0.04)
    end
    task.wait(0.2)
    Loading:Destroy()
end)

-- ============================================
-- PAINEL PRINCIPAL
-- ============================================
local Main = Instance.new("Frame")
Main.Size = UDim2.new(0, 420, 0, 300)
Main.Position = UDim2.new(0.5, -210, 0.5, -150)
Main.BackgroundColor3 = C.Fundo
Main.BorderSizePixel = 0
Main.Active = true
Main.ClipsDescendants = true
Main.Parent = SG
Instance.new("UICorner", Main).CornerRadius = UDim.new(0, 10)

local MStroke = Instance.new("UIStroke", Main)
MStroke.Color = C.Accent
MStroke.Thickness = 2

-- TOPBAR
local Top = Instance.new("Frame", Main)
Top.Size = UDim2.new(1, 0, 0, 32)
Top.BackgroundColor3 = C.Card
Top.BorderSizePixel = 0
Top.Parent = Main
Instance.new("UICorner", Top).CornerRadius = UDim.new(0, 10)

local Titulo = Instance.new("TextLabel", Top)
Titulo.Size = UDim2.new(1, -80, 1, 0)
Titulo.Position = UDim2.new(0, 12, 0, 0)
Titulo.BackgroundTransparency = 1
Titulo.Text = "PRO HUB - Metropole RP"
Titulo.TextColor3 = C.Texto
Titulo.TextSize = 13
Titulo.Font = Enum.Font.GothamBold
Titulo.TextXAlignment = Enum.TextXAlignment.Left

local BMin = Instance.new("TextButton", Top)
BMin.Size = UDim2.new(0, 22, 0, 22)
BMin.Position = UDim2.new(1, -52, 0, 5)
BMin.BackgroundColor3 = C.Botao
BMin.Text = "-"
BMin.TextColor3 = C.Texto
BMin.TextSize = 14
BMin.Font = Enum.Font.GothamBold
BMin.BorderSizePixel = 0
Instance.new("UICorner", BMin).CornerRadius = UDim.new(0, 4)

local BClose = Instance.new("TextButton", Top)
BClose.Size = UDim2.new(0, 22, 0, 22)
BClose.Position = UDim2.new(1, -26, 0, 5)
BClose.BackgroundColor3 = C.Vermelho
BClose.Text = "X"
BClose.TextColor3 = Color3.new(1,1,1)
BClose.TextSize = 12
BClose.Font = Enum.Font.GothamBold
BClose.BorderSizePixel = 0
Instance.new("UICorner", BClose).CornerRadius = UDim.new(0, 4)

-- SIDEBAR
local Side = Instance.new("Frame", Main)
Side.Size = UDim2.new(0, 110, 1, -32)
Side.Position = UDim2.new(0, 0, 0, 32)
Side.BackgroundColor3 = C.Card
Side.BorderSizePixel = 0
Side.Parent = Main

local SL = Instance.new("UIListLayout", Side)
SL.Padding = UDim.new(0, 4)
SL.SortOrder = Enum.SortOrder.LayoutOrder

local SP = Instance.new("UIPadding", Side)
SP.PaddingTop = UDim.new(0, 8)
SP.PaddingLeft = UDim.new(0, 6)
SP.PaddingRight = UDim.new(0, 6)

-- CONTENT
local Content = Instance.new("Frame", Main)
Content.Size = UDim2.new(1, -110, 1, -32)
Content.Position = UDim2.new(0, 110, 0, 32)
Content.BackgroundColor3 = C.Fundo
Content.BorderSizePixel = 0
Content.ClipsDescendants = true
Content.Parent = Main

-- ============================================
-- SISTEMA DE ABAS
-- ============================================
local Abas = {}
local AbaAtual = nil

local function criarAba(nome, icone)
    local btn = Instance.new("TextButton", Side)
    btn.Size = UDim2.new(1, 0, 0, 30)
    btn.BackgroundColor3 = C.Botao
    btn.Text = icone.." "..nome
    btn.TextColor3 = C.Texto
    btn.TextSize = 11
    btn.Font = Enum.Font.GothamBold
    btn.BorderSizePixel = 0
    btn.AutoButtonColor = false
    Instance.new("UICorner", btn).CornerRadius = UDim.new(0, 6)
    
    local scroll = Instance.new("ScrollingFrame", Content)
    scroll.Size = UDim2.new(1, -12, 1, -12)
    scroll.Position = UDim2.new(0, 6, 0, 6)
    scroll.BackgroundTransparency = 1
    scroll.BorderSizePixel = 0
    scroll.ScrollBarThickness = 4
    scroll.ScrollBarImageColor3 = C.Accent
    scroll.CanvasSize = UDim2.new(0, 0, 0, 0)
    scroll.AutomaticCanvasSize = Enum.AutomaticSize.Y
    scroll.Visible = false
    
    local list = Instance.new("UIListLayout", scroll)
    list.Padding = UDim.new(0, 6)
    list.SortOrder = Enum.SortOrder.LayoutOrder
    
    local aba = {btn=btn, scroll=scroll, nome=nome}
    Abas[nome] = aba
    
    btn.MouseButton1Click:Connect(function()
        if AbaAtual then
            AbaAtual.scroll.Visible = false
            AbaAtual.btn.BackgroundColor3 = C.Botao
        end
        scroll.Visible = true
        btn.BackgroundColor3 = C.Accent
        AbaAtual = aba
    end)
    return aba
end

-- ============================================
-- COMPONENTES
-- ============================================
local function addToggle(aba, nome, cb)
    local card = Instance.new("Frame", aba.scroll)
    card.Size = UDim2.new(1, -8, 0, 34)
    card.BackgroundColor3 = C.Card
    card.BorderSizePixel = 0
    Instance.new("UICorner", card).CornerRadius = UDim.new(0, 6)
    
    local lbl = Instance.new("TextLabel", card)
    lbl.Size = UDim2.new(1, -60, 1, 0)
    lbl.Position = UDim2.new(0, 12, 0, 0)
    lbl.BackgroundTransparency = 1
    lbl.Text = nome
    lbl.TextColor3 = C.Texto
    lbl.TextSize = 11
    lbl.Font = Enum.Font.GothamBold
    lbl.TextXAlignment = Enum.TextXAlignment.Left
    
    local sw = Instance.new("Frame", card)
    sw.Size = UDim2.new(0, 38, 0, 20)
    sw.Position = UDim2.new(1, -48, 0.5, -10)
    sw.BackgroundColor3 = C.Botao
    sw.BorderSizePixel = 0
    Instance.new("UICorner", sw).CornerRadius = UDim.new(1, 0)
    
    local dot = Instance.new("Frame", sw)
    dot.Size = UDim2.new(0, 14, 0, 14)
    dot.Position = UDim2.new(0, 3, 0.5, -7)
    dot.BackgroundColor3 = C.Sub
    dot.BorderSizePixel = 0
    Instance.new("UICorner", dot).CornerRadius = UDim.new(1, 0)
    
    local btn = Instance.new("TextButton", card)
    btn.Size = UDim2.new(1, 0, 1, 0)
    btn.BackgroundTransparency = 1
    btn.Text = ""
    
    local est = false
    btn.MouseButton1Click:Connect(function()
        est = not est
        if est then
            sw.BackgroundColor3 = C.Verde
            dot.Position = UDim2.new(0, 21, 0.5, -7)
            dot.BackgroundColor3 = Color3.new(1,1,1)
        else
            sw.BackgroundColor3 = C.Botao
            dot.Position = UDim2.new(0, 3, 0.5, -7)
            dot.BackgroundColor3 = C.Sub
        end
        if cb then pcall(cb, est) end
    end)
end

local function addBotao(aba, nome, cb, cor)
    local btn = Instance.new("TextButton", aba.scroll)
    btn.Size = UDim2.new(1, -8, 0, 34)
    btn.BackgroundColor3 = cor or C.Card
    btn.Text = nome
    btn.TextColor3 = C.Texto
    btn.TextSize = 11
    btn.Font = Enum.Font.GothamBold
    btn.BorderSizePixel = 0
    Instance.new("UICorner", btn).CornerRadius = UDim.new(0, 6)
    btn.MouseButton1Click:Connect(function() if cb then pcall(cb) end end)
end

local function addSlider(aba, nome, min, max, ini, cb)
    local card = Instance.new("Frame", aba.scroll)
    card.Size = UDim2.new(1, -8, 0, 42)
    card.BackgroundColor3 = C.Card
    card.BorderSizePixel = 0
    Instance.new("UICorner", card).CornerRadius = UDim.new(0, 6)
    
    local lbl = Instance.new("TextLabel", card)
    lbl.Size = UDim2.new(1, -20, 0, 18)
    lbl.Position = UDim2.new(0, 12, 0, 4)
    lbl.BackgroundTransparency = 1
    lbl.Text = nome..": "..tostring(ini)
    lbl.TextColor3 = C.Texto
    lbl.TextSize = 11
    lbl.Font = Enum.Font.GothamBold
    lbl.TextXAlignment = Enum.TextXAlignment.Left
    
    local bg = Instance.new("Frame", card)
    bg.Size = UDim2.new(1, -24, 0, 6)
    bg.Position = UDim2.new(0, 12, 0, 28)
    bg.BackgroundColor3 = C.Botao
    bg.BorderSizePixel = 0
    Instance.new("UICorner", bg).CornerRadius = UDim.new(1, 0)
    
    local fill = Instance.new("Frame", bg)
    fill.Size = UDim2.new((ini-min)/(max-min), 0, 1, 0)
    fill.BackgroundColor3 = C.Accent
    fill.BorderSizePixel = 0
    Instance.new("UICorner", fill).CornerRadius = UDim.new(1, 0)
    
    local btn = Instance.new("TextButton", card)
    btn.Size = UDim2.new(1, 0, 1, 0)
    btn.BackgroundTransparency = 1
    btn.Text = ""
    
    local arr = false
    local function setV(x)
        local rel = math.clamp((x - bg.AbsolutePosition.X) / bg.AbsoluteSize.X, 0, 1)
        local val = math.floor(min + (max-min)*rel)
        fill.Size = UDim2.new(rel, 0, 1, 0)
        lbl.Text = nome..": "..tostring(val)
        if cb then pcall(cb, val) end
    end
    btn.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
            arr = true
            setV(input.Position.X)
        end
    end)
    UIS.InputChanged:Connect(function(input)
        if arr and (input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch) then
            setV(input.Position.X)
        end
    end)
    UIS.InputEnded:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then arr = false end
    end)
end

-- ============================================
-- FUNÇÕES AUXILIARES
-- ============================================
local function getHum() local c = LP.Character return c and c:FindFirstChildOfClass("Humanoid") end
local function getHRP() local c = LP.Character return c and c:FindFirstChild("HumanoidRootPart") end

-- ============================================
-- 🦴 ESP
-- ============================================
local ESPObj = {}

local function limparESP()
    for _, o in pairs(ESPObj) do
        for _, v in pairs(o) do
            if v then
                pcall(function() if v.Remove then v:Remove() end end)
                pcall(function() if v.Destroy then v:Destroy() end end)
            end
        end
    end
    ESPObj = {}
end

local function updateESP()
    if not CFG.ESP then
        if next(ESPObj) then limparESP() end
        return
    end
    
    local hrpMeu = getHRP()
    local posMeu = hrpMeu and hrpMeu.Position or Vector3.zero
    
    for _, p in ipairs(Players:GetPlayers()) do
        if p ~= LP and p.Character then
            local hrp = p.Character:FindFirstChild("HumanoidRootPart")
            local hum = p.Character:FindFirstChildOfClass("Humanoid")
            
            if hrp and hum and hum.Health > 0 then
                local dist = (hrp.Position - posMeu).Magnitude
                
                if dist <= 1500 then
                    local mesmaEq = false
                    if CFG.TeamCheck and p.Team and LP.Team then
                        mesmaEq = p.Team == LP.Team
                    end
                    local cor = mesmaEq and Color3.fromRGB(60,255,100) or Color3.fromRGB(255,60,60)
                    
                    if not ESPObj[p] then
                        local o = {}
                        if CFG.Box then
                            o.Box = Drawing.new("Square")
                            o.Box.Thickness = 1
                            o.Box.Filled = false
                            o.Box.Visible = false
                        end
                        if CFG.Line then
                            o.Line = Drawing.new("Line")
                            o.Line.Thickness = 1.5
                            o.Line.Visible = false
                        end
                        if CFG.Nome then
                            o.Nome = Drawing.new("Text")
                            o.Nome.Size = 13
                            o.Nome.Center = true
                            o.Nome.Outline = true
                            o.Nome.Visible = false
                        end
                        if CFG.Dist then
                            o.Dist = Drawing.new("Text")
                            o.Dist.Size = 11
                            o.Dist.Center = true
                            o.Dist.Outline = true
                            o.Dist.Visible = false
                        end
                        if CFG.Chams then
                            o.HL = Instance.new("Highlight")
                            o.HL.OutlineColor = Color3.new(1,1,1)
                            o.HL.FillTransparency = 0.5
                            o.HL.Enabled = false
                            o.HL.Parent = p.Character
                        end
                        ESPObj[p] = o
                    end
                    
                    local o = ESPObj[p]
                    
                    -- Box
                    if o.Box then
                        local tp = Camera:WorldToViewportPoint(hrp.Position + Vector3.new(0,3,0))
                        local bp = Camera:WorldToViewportPoint(hrp.Position - Vector3.new(0,3,0))
                        if tp.Z > 0 and bp.Z > 0 then
                            local h = math.abs(bp.Y - tp.Y)
                            local w = h * 0.6
                            o.Box.Size = Vector2.new(w, h)
                            o.Box.Position = Vector2.new(tp.X - w/2, tp.Y)
                            o.Box.Color = cor
                            o.Box.Visible = true
                        else
                            o.Box.Visible = false
                        end
                    end
                    
                    -- Line (embaixo)
                    if o.Line then
                        local pe = Camera:WorldToViewportPoint(hrp.Position - Vector3.new(0,2.5,0))
                        local ch = Camera:WorldToViewportPoint(hrp.Position - Vector3.new(0,8,0))
                        if pe.Z > 0 and ch.Z > 0 then
                            o.Line.From = Vector2.new(pe.X, pe.Y)
                            o.Line.To = Vector2.new(ch.X, ch.Y)
                            o.Line.Color = cor
                            o.Line.Visible = true
                        else
                            o.Line.Visible = false
                        end
                    end
                    
                    -- Nome
                    if o.Nome then
                        local tp = Camera:WorldToViewportPoint(hrp.Position + Vector3.new(0,3.5,0))
                        if tp.Z > 0 then
                            o.Nome.Text = p.Name
                            o.Nome.Position = Vector2.new(tp.X, tp.Y)
                            o.Nome.Color = cor
                            o.Nome.Visible = true
                        else
                            o.Nome.Visible = false
                        end
                    end
                    
                    -- Distância
                    if o.Dist then
                        local tp = Camera:WorldToViewportPoint(hrp.Position + Vector3.new(0,3,0))
                        if tp.Z > 0 then
                            o.Dist.Text = "["..math.floor(dist).."m]"
                            o.Dist.Position = Vector2.new(tp.X, tp.Y + 14)
                            o.Dist.Color = cor
                            o.Dist.Visible = true
                        else
                            o.Dist.Visible = false
                        end
                    end
                    
                    -- Chams
                    if o.HL then
                        if o.HL.Parent ~= p.Character then o.HL.Parent = p.Character end
                        o.HL.FillColor = cor
                        o.HL.Enabled = true
                    end
                end
            end
        end
    end
end

-- ============================================
-- 🎯 AIMBOT
-- ============================================
local function acharAlvo()
    local alvo, menorD = nil, CFG.FOV
    local mp = UIS:GetMouseLocation()
    
    for _, p in ipairs(Players:GetPlayers()) do
        if p ~= LP and p.Character then
            local hrp = p.Character:FindFirstChild("HumanoidRootPart")
            local hum = p.Character:FindFirstChildOfClass("Humanoid")
            
            if hrp and hum and hum.Health > 0 then
                if CFG.TeamCheck and p.Team and LP.Team then
                    if p.Team == LP.Team then continue end
                end
                
                local sp, on = Camera:WorldToViewportPoint(hrp.Position)
                if on then
                    local d = (Vector2.new(sp.X, sp.Y) - mp).Magnitude
                    if d < menorD then
                        menorD = d
                        alvo = p.Character
                    end
                end
            end
        end
    end
    return alvo
end

local function updateAimbot()
    if not CFG.Aim then return end
    local alvo = acharAlvo()
    if alvo then
        local parte = alvo:FindFirstChild("Head")
        if parte and UIS.MouseEnabled then
            local sp = Camera:WorldToViewportPoint(parte.Position)
            local mp = UIS:GetMouseLocation()
            pcall(function()
                mousemoverel((sp.X-mp.X)*CFG.Smooth, (sp.Y-mp.Y)*CFG.Smooth)
            end)
        end
    end
end

local function updateSilent()
    if not CFG.Silent then return end
    local alvo = acharAlvo()
    if alvo then
        local parte = alvo:FindFirstChi
