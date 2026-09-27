--[[
    ╔═══════════════════════════════════════════╗
    ║           ZetGames - Violence District     ║
    ║           Advanced Cheat Menu v1.0         ║
    ║           UI: Modern Purple Theme          ║
    ╚═══════════════════════════════════════════╝
--]]

local ZetGames = {}
ZetGames.Version = "1.0"
ZetGames.Theme = {
    Primary   = Color3.fromRGB(138, 43, 226),   -- Ungu
    Secondary = Color3.fromRGB(75, 0, 130),     -- Ungu gelap
    Accent    = Color3.fromRGB(186, 85, 211),   -- Orchid
    Background= Color3.fromRGB(20, 15, 30),     -- Hitam keunguan
    Text      = Color3.fromRGB(240, 230, 255),
    Toggle_On = Color3.fromRGB(0, 255, 170),
    Toggle_Off= Color3.fromRGB(80, 80, 100),
}

--// SERVICES
local Players          = game:GetService("Players")
local RunService       = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local TweenService     = game:GetService("TweenService")
local CoreGui          = game:GetService("CoreGui")
local Workspace        = game:GetService("Workspace")
local Lighting         = game:GetService("Lighting")

local LocalPlayer = Players.LocalPlayer
local Camera      = Workspace.CurrentCamera

--// HAPUS GUI LAMA
pcall(function()
    if CoreGui:FindFirstChild("ZetGamesUI") then
        CoreGui.ZetGamesUI:Destroy()
    end
end)

--// SCREEN GUI
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "ZetGamesUI"
ScreenGui.ResetOnSpawn = false
ScreenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
pcall(function() ScreenGui.Parent = CoreGui end)
if not ScreenGui.Parent then ScreenGui.Parent = LocalPlayer:WaitForChild("PlayerGui") end

--// STATE
local State = {
    ESP = false,
    ESP_Killer = false,
    ESP_Survivor = false,
    ESP_Generator = false,
    ESP_Hook = false,
    ESP_Pallet = false,
    ESP_Distance = false,
    AutoParry = false,
    AutoSkillCheck = false,
    SilentAim = false,
    GodMode = false,
    SpeedBoost = false,
    NoClip = false,
    Fullbright = false,
    AntiAFK = false,
    ThirdPerson = false,
    SpeedValue = 32,
    WalkSpeed = 16,
}

--// FUNGSI PEMBUAT UI
local function Create(className, props)
    local obj = Instance.new(className)
    for k, v in pairs(props) do
        if k ~= "Parent" then obj[k] = v end
    end
    if props.Parent then obj.Parent = props.Parent end
    return obj
end

local function Corner(parent, radius)
    return Create("UICorner", {CornerRadius = UDim.new(0, radius or 8), Parent = parent})
end

local function Stroke(parent, color, thickness)
    return Create("UIStroke", {
        Color = color or ZetGames.Theme.Primary,
        Thickness = thickness or 1.5,
        Parent = parent
    })
end

local function Gradient(parent, c1, c2, rot)
    return Create("UIGradient", {
        Color = ColorSequence.new(c1, c2),
        Rotation = rot or 90,
        Parent = parent
    })
end

--// MAIN FRAME
local Main = Create("Frame", {
    Name = "Main",
    Parent = ScreenGui,
    BackgroundColor3 = ZetGames.Theme.Background,
    BorderSizePixel = 0,
    Position = UDim2.new(0.5, -320, 0.5, -240),
    Size = UDim2.new(0, 640, 0, 480),
    Active = true,
    Draggable = true,
})
Corner(Main, 12)
Stroke(Main, ZetGames.Theme.Primary, 2)

--// HEADER
local Header = Create("Frame", {
    Parent = Main,
    BackgroundColor3 = ZetGames.Theme.Secondary,
    BorderSizePixel = 0,
    Size = UDim2.new(1, 0, 0, 50),
})
Corner(Header, 12)
Create("Frame", {
    Parent = Header,
    BackgroundColor3 = ZetGames.Theme.Secondary,
    BorderSizePixel = 0,
    Position = UDim2.new(0, 0, 0.6, 0),
    Size = UDim2.new(1, 0, 0.5, 0),
})
Gradient(Header, ZetGames.Theme.Primary, ZetGames.Theme.Secondary, 90)

