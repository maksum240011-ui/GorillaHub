--[[
    Created by sozokier
    Verified by sozokier
]]

local P, R, V = game:GetService("Players"), game:GetService("RunService"), game:GetService("VirtualUser")
local TS = game:GetService("TweenService")
local lp = P.LocalPlayer
local pg = lp:WaitForChild("PlayerGui")

if pg:FindFirstChild("GorillaHubPRO") then pg.GorillaHubPRO:Destroy() end
pcall(function() R:Set3dRenderingEnabled(true) end)

local KEY, lang = "0011011110", "RU"
local T = {
	RU = {
		load = "Загрузка Gorilla Hub PRO...",
		t = "Авторизация",
		s = "Создал и проверил: sozokier",
		p = "Введите пароль...",
		b = "Проверить",
		e = "Неверный пароль!",
		o = "Добро пожаловать, " .. lp.Name .. "!"
	},
	EN = {
		load = "Loading Gorilla Hub PRO...",
		t = "Authorization",
		s = "Created & Verified: sozokier",
		p = "Enter password...",
		b = "Check",
		e = "Wrong password!",
		o = "Welcome, " .. lp.Name .. "!"
	}
}

local gui = Instance.new("ScreenGui", pg)
gui.Name = "GorillaHubPRO"
gui.ResetOnSpawn = false

local rgb = {}
task.spawn(function()
	local h = 0
	while gui.Parent do
		h = (h + 0.005) % 1
		local c = Color3.fromHSV(h, 0.85, 1)
		for _, s in ipairs(rgb) do if s and s.Parent then s.Color = c end end
		task.wait(0.03)
	end
end)

local function st(o, r, sc, th, isRgb)
	local c = Instance.new("UICorner", o)
	c.CornerRadius = UDim.new(0, r)
	if sc then
		local s = Instance.new("UIStroke", o)
		s.Color = sc
		s.Thickness = th or 1.5
		s.ApplyStrokeMode = Enum.ApplyStrokeMode.Border
		if isRgb then table.insert(rgb, s) end
		return s
	end
end

local function mk(cls, par, sz, pos, bg, txt, tc, ts)
	local o = Instance.new(cls, par)
	o.Size = sz
	o.Position = pos
	if bg then o.BackgroundColor3 = bg else o.BackgroundTransparency = 1 end
	if txt ~= nil then
		o.Text = txt
		o.TextColor3 = tc or Color3.new(1, 1, 1)
		o.TextSize = ts or 13
		o.Font = Enum.Font.GothamBold
	end
	return o
end

local OFF, ON = Color3.fromRGB(28, 31, 44), Color3.fromRGB(20, 135, 85)
local OFF_S, ON_S = Color3.fromRGB(55, 60, 80), Color3.fromRGB(50, 255, 160)

