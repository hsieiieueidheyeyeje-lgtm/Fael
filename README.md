--[[
    ╔══════════════════════════════════════╗
    ║         PRO HUB - COMPLETO           ║
    ║   ESP | Aimbot | Noclip | Revistar   ║
    ╚══════════════════════════════════════╝
    Teclas:
    → P = Liga/Desliga ESP
    → RightShift = Abre/Fecha painel
]]

task.wait(0.5)

local Players = game:GetService("Players")
local UIS = game:GetService("UserInputService")
local RunService = game:GetService("RunService")
local LP = Players.LocalPlayer
local PG = LP:WaitForChild("PlayerGui")
local Camera = workspace.CurrentCamera

-- ============================================
-- CONFIG
-- ============================================
local CFG = {
    -- ESP
    ESP = true, Box = true, Nome = true, Dist = true, Health = false,
    Chams = true, TeamCheck = false, MaxDist = 2000,
    Cor = Color3.fromRGB(255, 50, 50),
    CorAmigo = Color3.fromRGB(50, 255, 100),
    -- AIMBOT
    Aim = false, Silent = false, WallCheck = true,
    FOV = 200, Smooth = 0.3, AimPart = "Head",
    -- MISC
    Noclip = false,
    RevistarAuto = false,
    -- GUI
    PainelAberto = true,
}

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
-- AUXILIARES
-- ============================================
local function getChar() return LP.Character end
local function getHRP()
    local c = getChar()
    return c and c:FindFirstChild("HumanoidRootPart")
end
local function getHum()
    local c = getChar()
    return c and c:FindFirstChildOfClass("Humanoid")
end

-- ============================================
-- SCREEN GUI
-- ============================================
local SG = Instance.new("ScreenGui")
SG.Name = "ProHub"
SG.ResetOnSpawn = false
SG.IgnoreGuiInset = true
SG.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
SG.Parent = PG

-- ============================================
-- LOADING
-- ============================================
local Loading = Instance.new("Frame", SG)
Loading.Size = UDim2.new(0, 320, 0, 120)
Loading.Position = UDim2.new(0.5, -160, 0.5, -60)
Loading.BackgroundColor3 = C.Fundo
Loading.BorderSizePixel = 0
Instance.new("UICorner", Loading).CornerRadius = UDim.new(0, 10)
local LS = Instance.new("UIStroke", Loading)
LS.Color = C.Accent LS.Thickness = 2

local LT = Instance.new("TextLabel", Loading)
LT.Size = UDim2.new(1, 0, 0, 30)
LT.Position = UDim2.new(0, 0, 0, 20)
LT.BackgroundTransparency = 1
LT.Text = "PRO HUB"
LT.TextColor3 = C.Texto
LT.TextSize = 20
LT.Font = Enum.Font.GothamBold

local LS2 = Instance.new("TextLabel", Loading)
LS2.Size = UDim2.new(1, 0, 0, 18)
LS2.Position = UDim2.new(0, 0, 0, 50)
LS2.BackgroundTransparency = 1
LS2.Text = "Carregando..."
LS2.TextColor3 = C.Sub
LS2.TextSize = 11
LS2.Font = Enum.Font.Gotham

local LBB = Instance.new("Frame", Loading)
LBB.Size = UDim2.new(0, 260, 0, 6)
LBB.Position = UDim2.new(0.5, -130, 0, 85)
LBB.BackgroundColor3 = C.Botao
LBB.BorderSizePixel = 0
Instance.new("UICorner", LBB).CornerRadius = UDim.new(1, 0)

local LB = Instance.new("Frame", LBB)
LB.Size = UDim2.new(0, 0, 1, 0)
LB.BackgroundColor3 = C.Accent
LB.BorderSizePixel = 0
Instance.new("UICorner", LB).CornerRadius = UDim.new(1, 0)

task.spawn(function()
    for i = 1, 20 do
        LB.Size = UDim2.new(i/20, 0, 1, 0)
        task.wait(0.03)
    end
    task.wait(0.1)
    Loading:Destroy()
end)