local Title = Create("TextLabel", {
    Parent = Header,
    BackgroundTransparency = 1,
    Size = UDim2.new(0.7, 0, 1, 0),
    Position = UDim2.new(0, 60, 0, 0),
    Font = Enum.Font.GothamBold,
    Text = "ZetGames",
    TextColor3 = ZetGames.Theme.Text,
    TextSize = 22,
    TextXAlignment = Enum.TextXAlignment.Left,
})
Create("TextLabel", {
    Parent = Header,
    BackgroundTransparency = 1,
    Size = UDim2.new(0.7, 0, 1, 0),
    Position = UDim2.new(0, 165, 0, 0),
    Font = Enum.Font.Gotham,
    Text = "| Violence District",
    TextColor3 = Color3.fromRGB(200, 180, 240),
    TextSize = 14,
    TextXAlignment = Enum.TextXAlignment.Left,
})

-- Icon kecil
local Icon = Create("Frame", {
    Parent = Header,
    BackgroundColor3 = ZetGames.Theme.Accent,
    Position = UDim2.new(0, 15, 0.5, -14),
    Size = UDim2.new(0, 28, 0, 28),
})
Corner(Icon, 8)
Create("TextLabel", {
    Parent = Icon,
    BackgroundTransparency = 1,
    Size = UDim2.new(1, 0, 1, 0),
    Font = Enum.Font.GothamBold,
    Text = "Z",
    TextColor3 = Color3.new(1,1,1),
    TextSize = 18,
})

-- Tombol Close
local CloseBtn = Create("TextButton", {
    Parent = Header,
    BackgroundColor3 = Color3.fromRGB(220, 50, 80),
    Position = UDim2.new(1, -40, 0.5, -13),
    Size = UDim2.new(0, 26, 0, 26),
    Font = Enum.Font.GothamBold,
    Text = "X",
    TextColor3 = Color3.new(1,1,1),
    TextSize = 14,
    AutoButtonColor = true,
})
Corner(CloseBtn, 6)
CloseBtn.MouseButton1Click:Connect(function()
    ScreenGui:Destroy()
end)

-- Tombol Minimize
local MinBtn = Create("TextButton", {
    Parent = Header,
    BackgroundColor3 = Color3.fromRGB(255, 180, 50),
    Position = UDim2.new(1, -72, 0.5, -13),
    Size = UDim2.new(0, 26, 0, 26),
    Font = Enum.Font.GothamBold,
    Text = "-",
    TextColor3 = Color3.new(1,1,1),
    TextSize = 16,
})
Corner(MinBtn, 6)

--// SIDEBAR
local Sidebar = Create("Frame", {
    Parent = Main,
    BackgroundColor3 = ZetGames.Theme.Secondary,
    BorderSizePixel = 0,
    Position = UDim2.new(0, 10, 0, 60),
    Size = UDim2.new(0, 150, 1, -75),
})
Corner(Sidebar, 10)
Stroke(Sidebar, ZetGames.Theme.Primary, 1)

local SidebarLayout = Create("UIListLayout", {
    Parent = Sidebar,
    Padding = UDim.new(0, 6),
    HorizontalAlignment = Enum.HorizontalAlignment.Center,
    SortOrder = Enum.SortOrder.LayoutOrder,
})
Create("UIPadding", {
    Parent = Sidebar,
    PaddingTop = UDim.new(0, 10),
})

--// CONTENT AREA
local Content = Create("Frame", {
    Parent = Main,
    BackgroundColor3 = ZetGames.Theme.Secondary,
    BorderSizePixel = 0,
    Position = UDim2.new(0, 170, 0, 60),
    Size = UDim2.new(1, -185, 1, -75),
})
Corner(Content, 10)
Stroke(Content, ZetGames.Theme.Primary, 1)

