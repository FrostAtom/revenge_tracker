```lua
do
	local GetTime = GetTime
	local GetSpellInfo = GetSpellInfo
	local revenge

	local lastProc = 0
	local function frame_update_cb(self)
		if GetTime()-lastProc > 4 then
			self:Hide()
		end
	end

	local function frame_event_cb(self,event,...)
		if event == "PLAYER_ENTERING_WORLD" then
			if not revenge then revenge = GetSpellInfo(57823) end
			self:Hide()
		elseif event == "UNIT_SPELLCAST_SUCCEEDED" then
			local unitID,spellName = ...
			if revenge and unitID == "player" and spellName == revenge then
				self:Hide()
			end
		elseif event == "COMBAT_TEXT_UPDATE" then
			local arg1 = ...
			if arg1 == "BLOCK" or arg1 == "PARRY" or arg1 == "DODGE" then
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

	frame:RegisterEvent("PLAYER_ENTERING_WORLD")
	frame:RegisterEvent("UNIT_SPELLCAST_SUCCEEDED")
	frame:RegisterEvent("COMBAT_TEXT_UPDATE")
	frame:SetScript("OnEvent",frame_event_cb)
	frame:SetScript("OnUpdate",frame_update_cb)
end

```
