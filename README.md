```lua
do
	local lastProc = 0
	local function frame_update_cb(self)
		if GetTime()-lastProc > 6 then
			self:Hide()
		end
	end

	local function frame_event_cb(self,event,...)
		if event == "PLAYER_ENTERING_WORLD" then
			self:Hide()
		elseif event == "COMBAT_TEXT_UPDATE" then
			local arg1 = ...
			if arg1 == "BLOCK" or arg1 == "PARRY" or arg1 == "DODGE" or arg1 == "MISS" then
				lastProc = GetTime()
				self:Show()
			end
		end
	end

	local frame = CreateFrame("frame",nil,UIParent)
	frame:SetPoint("CENTER",0,0)
	frame:SetSize(36,36)

	local texture = frame:CreateTexture(nil,"BORDER")
	texture:SetAllPoints()
	texture:SetTexture("Interface\\Icons\\Ability_Warrior_Revenge")

	frame:RegisterEvent("COMBAT_TEXT_UPDATE")
	frame:RegisterEvent("PLAYER_ENTERING_WORLD")
	frame:SetScript("OnEvent",frame_event_cb)
	frame:SetScript("OnUpdate",frame_update_cb)
end
```
