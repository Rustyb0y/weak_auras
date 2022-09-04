# Running Clock

`function(unit, unitOwner)

    if UnitAffectingCombat(unit) then
        if not combatActive then
            startTime = GetTime()
            runningClock = GetTime() - startTime
            combatActive = true
            outputText = True    
        else
            runningClock = GetTime() - startTime
        end

        initialRun = true
    else
        combatActive = false
    end
    
    if not initialRun then
        return ""
    else
        if not runningClock then
            return ""
        else
            return format("%01.f:%02.f", floor(runningClock / 60) , floor(runningClock % 60))
        end
    end
end`