-- ============================================
-- PAINEL PRINCIPAL
-- ============================================
local Main = Instance.new("Frame", SG)
Main.Size = UDim2.new(0, 440, 0, 320)
Main.Position = UDim2.new(0.5, -220, 0.5, -160)
Main.BackgroundColor3 = C.Fundo
Main.BorderSizePixel = 0
Main.Active = true
Main.ClipsDescendants = true
Instance.new("UICorner", Main).CornerRadius = UDim.new(0, 8)

local MS = Instance.new("UIStroke", Main)
MS.Color = C.Accent MS.Thickness = 1.5

-- TOPBAR
local TopBar = Instance.new("Frame", Main)
TopBar.Size = UDim2.new(1, 0, 0, 32)
TopBar.BackgroundColor3 = C.Card
TopBar.BorderSizePixel = 0
Instance.new("UICorner", TopBar).CornerRadius = UDim.new(0, 8)

local TBFix = Instance.new("Frame", TopBar)
TBFix.Size = UDim2.new(1, 0, 0, 8)
TBFix.Position = UDim2.new(0, 0, 1, -8)
TBFix.BackgroundColor3 = C.Card
TBFix.BorderSizePixel = 0

local Title = Instance.new("TextLabel", TopBar)
Title.Size = UDim2.new(1, -70, 1, 0)
Title.Position = UDim2.new(0, 10, 0, 0)
Title.BackgroundTransparency = 1
Title.Text = "PRO HUB ✦ Metrópole RP"
Title.TextColor3 = C.Texto
Title.TextSize = 12
Title.Font = Enum.Font.GothamBold
Title.TextXAlignment = Enum.TextXAlignment.Left

local BtnMin = Instance.new("TextButton", TopBar)
BtnMin.Size = UDim2.new(0, 22, 0, 22)
BtnMin.Position = UDim2.new(1, -52, 0, 5)
BtnMin.BackgroundColor3 = C.Botao
BtnMin.Text = "—"
BtnMin.TextColor3 = C.Texto
BtnMin.TextSize = 13
BtnMin.Font = Enum.Font.GothamBold
BtnMin.BorderSizePixel = 0
Instance.new("UICorner", BtnMin).CornerRadius = UDim.new(0, 4)

local BtnClose = Instance.new("TextButton", TopBar)
BtnClose.Size = UDim2.new(0, 22, 0, 22)
BtnClose.Position = UDim2.new(1, -26, 0, 5)
BtnClose.BackgroundColor3 = C.Vermelho
BtnClose.Text = "X"
BtnClose.TextColor3 = Color3.new(1, 1, 1)
BtnClose.TextSize = 11
BtnClose.Font = Enum.Font.GothamBold
BtnClose.BorderSizePixel = 0
Instance.new("UICorner", BtnClose).CornerRadius = UDim.new(0, 4)

-- SIDEBAR
local Sidebar = Instance.new("Frame", Main)
Sidebar.Size = UDim2.new(0, 95, 1, -32)
Sidebar.Position = UDim2.new(0, 0, 0, 32)
Sidebar.BackgroundColor3 = C.Card
Sidebar.BorderSizePixel = 0

local SBList = Instance.new("UIListLayout", Sidebar)
SBList.Padding = UDim.new(0, 3)
SBList.SortOrder = Enum.SortOrder.LayoutOrder

local SBPad = Instance.new("UIPadding", Sidebar)
SBPad.PaddingTop = UDim.new(0, 6)
SBPad.PaddingLeft = UDim.new(0, 6)
SBPad.PaddingRight = UDim.new(0, 6)

-- CONTENT
local Content = Instance.new("Frame", Main)
Content.Size = UDim2.new(1, -95, 1, -32)
Content.Position = UDim2.new(0, 95, 0, 32)
Content.BackgroundColor3 = C.Fundo
Content.BorderSizePixel = 0

local CPad = Instance.new("UIPadding", Content)
CPad.PaddingTop = UDim.new(0, 8)
CPad.PaddingLeft = UDim.new(0, 8)
CPad.PaddingRight = UDim.new(0, 8)

