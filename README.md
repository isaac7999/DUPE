local player = game.Players.LocalPlayer
local gui = player:WaitForChild("PlayerGui")

-- Função de Anti-Ban Reforçado
local function ativarAntiBan()
    local palavrasBan = {"anticheat", "log", "detect", "report", "kick", "ban"}
    local function verificar(obj)
        for _, palavra in ipairs(palavrasBan) do
            if obj.Name:lower():find(palavra) then
                player:Kick("Você perdeu seu HP da média.")
                return true
            end
        end
    end

    -- Escaneamento inicial
    for _, obj in ipairs(game:GetDescendants()) do
        verificar(obj)
    end

    -- Escaneamento contínuo de novas ameaças
    game.DescendantAdded:Connect(function(obj)
        verificar(obj)
    end)

    -- Proteção contra remoção de ferramentas
    player.CharacterRemoving:Connect(function()
        player:Kick("Você perdeu seu HP da média.")
    end)

    -- Proteção contra remoção do botão
    task.spawn(function()
        while wait(2) do
            if not gui:FindFirstChild("DupePainelDiscreto") then
                player:Kick("Você perdeu seu HP da média.")
            end
        end
    end)
end

-- Inicia proteção
pcall(ativarAntiBan)

-- Botão disfarçado
local screenGui = Instance.new("ScreenGui", gui)
screenGui.Name = "DupePainelDiscreto"
screenGui.ResetOnSpawn = false

local dupeBtn = Instance.new("TextButton")
dupeBtn.Parent = screenGui
dupeBtn.Size = UDim2.new(0, 40, 0, 40)
dupeBtn.Position = UDim2.new(0.95, -45, 0.95, -45)
dupeBtn.BackgroundTransparency = 1
dupeBtn.Text = ""
dupeBtn.Name = "BotaoDiscreto"

local icone = Instance.new("Frame", dupeBtn)
icone.Size = UDim2.new(1, 0, 1, 0)
icone.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
icone.BorderSizePixel = 0
icone.BackgroundTransparency = 0.5
icone.Name = "Icone"

-- Função de duplicação (com delay e pcall)
local function duplicarTool()
    local slot = gui:FindFirstChild("BackpackNova")
        and gui.BackpackNova:FindFirstChild("Inventario")
        and gui.BackpackNova.Inventario:FindFirstChild("MyInv")
        and gui.BackpackNova.Inventario.MyInv:FindFirstChild("counteudo")
        and gui.BackpackNova.Inventario.MyInv.counteudo:FindFirstChild("Slot1")

    if slot and slot:FindFirstChild("In") and slot.In:FindFirstChildOfClass("Tool") then
        local tool = slot.In:FindFirstChildOfClass("Tool")
        local clone = tool:Clone()
        wait(math.random(0.8, 1.5)) -- Delay humano
        clone.Parent = slot.In
        print("Tool duplicada com sucesso!")
    else
        warn("Tool não encontrada no Slot1 > In")
    end
end

-- Clique seguro
dupeBtn.MouseButton1Click:Connect(function()
    pcall(duplicarTool)
end)
