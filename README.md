-- Référence à ReplicatedStorage
local ReplicatedStorage = game:GetService("ReplicatedStorage")

-- Créer ou obtenir une valeur Bool dans ReplicatedStorage pour activer/désactiver le spawn
local spawnEnabled = ReplicatedStorage:FindFirstChild("SpawnEnabled")
if not spawnEnabled then
    spawnEnabled = Instance.new("BoolValue")
    spawnEnabled.Name = "SpawnEnabled"
    spawnEnabled.Value = true  -- Initialement activé
    spawnEnabled.Parent = ReplicatedStorage
end

-- Fonction pour définir le point de spawn personnalisé
local function setCustomSpawn()
    local player = game.Players.LocalPlayer
    local character = player.Character or player.CharacterAdded:Wait()

    -- Attendre que le HumanoidRootPart soit disponible
    local rootPart = character:WaitForChild("HumanoidRootPart")

    -- Point de spawn personnalisé (ici défini comme la position actuelle du joueur)
    local spawnPos = rootPart.CFrame

    -- Lorsque le joueur respawn, le téléporter au point de spawn personnalisé
    player.CharacterAdded:Connect(function(char)
        if spawnEnabled.Value then
            local humanoidRootPart = char:WaitForChild("HumanoidRootPart")
            -- Déplacer le joueur au point de spawn
            humanoidRootPart.CFrame = spawnPos
            print("Téléporté à la position personnalisée.")
        end
    end)
end

-- Exécuter la fonction pour définir le point de spawn
setCustomSpawn()