-- ============================================
-- ARRASTAR PAINEL
-- ============================================
do
    local dragging, dragStart, startPos
    TopBar.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
            dragging = true
            dragStart = input.Position
            startPos = Main.Position
        end
    end)
    UIS.InputChanged:Connect(function(input)
        if dragging and (input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch) then
            local d = input.Position - dragStart
            Main.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + d.X, startPos.Y.Scale, startPos.Y.Offset + d.Y)
        end
    end)
    UIS.InputEnded:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
            dragging = false
        end
    end)
end

-- MINIMIZAR / FECHAR
local minimizado = false
BtnMin.MouseButton1Click:Connect(function()
    minimizado = not minimizado
    if minimizado then
        Main.Size = UDim2.new(0, 440, 0, 32)
        Sidebar.Visible = false
        Content.Visible = false
    else
        Main.Size = UDim2.new(0, 440, 0, 320)
        Sidebar.Visible = true
        Content.Visible = true
    end
end)
BtnClose.MouseButton1Click:Connect(function()
    CFG.PainelAberto = false
    Main.Visible = false
end)

-- ============================================
-- ABAS
-- ============================================
local Abas, AbaAtual = {}, nil

local function criarAba(nome, icone)
    local btn = Instance.new("TextButton", Sidebar)
    btn.Size = UDim2.new(1, 0, 0, 26)
    btn.BackgroundColor3 = C.Botao
    btn.Text = icone.." "..nome
    btn.TextColor3 = C.Texto
    btn.TextSize = 9
    btn.Font = Enum.Font.GothamBold
    btn.BorderSizePixel = 0
    btn.AutoButtonColor = false
    Instance.new("UICorner", btn).CornerRadius = UDim.new(0, 6)

    local scroll = Instance.new("ScrollingFrame", Content)
    scroll.Size = UDim2.new(1, 0, 1, 0)
    scroll.BackgroundTransparency = 1
    scroll.ScrollBarThickness = 3
    scroll.ScrollBarImageColor3 = C.Accent
    scroll.CanvasSize = UDim2.new(0, 0, 0, 0)
    scroll.AutomaticCanvasSize = Enum.AutomaticSize.Y
    scroll.Visible = false

    local list = Instance.new("UIListLayout", scroll)
    list.Padding = UDim.new(0, 4)
    list.SortOrder = Enum.SortOrder.LayoutOrder

    local aba = {botao=btn, scroll=scroll}
    Abas[nome] = aba

    btn.MouseButton1Click:Connect(function()
        if AbaAtual then
            AbaAtual.scroll.Visible = false
            AbaAtual.botao.BackgroundColor3 = C.Botao
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
local function criarToggle(aba, nome, ini, cb)
    local card = Instance.new("Frame", aba.scroll)
    card.Size = UDim2.new(1, -6, 0, 30)
    card.BackgroundColor3 = C.Card
    card.BorderSizePixel = 0
    Instance.new("UICorner", card).CornerRadius = UDim.new(0, 6)

    local lbl = Instance.new("TextLabel", card)
    lbl.Size = UDim2.new(1, -55, 1, 0)
    lbl.Position = UDim2.new(0, 10, 0, 0)
    lbl.BackgroundTransparency = 1
    lbl.Text = nome
    lbl.TextColor3 = C.Texto
    lbl.TextSize = 10
    lbl.Font = Enum.Font.GothamBold
    lbl.TextXAlignment = Enum.TextXAlignment.Left

    local swBg = Instance.new("Frame", card)
    swBg.Size = UDim2.new(0, 34, 0, 18)
    swBg.Position = UDim2.new(1, -42, 0.5, -9)
    swBg.BackgroundColor3 = ini and C.Verde or C.Botao
    swBg.BorderSizePixel = 0
    Instance.new("UICorner", swBg).CornerRadius = UDim.new(1, 0)

    local swDot = Instance.new("Frame", swBg)
    swDot.Size = UDim2.new(0, 12, 0, 12)
    swDot.Position = ini and UDim2.new(0, 19, 0.5, -6) or UDim2.new(0, 3, 0.5, -6)
    swDot.BackgroundColor3 = ini and Color3.new(1,1,1) or C.Sub
    swDot.BorderSizePixel = 0
    Instance.new("UICorner", swDot).CornerRadius = UDim.new(1, 0)

    local btn = Instance.new("TextButton", card)
    btn.Size = UDim2.new(1, 0, 1, 0)
    btn.BackgroundTransparency = 1
    btn.Text = ""

    local est = ini
    btn.MouseButton1Click:Connect(function()
        est = not est
        if est then
            swBg.BackgroundColor3 = C.Verde
            swDot.Position = UDim2.new(0, 19, 0.5, -6)
            swDot.BackgroundColor3 = Color3.new(1, 1, 1)
        else
            swBg.BackgroundColor3 = C.Botao
            swDot.Position = UDim2.new(0, 3, 0.5, -6)
            swDot.BackgroundColor3 = C.Sub
        end
        if cb then pcall(cb, est) end
    end)
end

local function criarBotao(aba, nome, cb, cor)
    local btn = Instance.new("TextButton", aba.scroll)
    btn.Size = UDim2.new(1, -6, 0, 28)
    btn.BackgroundColor3 = cor or C.Card
    btn.Text = nome
    btn.TextColor3 = C.Texto
    btn.TextSize = 10
    btn.Font = Enum.Font.GothamBold
    btn.BorderSizePixel = 0
    Instance.new("UICorner", btn).CornerRadius = UDim.new(0, 6)
    btn.MouseButton1Click:Connect(function() if cb then pcall(cb) end end)
end

local function criarSlider(aba, nome, min, max, ini, cb)
    local card = Instance.new("Frame", aba.scroll)
    card.Size = UDim2.new(1, -6, 0, 38)
    card.BackgroundColor3 = C.Card
    card.BorderSizePixel = 0
    Instance.new("UICorner", card).CornerRadius = UDim.new(0, 6)

    local lbl = Instance.new("TextLabel", card)
    lbl.Size = UDim2.new(1, -16, 0, 16)
    lbl.Position = UDim2.new(0, 10, 0, 4)
    lbl.BackgroundTransparency = 1
    lbl.Text = nome..": "..tostring(ini)
    lbl.TextColor3 = C.Texto
    lbl.TextSize = 10
    lbl.Font = Enum.Font.GothamBold
    lbl.TextXAlignment = Enum.TextXAlignment.Left

    local barBg = Instance.new("Frame", card)
    barBg.Size = UDim2.new(1, -20, 0, 5)
    barBg.Position = UDim2.new(0, 10, 0, 26)
    barBg.BackgroundColor3 = C.Botao
    barBg.BorderSizePixel = 0
    Instance.new("UICorner", barBg).CornerRadius = UDim.new(1, 0)

    local fill = Instance.new("Frame", barBg)
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
        local rel = math.clamp((x - barBg.AbsolutePosition.X) / barBg.AbsoluteSize.X, 0, 1)
        local val = math.floor(min + (max-min)*rel)
        fill.Size = UDim2.new(rel, 0, 1, 0)
        lbl.Text = nome..": "..tostring(val)
        if cb then pcall(cb, val) end
    end
    btn.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
            arr = true setV(input.Position.X)
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
-- ESP
-- ============================================
local ESPs = {}

local function removerESP(player)
    local e = ESPs[player]
    if not e then return end
    for _, obj in pairs(e) do
        pcall(function() if obj.Remove then obj:Remove() end end)
        pcall(function() if obj.Destroy then obj:Destroy() end end)
    end
    ESPs[player] = nil
end

local function limparTudoESP()
    for p, _ in pairs(ESPs) do removerESP(p) end
    ESPs = {}
end

local function criarESP(player)
    if player == LP or ESPs[player] then return end
    local e = {}

    e.box = Drawing.new("Square")
    e.box.Thickness = 1
    e.box.Filled = false
    e.box.Transparency = 1
    e.box.Color = CFG.Cor
    e.box.Visible = false

    e.nome = Drawing.new("Text")
    e.nome.Size = 14
    e.nome.Center = true
    e.nome.Outline = true
    e.nome.Color = Color3.fromRGB(255, 255, 255)
    e.nome.Visible = false

    e.dist = Drawing.new("Text")
    e.dist.Size = 12
    e.dist.Center = true
    e.dist.Outline = true
    e.dist.Color = Color3.fromRGB(255, 255, 0)
    e.dist.Visible = false

    e.health = Drawing.new("Text")
    e.health.Size = 12
    e.health.Center = true
    e.health.Outline = true
    e.health.Color = Color3.fromRGB(50, 255, 50)
    e.health.Visible = false

    e.hl = Instance.new("Highlight")
    e.hl.FillColor = CFG.Cor
    e.hl.OutlineColor = Color3.new(1, 1, 1)
    e.hl.FillTransparency = 0.6
    e.hl.OutlineTransparency = 0
    e.hl.DepthMode = Enum.HighlightDepthMode.AlwaysOnTop
    e.hl.Enabled = false

    ESPs[player] = e
end

local function updateESP()
    if not CFG.ESP then
        if next(ESPs) then limparTudoESP() end
        return
    end

    local meuHRP = getHRP()
    local minhaPos = meuHRP and meuHRP.Position or Vector3.zero

    for _, p in ipairs(Players:GetPlayers()) do
        if p ~= LP and p.Character then
            local hrp = p.Character:FindFirstChild("HumanoidRootPart")
            local hum = p.Character:FindFirstChildOfClass("Humanoid")
            local head = p.Character:FindFirstChild("Head")

            if hrp and hum and head and hum.Health > 0 then
                local dist = (hrp.Position - minhaPos).Magnitude
                if dist <= CFG.MaxDist then
                    if not ESPs[p] then criarESP(p) end
                    local e = ESPs[p]
                    if e then
                        local mesmaEq = false
                        if CFG.TeamCheck and p.Team and LP.Team then mesmaEq = p.Team == LP.Team end
                        local cor = mesmaEq and CFG.CorAmigo or CFG.Cor

                        local headPos, headOn = Camera:WorldToViewportPoint(head.Position + Vector3.new(0, 0.5, 0))
                        local rootPos, rootOn = Camera:WorldToViewportPoint(hrp.Position - Vector3.new(0, 3, 0))

                        if headOn and rootOn and headPos.Z > 0 and rootPos.Z > 0 then
                            local h = math.abs(rootPos.Y - headPos.Y)
                            local w = h * 0.6
                            if h < 10 then h = 50 w = 30 end

                            if CFG.Box then
                                e.box.Size = Vector2.new(w, h)
                                e.box.Position = Vector2.new(headPos.X - w/2, headPos.Y)
                                e.box.Color = cor
                                e.box.Visible = true
                            else e.box.Visible = false end

                            if CFG.Nome then
                                e.nome.Text = p.Name
                                e.nome.Position = Vector2.new(headPos.X, headPos.Y - 18)
                                e.nome.Color = cor
                                e.nome.Visible = true
                            else e.nome.Visible = false end

                            if CFG.Dist then
                                e.dist.Text = "["..math.floor(dist).."m]"
                                e.dist.Position = Vector2.new(headPos.X, rootPos.Y + 2)
                                e.dist.Color = cor
                                e.dist.Visible = true
                            else e.dist.Visible = false end

                            if CFG.Health then
                                e.health.Text = "❤ "..math.floor(hum.Health)
                                e.health.Position = Vector2.new(headPos.X, headPos.Y - 34)
                                e.health.Color = Color3.fromRGB(
                                    255 - math.floor((hum.Health/hum.MaxHealth)*255),
                                    math.floor((hum.Health/hum.MaxHealth)*255),
                                    0)
                                e.health.Visible = true
                            else e.health.Visible = false end

                            if CFG.Chams then
                                e.hl.Parent = p.Character
                                e.hl.FillColor = cor
                                e.hl.Enabled = true
                            else e.hl.Enabled = false end
                        else
                            e.box.Visible = false e.nome.Visible = false
                            e.dist.Visible = false e.health.Visible = false
                            e.hl.Enabled = false
                        end
                    end
                else
                    if ESPs[p] then
                        local e = ESPs[p]
                        e.box.Visible = false e.nome.Visible = false
                        e.dist.Visible = false e.health.Visible = false
                        e.hl.Enabled = false
                    end
                end
            else
                if ESPs[p] then
                    local e = ESPs[p]
                    e.box.Visible = false e.nome.Visible = false
                    e.dist.Visible = false e.health.Visible = false
                    e.hl.Enabled = false
                end
            end
        end
    end
end

-- ==================