-- 3. Основной хаб
local function startHub()
	local tpOn, afOn, akOn, r3dOn, onlyT, min = false, true, true, true, false, false
	local tName, dist, delay, loops = "", 0, 0.1, {}

	lp.Idled:Connect(function()
		if akOn then V:CaptureController() V:ClickButton2(Vector2.new()) end
	end)

	local badge = mk("TextLabel", gui, UDim2.new(0, 165, 0, 26), UDim2.new(0, 115, 0, 8), Color3.fromRGB(16, 18, 26), "FPS: 60 | Ping: 50ms", Color3.fromRGB(0, 255, 180), 13)
	badge.BackgroundTransparency = 0.15
	badge.Active = true
	badge.Draggable = true
	st(badge, 8, Color3.fromRGB(0, 220, 255), 1.5, true)

	local fc = 0
	R.RenderStepped:Connect(function() fc = fc + 1 end)
	task.spawn(function()
		while gui.Parent do
			task.wait(0.5)
			local ping = 0
			pcall(function() ping = math.floor(lp:GetNetworkPing() * 1000) end)
			badge.Text = "FPS: " .. (fc * 2) .. " | Ping: " .. ping .. "ms"
			fc = 0
		end
	end)

	local main = mk("Frame", gui, UDim2.new(0, 246, 0, 364), UDim2.new(0.04, 0, 0.14, 0), Color3.fromRGB(16, 18, 25))
	main.Active = true
	main.Draggable = true
	st(main, 12, Color3.fromRGB(0, 220, 255), 2, true)

	local top = mk("Frame", main, UDim2.new(1, 0, 0, 34), UDim2.new(0, 0, 0, 0), Color3.fromRGB(24, 27, 38))
	st(top, 12)
	local title = mk("TextLabel", top, UDim2.new(1, -38, 1, 0), UDim2.new(0, 12, 0, 0), nil, "* Gorilla Hub PRO", Color3.fromRGB(0, 240, 210), 15)
	title.TextXAlignment = Enum.TextXAlignment.Left

	local minB = mk("TextButton", top, UDim2.new(0, 26, 0, 24), UDim2.new(1, -31, 0, 5), Color3.fromRGB(38, 42, 58), "-", nil, 16)
	st(minB, 6)
	local cont = mk("Frame", main, UDim2.new(1, -18, 1, -42), UDim2.new(0, 9, 0, 38))

	local function btn(txt, y, col, sc)
		local b = mk("TextButton", cont, UDim2.new(1, 0, 0, 34), UDim2.new(0, 0, 0, y), col, txt, nil, 13)
		return b, st(b, 8, sc or OFF_S, 1)
	end

	local tpB, tpS = btn("Teleport: OFF", 0, OFF)
	local mdB, mdS = btn("Mode: All Players", 40, OFF)
	local tBox = mk("TextBox", cont, UDim2.new(1, 0, 0, 32), UDim2.new(0, 0, 0, 80), Color3.fromRGB(22, 25, 36), "", nil, 13)
	tBox.PlaceholderText = "Target Name..."
	tBox.ClearTextOnFocus = false
	st(tBox, 8, OFF_S, 1)

	local spB = btn("Speed: 0.1s", 118, OFF)
	local afB, afS = btn("Anti-Fling: ON", 158, ON, ON_S)
	local akB, akS = btn("Anti-Kick: ON", 198, ON, ON_S)
	local r3B, r3S = btn("3D Render: ON", 238, ON, ON_S)

	local dLbl = mk("TextLabel", cont, UDim2.new(0.55, 0, 0, 26), UDim2.new(0, 4, 0, 276), nil, "Distance (studs):", Color3.fromRGB(170, 180, 200), 13)
	dLbl.TextXAlignment = Enum.TextXAlignment.Left
	local dBox = mk("TextBox", cont, UDim2.new(0.42, 0, 0, 26), UDim2.new(0.58, 0, 0, 276), Color3.fromRGB(22, 25, 36), "0", Color3.fromRGB(0, 240, 210), 14)
	st(dBox, 8, OFF_S, 1)

	mk("TextLabel", cont, UDim2.new(1, 0, 0, 16), UDim2.new(0, 0, 0, 305), nil, "Created & Verified by sozokier", Color3.fromRGB(115, 125, 145), 11)

	minB.MouseButton1Click:Connect(function()
		min = not min
		cont.Visible = not min
		main.Size = min and UDim2.new(0, 246, 0, 34) or UDim2.new(0, 246, 0, 364)
		minB.Text = min and "+" or "-"
	end)

	local function clearL(p) if loops[p] then loops[p]:Disconnect() loops[p] = nil end end
	local function setChar(p, c)
		clearL(p)
		loops[p] = R.Stepped:Connect(function()
			if not afOn then return end
			if not c or not c.Parent then clearL(p) return end
			for _, pt in ipairs(c:GetChildren()) do if pt:IsA("BasePart") and pt.CanCollide then pt.CanCollide = false end end
		end)
	end
	for _, p in ipairs(P:GetPlayers()) do if p ~= lp then p.CharacterAdded:Connect(function(c) setChar(p, c) end) if p.Character then setChar(p, p.Character) end end end
	P.PlayerAdded:Connect(function(p) if p ~= lp then p.CharacterAdded:Connect(function(c) setChar(p, c) end) end end)
	P.PlayerRemoving:Connect(clearL)

	task.spawn(function()
		while gui.Parent do
			if tpOn then
				local hrp = lp.Character and lp.Character:FindFirstChild("HumanoidRootPart")
				if hrp then
					local cf = hrp.CFrame * CFrame.new(0, 0, -dist)
					for _, p in ipairs(P:GetPlayers()) do
						if p ~= lp and p.Character then
							local q = tName:lower()
							if not onlyT or (q ~= "" and (p.Name:lower():sub(1, #q) == q or p.DisplayName:lower():sub(1, #q) == q)) then
								local t = p.Character:FindFirstChild("HumanoidRootPart")
								if t then t.AssemblyLinearVelocity = Vector3.zero t.AssemblyAngularVelocity = Vector3.zero t.CFrame = cf end
							end
						end
					end
				end
			end
			task.wait(delay)
		end
	end)

	tpB.MouseButton1Click:Connect(function() tpOn = not tpOn tpB.Text = tpOn and "Teleport: ON" or "Teleport: OFF" tpB.BackgroundColor3 = tpOn and ON or OFF tpS.Color = tpOn and ON_S or OFF_S end)
	mdB.MouseButton1Click:Connect(function() onlyT = not onlyT mdB.Text = onlyT and "Mode: Target Only" or "Mode: All Players" mdB.BackgroundColor3 = onlyT and Color3.fromRGB(35, 95, 190) or OFF mdS.Color = onlyT and Color3.fromRGB(90, 170, 255) or OFF_S end)
	tBox:GetPropertyChangedSignal("Text"):Connect(function() tName = tBox.Text end)
	local sps, idx = {0.1, 0.3, 0.6}, 1
	spB.MouseButton1Click:Connect(function() idx = (idx % 3) + 1 delay = sps[idx] spB.Text = "Speed: " .. delay .. "s" end)
	afB.MouseButton1Click:Connect(function() afOn = not afOn afB.Text = afOn and "Anti-Fling: ON" or "Anti-Fling: OFF" afB.BackgroundColor3 = afOn and ON or OFF afS.Color = afOn and ON_S or OFF_S end)
	akB.MouseButton1Click:Connect(function() akOn = not akOn akB.Text = akOn and "Anti-Kick: ON" or "Anti-Kick: OFF" akB.BackgroundColor3 = akOn and ON or OFF akS.Color = akOn and ON_S or OFF_S end)
	r3B.MouseButton1Click:Connect(function()
		r3dOn = not r3dOn pcall(function() R:Set3dRenderingEnabled(r3dOn) end)
		r3B.Text = r3dOn and "3D Render: ON" or "3D Render: OFF"
		r3B.BackgroundColor3 = r3dOn and ON or Color3.fromRGB(165, 45, 55)
		r3S.Color = r3dOn and ON_S or Color3.fromRGB(255, 100, 110)
	end)
	dBox.FocusLost:Connect(function() local n = tonumber(dBox.Text) if n then dist = n else dBox.Text = tostring(dist) end end)

	task.spawn(function()
		pcall(function()
			if hookmetamethod and getnamecallmethod then
				local old
				local fn = function(self, ...)
					if akOn and self == lp and (getnamecallmethod() == "Kick" or getnamecallmethod() == "kick") then return nil end
					return old(self, ...)
				end
				if newcclosure then fn = newcclosure(fn) end
				old = hookmetamethod(game, "__namecall", fn)
			end
		end)
	end)
end

-- 2. Окно авторизации
local function showAuth()
	local auth = mk("Frame", gui, UDim2.new(0, 296, 0, 192), UDim2.new(0.5, -148, 0.5, -96), Color3.fromRGB(16, 18, 25))
	auth.Active = true
	auth.Draggable = true
	st(auth, 12, Color3.fromRGB(0, 220, 255), 2, true)

	local aTop = mk("Frame", auth, UDim2.new(1, 0, 0, 34), UDim2.new(0, 0, 0, 0), Color3.fromRGB(24, 27, 38))
	st(aTop, 12)
	local aTitle = mk("TextLabel", aTop, UDim2.new(1, -85, 1, 0), UDim2.new(0, 12, 0, 0), nil, T[lang].t, Color3.fromRGB(0, 240, 210), 14)
	aTitle.TextXAlignment = Enum.TextXAlignment.Left

	local ruB = mk("TextButton", aTop, UDim2.new(0, 32, 0, 22), UDim2.new(1, -72, 0, 6), Color3.fromRGB(240, 245, 255), "RU", Color3.fromRGB(15, 18, 25), 11)
	st(ruB, 6)
	local enB = mk("TextButton", aTop, UDim2.new(0, 32, 0, 22), UDim2.new(1, -36, 0, 6), Color3.fromRGB(38, 42, 58), "EN", Color3.fromRGB(180, 190, 210), 11)
	st(enB, 6)

	local aSub = mk("TextLabel", auth, UDim2.new(1, -20, 0, 18), UDim2.new(0, 10, 0, 40), nil, T[lang].s, Color3.fromRGB(160, 170, 190), 11)
	local keyBox = mk("TextBox", auth, UDim2.new(1, -24, 0, 38), UDim2.new(0, 12, 0, 64), Color3.fromRGB(24, 27, 38), "", Color3.fromRGB(0, 240, 210), 14)
	keyBox.PlaceholderText = T[lang].p
	keyBox.ClearTextOnFocus = false
	st(keyBox, 8, Color3.fromRGB(65, 72, 95), 1.2)

	local stLbl = mk("TextLabel", auth, UDim2.new(1, -24, 0, 18), UDim2.new(0, 12, 0, 106), nil, "", Color3.fromRGB(255, 85, 95), 12)