local ContentPadding = Create("UIPadding", {
    Parent = Content,
    PaddingTop = UDim.new(0, 15),
    PaddingLeft = UDim.new(0, 15),
    PaddingRight = UDim.new(0, 10),
    PaddingBottom = UDim.new(0, 15),
})

--// PAGE SYSTEM
local Pages = {}
local CurrentTab = nil

local function CreatePage(name)
    local Scroll = Create("ScrollingFrame", {
        Parent = Content,
        BackgroundTransparency = 1,
        Size = UDim2.new(1, 0, 1, 0),
        CanvasSize = UDim2.new(0, 0, 0, 0),
        ScrollBarThickness = 4,
        ScrollBarImageColor3 = ZetGames.Theme.Accent,
        Visible = false,
        AutomaticCanvasSize = Enum.AutomaticSize.Y,
    })
    Create("UIListLayout", {
        Parent = Scroll,
        Padding = UDim.new(0, 8),
        SortOrder = Enum.SortOrder.LayoutOrder,
    })
    Pages[name] = Scroll
    return Scroll
end

local function SwitchTab(name)
    for n, page in pairs(Pages) do
        page.Visible = (n == name)
    end
    CurrentTab = name
    for _, btn in pairs(Sidebar:GetChildren()) do
        if btn:IsA("TextButton") then
            if btn.Name == name then
                TweenService:Create(btn, TweenInfo.new(0.2), {
                    BackgroundColor3 = ZetGames.Theme.Primary
                }):Play()
            else
                TweenService:Create(btn, TweenInfo.new(0.2), {
                    BackgroundColor3 = Color3.fromRGB(45, 25, 70)
                }):Play()
            end
        end
    end
end

local function CreateTabButton(name, displayName)
    local Btn = Create("TextButton", {
        Parent = Sidebar,
        Name = name,
        BackgroundColor3 = Color3.fromRGB(45, 25, 70),
        Size = UDim2.new(0.85, 0, 0, 38),
        Font = Enum.Font.GothamMedium,
        Text = displayName,
        TextColor3 = ZetGames.Theme.Text,
        TextSize = 14,
        AutoButtonColor = false,
    })
    Corner(Btn, 8)
    Btn.MouseButton1Click:Connect(function()
        SwitchTab(name)
    end)
    Btn.MouseEnter:Connect(function()
        if CurrentTab ~= name then
            TweenService:Create(Btn, TweenInfo.new(0.15), {
                BackgroundColor3 = Color3.fromRGB(70, 40, 110)
            }):Play()
        end
    end)
    Btn.MouseLeave:Connect(function()
        if CurrentTab ~= name then
            TweenService:Create(Btn, TweenInfo.new(0.15), {
                BackgroundColor3 = Color3.fromRGB(45, 25, 70)
            }):Play()
        end
    end)
    return Btn
end

--// WIDGET PEMBUAT
local function CreateSection(parent, title)
    local Section = Create("Frame", {
        Parent = parent,
        BackgroundColor3 = Color3.fromRGB(35, 20, 55),
        Size = UDim2.new(1, -8, 0, 40),
        BorderSizePixel = 0,
    })
    Corner(Section, 8)
    Stroke(Section, ZetGames.Theme.Accent, 1)
    Create("TextLabel", {
        Parent = Section,
        BackgroundTransparency = 1,
        Position = UDim2.new(0, 12, 0, 0),
        Size = UDim2.new(1, -20, 1, 0),
        Font = Enum.Font.GothamBold,
        Text = title,
        TextColor3 = ZetGames.Theme.Accent,
        TextSize = 14,
        TextXAlignment = Enum.TextXAlignment.Left,
    })
    local Layout = Create("Frame", {
        Parent = parent,
        BackgroundTransparency = 1,
        Size = UDim2.new(1, -8, 0, 0),
        AutomaticSize = Enum.AutomaticSize.Y,
    })
    Create("UIListLayout", {
        Parent = Layout,
        Padding = UDim.new(0, 6),
        SortOrder = Enum.SortOrder.LayoutOrder,
    })
    return Layout
