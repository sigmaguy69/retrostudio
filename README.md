

local Rayfield = loadstring(game:HttpGet('https://sirius.menu/rayfield'))()

local Window = Rayfield:CreateWindow({
   Name = "RetroStudioFucker",
   LoadingTitle = "Retrostudio is Fucked.",
   LoadingSubtitle = "Made By FeinFeintravisscott2",
   ConfigurationSaving = { Enabled = false },
   KeySystem = false
})

local Tab = Window:CreateTab("Main", nil)
Tab:CreateSection("All of our stuff (only two things.)")

-- Button 1: Get Real Old UI (classic Roblox feel)
Tab:CreateButton({
   Name = "Remove The Offical ROBLOX UI And Only Have The RetroStudio One.",
   Callback = function()
      Rayfield:Notify({Title = "Loading...", Content = "Removing modern topbar...", Duration = 3})
      pcall(function() loadstring(game:HttpGet("https://rawscripts.net/raw/RetroStudio-Old-Topbar-Script-25916"))() end)
      task.wait(0.2)
      pcall(function() loadstring(game:HttpGet("https://rawscripts.net/raw/Universal-Script-script-i-made-for-retrostudio-Fixed-52532"))() end)
      Rayfield:Notify({Title = "Done", Content = "Classic topbar active - old Roblox vibes", Duration = 4})
   end,
})

-- Button 2: ACTIVATE RETRODESTROYER (fling yourself + everyone every 5 seconds)
Tab:CreateButton({
   Name = "ACTIVATE RETRODESTROYER sadly patched ):<",
   Callback = function()
      Rayfield:Notify({
         Title = "RETRODESTROYER ACTIVATING",
         Content = "Attempting to Destroy The Map, Pls wait 5 seconds, reset your character to stop.",
         Duration = 5
      })
      task.wait(5)
      
      local plr = game.Players.LocalPlayer
      local char = plr.Character or plr.CharacterAdded:Wait()
      local root = char:WaitForChild("HumanoidRootPart")
      local hats = {}
      
      for _, acc in pairs(char:GetChildren()) do
         if acc:IsA("Accessory") and acc:FindFirstChild("Handle") then
            local h = acc.Handle
            h.CanCollide = false
            h.Massless = true
            table.insert(hats, h)
         end
      end
      
      if #hats == 0 then
         Rayfield:Notify({Title = "No Hats", Content = "Patched.", Duration = 6})
         return
      end
      
      Rayfield:Notify({
         Title = "RETRODESTROYER ACTIVE",
         Content = "Your Fucking Destroying the map, Holy Shit!",
         Duration = 6
      })
      
      local rs = game:GetService("RunService")
      local angle = 0
      local nextFlingTime = tick() + 5
      
      rs.RenderStepped:Connect(function(dt)
         if not root or not root.Parent then return end
         angle = angle + dt * 450  -- insane fast spin for maximum chaos
         
         -- Constant chaotic orbit + self-fling
         for i, handle in ipairs(hats) do
            if not handle or not handle.Parent then continue end
            
            local offset = i / #hats * math.pi * 2
            local rad = angle + offset
            
            local radius = 6 + math.random(-4,4)
            local targetPos = root.Position + Vector3.new(
               math.cos(rad) * radius,
               8 + math.sin(angle * 8 + offset) * 8,
               math.sin(rad) * radius
            )
            
            local diff = targetPos - handle.Position
            handle.Velocity = diff * 300 + Vector3.new(
               math.random(-200,200),
               math.random(300,600),
               math.random(-200,200)
            ) + root.Velocity * 2.5
            
            handle.CFrame = handle.CFrame * CFrame.Angles(
               math.rad(math.random(-60,60)),
               math.rad(angle * 20),
               math.rad(math.random(-60,60))
            )
         end
         
         -- Every 5 seconds: teleport hats to random player & attempt to fling them
         if tick() >= nextFlingTime then
            local targets = {}
            for _, p in ipairs(game.Players:GetPlayers()) do
               if p ~= plr and p.Character and p.Character:FindFirstChild("HumanoidRootPart") and p.Character.Humanoid.Health > 0 then
                  table.insert(targets, p.Character.HumanoidRootPart)
               end
            end
            
            if #targets > 0 then
               local victimRoot = targets[math.random(1, #targets)]
               for _, handle in ipairs(hats) do
                  if handle.Parent then
                     local randOffset = Vector3.new(math.random(-5,5), math.random(2,6), math.random(-5,5))
                     handle.CFrame = victimRoot.CFrame * CFrame.new(randOffset)
                     handle.Velocity = Vector3.new(
                        math.random(-400,400),
                        math.random(500,900),
                        math.random(-400,400)
                     )
                  end
               end
               print("Fling attempt on: " .. victimRoot.Parent.Name)
            end
            
            nextFlingTime = tick() + 5  -- wait 5 seconds for next target
         end
      end)
      
      print("RetroDESTROYER running - server chaos engaged")
   end,
})

-- Cleanup button
Tab:CreateButton({
   Name = "Stop & Cleanup",
   Callback = function()
      for _, obj in ipairs(workspace:GetChildren()) do
         if obj:IsA("Part") and (obj.Name:match("Handle") or obj.Name:match("OrbitClone")) then
            obj:Destroy()
         end
      end
      Rayfield:Notify({Title = "Stopped", Content = "Everything Cleaned Up", Duration = 3})
   end,
})

Tab:CreateSection("")

print("RetroDESTROYER loaded - old UI + fling everyone chaos ready")
