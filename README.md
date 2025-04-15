-- Hàm hấp thụ cấp độ
function absorbLevel(target)
    if target:FindFirstChild("Humanoid") then
        local targetLevel = target.Humanoid.Health  -- Bạn có thể thay đổi cách tính cấp độ tùy theo cấu trúc của game

        -- Không giới hạn cấp độ, chỉ cộng luôn vào người chơi
        local player = game.Players.LocalPlayer
        player.Level = player.Level + targetLevel  -- Cộng cấp độ của NPC vào người chơi
        print("Absorbed Level: " .. targetLevel)
    end
end

-- Hàm tìm mục tiêu (bạn có thể thay đổi cách tìm mục tiêu nếu cần)
function findTarget()
    for _, target in pairs(game.Workspace:GetChildren()) do
        if target:FindFirstChild("Humanoid") and target.Humanoid.Health > 0 then
            return target
        end
    end
    return nil
end

-- Hàm rơi đồ (thực tế cần xác định cách bạn muốn rơi đồ)
function dropLoot(target)
    local loot = Instance.new("Part", Workspace)
    loot.Size = Vector3.new(2, 2, 2)
    loot.Position = target.HumanoidRootPart.Position + Vector3.new(0, 5, 0)
    loot.Color = Color3.fromRGB(255, 215, 0)  -- Màu vàng đại diện cho loot
    loot.Anchored = true
    loot.CanCollide = false

    -- Tạo hiệu ứng ánh sáng cho loot
    local light = Instance.new("PointLight", loot)
    light.Color = Color3.fromRGB(255, 255, 0)  -- Màu vàng sáng
    light.Brightness = 5
    light.Range = 10

    -- Tạo âm thanh cho loot
    local sound = Instance.new("Sound", loot)
    sound.SoundId = "rbxassetid://987654321"  -- Thay ID âm thanh của bạn
    sound.Volume = 0.8
    sound:Play()

    print("Loot dropped!")
end

-- SUMMON DARK DRAGON V2 (Rồng Bóng Tối V2)
function summonDarkDragonV2()
    local dragon = Instance.new("Part", Workspace)
    dragon.Size = Vector3.new(20, 20, 20)
    dragon.Position = game.Players.LocalPlayer.Character.HumanoidRootPart.Position + Vector3.new(0, 5, 0)
    dragon.Color = Color3.fromRGB(0, 0, 0)
    dragon.Anchored = false
    dragon.CanCollide = false

    -- Tạo mesh rồng bóng tối
    local dragonMesh = Instance.new("SpecialMesh")
    dragonMesh.MeshType = Enum.MeshType.FileMesh
    dragonMesh.MeshId = "rbxassetid://123456789"  -- Mesh ID của rồng bóng tối
    dragonMesh.Parent = dragon

    -- Thêm hiệu ứng ánh sáng cho mắt rồng
    local eyeGlow = Instance.new("PointLight", dragon)
    eyeGlow.Color = Color3.fromRGB(255, 0, 0)
    eyeGlow.Range = 10
    eyeGlow.Brightness = 10

    -- Âm thanh triệu hồi rồng
    local sound = Instance.new("Sound", game.Players.LocalPlayer.Character)
    sound.SoundId = "rbxassetid://123456789"
    sound:Play()

    -- Di chuyển rồng đến mục tiêu
    local target = findTarget()
    if target then
        local bodyVelocityDown = Instance.new("BodyVelocity")
        bodyVelocityDown.MaxForce = Vector3.new(100000, 100000, 100000)
        bodyVelocityDown.Velocity = (target.HumanoidRootPart.Position - dragon.Position).unit * 100
        bodyVelocityDown.Parent = dragon

        -- Gây sát thương vô hạn
        while target.Humanoid.Health > 0 do
            target.Humanoid:TakeDamage(math.huge)  -- Sát thương vô hạn
            wait(0.1)
        end

        -- Luôn luôn rơi đồ nếu NPC có cấp độ > 10
        if target:FindFirstChild("Level") and target.Level > 10 then
            dropLoot(target)
        end

        -- Hấp thụ cấp độ từ NPC
        absorbLevel(target)
    end

    print("Dark Dragon V2 summoned!")
end