end

local function CreateToggle(parent, text, key, callback)
    local Btn = Create("TextButton", {
        Parent = parent,
        BackgroundColor3 = Color3.fromRGB(50, 30, 75),
        Size = UDim2.new(1, 0, 0, 34),
        Font = Enum.Font.Gotham,
        Text = "",
        AutoButtonColor = false,
    })
    Corner(Btn, 6)

    local Label = Create("TextLabel", {
        Parent = Btn,
        BackgroundTransparency = 1,
        Position = UDim2.new(0, 12, 0, 0),
        Size = UDim2.new(0.7, 0, 1, 0),
        Font = Enum.Font.Gotham,
        Text = text,
        TextColor3 = ZetGames.Theme.Text,
        TextSize = 13,
        TextXAlignment = Enum.TextXAlignment.Left,
    })

    local Indicator = Create("Frame", {
        Parent = Btn,
        BackgroundColor3 = ZetGames.Theme.Toggle_Off,
        Position = UDim2.new(1, -50, 0.5, -10),
        Size = UDim2.new(0, 40, 0, 20),
    })
    Corner(Indicator, 10)

    local Knob = Create("Frame", {
        Parent = Indicator,
        BackgroundColor3 = Color3.fromRGB(255, 255, 255),
        Position = UDim2.new(0, 2, 0.5, -8),
        Size = UDim2.new(0, 16, 0, 16),
        ZIndex = 2,
    })
    Corner(Knob, 8)

    local enabled = false

    local function Toggle()
        enabled = not enabled
        State[key] = enabled
        TweenService:Create(Indicator, TweenInfo.new(0.2), {
            BackgroundColor3 = enabled and ZetGames.Theme.Toggle_On or ZetGames.Theme.Toggle_Off
        }):Play()
        TweenService:Create(Knob, TweenInfo.new(0.2), {
            Position = enabled and UDim2.new(1, -18, 0.5, -8) or UDim2.new(0, 2, 0.5, -8)
        }):Play()
        if callback then callback(enabled) end
    end

    Btn.MouseButton1Click:Connect(Toggle)
    return Btn
end

local function CreateSlider(parent, text, min, max, default, key, callback)
    local Frame = Create("Frame", {
        Parent = parent,
        BackgroundColor3 = Color3.fromRGB(50, 30, 75),
        Size = UDim2.new(1, 0, 0, 50),
    })
    Corner(Frame, 6)

    Create("TextLabel", {
        Parent = Frame,
        BackgroundTransparency = 1,
        Position = UDim2.new(0, 12, 0, 4),
        Size = UDim2.new(0.6, 0, 0, 18),
        Font = Enum.Font.Gotham,
        Text = text,
        TextColor3 = ZetGames.Theme.Text,
        TextSize = 12,
        TextXAlignment = Enum.TextXAlignment.Left,
    })

    local ValueLabel = Create("TextLabel", {
        Parent = Frame,
        BackgroundTransparency = 1,
        Position = UDim2.new(0.7, 0, 0, 4),
        Size = UDim2.new(0.28, 0, 0, 18),
        Font = Enum.Font.GothamBold,
        Text = tostring(default),
        TextColor3 = ZetGames.Theme.Accent,
        TextSize = 12,
        TextXAlignment = Enum.TextXAlignment.Right,
    })

    local Bar = Create("Frame", {
        Parent = Frame,
        BackgroundColor3 = Color3.fromRGB(30, 20, 45),
        Position = UDim2.new(0, 12, 0, 30),
        Size = UDim2.new(1, -24, 0, 8),
    })
    Corner(Bar, 4)

    local Fill = Create("Frame", {
        Parent = Bar,
        BackgroundColor3 = ZetGames.Theme.Primary,
        Size = UDim2.new((default - min) / (max - min), 0, 1, 0),
    })
    Corner(Fill, 4)
    Gradient(Fill, ZetGames.Theme.Primary, ZetGames.Theme.Accent, 0)

    local dragging = false

    local function UpdateValue(input)
        local rel = math.clamp((input.Position.X - Bar.AbsolutePosition.X) / Bar.AbsoluteSize.X, 0, 1)
        local val = math.floor(min + (max - min) * rel)
        ValueLabel.Text = tostring(val)
        Fill.Size = UDim2.new(rel, 0, 1, 0)
        State[key] = val
        if callback then callback(val) end
    end

    Bar.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
            dragging = true
            UpdateValue(input)
        end
    end)
    UserInputService.InputChanged:Connect(function(input)
        if dragging and (input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch) then
            UpdateValue(input)
        end
    end)
    UserInputService.InputEnded:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
            dragging = false
        end
    end)
    return Frame
end

local function CreateButton(parent, text, callback)
    local Btn = Create("TextButton", {
        Parent = parent,
        BackgroundColor3 = ZetGames.Theme.Primary,
        Size = UDim2.new(1, 0, 0, 34),
        Font = Enum.Font.GothamBold,
        Text = text,
        TextColor3 = Color3.new(1,1,1),
        TextSize = 13,
        AutoButtonColor = false,
    })
    Corner(Btn, 6)
    Gradient(Btn, ZetGames.Theme.Primary, ZetGames.Theme.Accent, 0)
    Btn.MouseEnter:Connect(function()
        TweenService:Create(Btn, TweenInfo.new(0.15), {BackgroundColor3 = ZetGames.Theme.Accent}):Play()
    end)
    Btn.MouseLeave:Connect(function()
        TweenService:Create(Btn, TweenInfo.new(0.15), {BackgroundColor3 = ZetGames.Theme.Primary}):Play()
    end)
    Btn.MouseButton1Click:Connect(callback)
    return Btn
end

--// BUAT TAB
CreateTabButton("Combat", "⚔ Combat")
CreateTabButton("Visual", "👁 Visual")
CreateTabButton("Movement", "🏃 Movement")
CreateTabButton("Farm", "🌾 Farm")
CreateTabButton("Misc", "⚙ Misc")

CreatePage("Combat")
CreatePage("Visual")
CreatePage("Movement")
CreatePage("Farm")
CreatePage("Misc")

--// TAB COMBAT
local Combat = Pages["Combat"]
local s1 = CreateSection(Combat, "⚔ AUTO COMBAT")
CreateToggle(s1, "Auto Parry", "AutoParry", function(v)
    -- Placeholder logika
end)
CreateToggle(s1, "Silent Aim", "SilentAim", function(v)
    -- Placeholder logika
end)
CreateToggle(s1, "God Mode", "GodMode", function(v)
    if v and LocalPlayer.Character then
        local hum = LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
        if hum then hum.MaxHealth = math.huge; hum.Health = math.huge end
    end
end)

--// TAB VISUAL
local Visual = Pages["Visual"]
local s2 = CreateSection(Visual, "👁 ESP SYSTEM")
CreateToggle(s2, "ESP Killer", "ESP_Killer", function(v) end)
CreateToggle(s2, "ESP Survivor", "ESP_Survivor", function(v) end)
CreateToggle(s2, "ESP Generator", "ESP_Generator", function(v) end)
CreateToggle(s2, "ESP Hook", "ESP_Hook", function(v) end)
CreateToggle(s2, "ESP Pallet", "ESP_Pallet", function(v) end)
CreateToggle(s2, "Show Distance", "ESP_Distance", function(v) end)

local s2b = CreateSection(Visual, "🌍 ENVIRONMENT")
CreateToggle(s2b, "Fullbright", "Fullbright", function(v)
    if v then
        Lighting.Brightness = 3
        Lighting.ClockTime = 14
        Lighting.FogEnd = 100000
        Lighting.GlobalShadows = false
    else
        Lighting.Brightness = 2
        Lighting.FogEnd = 100000
        Lighting.GlobalShadows = true
    end
end)

--// TAB MOVEMENT
local Movement = Pages["Movement"]
local s3 = CreateSection(Movement, "🏃 SPEED & JUMP")
CreateSlider(s3, "WalkSpeed", 16, 200, 16, "WalkSpeed", function(v)
    if LocalPlayer.Character then
        local hum = LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
        if hum then hum.WalkSpeed = v end
    end
end)
CreateToggle(s3, "Speed Boost", "SpeedBoost", function(v)
    if LocalPlayer.Character then
        local hum = LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
        if hum then hum.WalkSpeed = v and 50 or 16 end
    end
end)
CreateToggle(s3, "No Clip", "NoClip", function(v) end)
CreateToggle(s3, "Third Person", "ThirdPerson", function(v)
    LocalPlayer.CameraMode = v and Enum.CameraMode.Classic or Enum.CameraMode.LockFirstPerson
end)

--// TAB FARM
local Farm = Pages["Farm"]
local s4 = CreateSection(Farm, "🌾 AUTO FARM")
CreateToggle(s4, "Auto Skill Check", "AutoSkillCheck", function(v) end)
CreateButton(s4, "Auto Complete Generator", function()
    -- Placeholder
end)
CreateButton(s4, "Instant Escape", function()
    -- Placeholder
end)
CreateButton(s4, "Teleport ke Generator Terdekat", function()
    -- Placeholder
end)

--// TAB MISC
local Misc = Pages["Misc"]
local s5 = CreateSection(Misc, "⚙ UTILITY")
CreateToggle(s5, "Anti AFK", "AntiAFK", function(v) end)
CreateButton(s5, "Rejoin Server", function()
    game:GetService("TeleportService"):Teleport(game.PlaceId, LocalPlayer)
end)
CreateButton(s5, "Server Hop", function()
    local Http = game:GetService("HttpService")
    local TPS = game:GetService("TeleportService")
    local servers = Http:JSONDecode(game:HttpGet("https://games.roblox.com/v1/games/"..game.PlaceId.."/servers/Public?sortOrder=Asc&limit=100"))
    for _, srv in pairs(servers.data) do
        if srv.playing < srv.maxPlayers and srv.id ~= game.JobId then
            TPS:TeleportToPlaceInstance(game.PlaceId, srv.id, LocalPlayer)
            break
        end
    end
end)
CreateButton(s5, "Destroy GUI", function()
    ScreenGui:Destroy()
end)

--// INFO BAWAH
Create("TextLabel", {
    Parent = Main,
    BackgroundTransparency = 1,
    Position = UDim2.new(0, 170, 1, -22),
    Size = UDim2.new(1, -185, 0, 18),
    Font = Enum.Font.Gotham,
    Text = "ZetGames v"..ZetGames.Version.."  •  Violence District  •  By Admin",
    TextColor3 = Color3.fromRGB(160, 140, 200),
    TextSize = 10,
    TextXAlignment = Enum.TextXAlignment.Right,
})

--// INISIALISASI
SwitchTab("Combat")

--// FUNGSI CLOSE/OPEN MINIMIZE
local minimized = false
local originalSize = Main.Size
MinBtn.MouseButton1Click:Connect(function()
    minimized = not minimized
    TweenService:Create(Main, TweenInfo.new(0.3, Enum.EasingStyle.Quart), {
        Size = minimized and UDim2.new(0, 300, 0, 50) or originalSize
    }):Play()
end)

--// ANTI AFK
task.spawn(function()
    while task.wait(60) do
        if State.AntiAFK then
            pcall(function()
                game:GetService("VirtualUser"):CaptureController()
                game:GetService("VirtualUser"):ClickButton2(Vector2.new())
            end)
        end
    end
end)

--// NO CLIP LOOP
RunService.Stepped:Connect(function()
    if State.NoClip and LocalPlayer.Character then
        for _, part in pairs(LocalPlayer.Character:GetDescendants()) do
            if part:IsA("BasePart") and part.CanCollide then
                part.CanCollide = false
            end
        end
    end
end)

print("[ZetGames] UI berhasil dimuat! Version: "..ZetGames.Version)
