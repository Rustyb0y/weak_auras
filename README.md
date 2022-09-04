# Running Clock Example

```lua
function(unit, unitOwner)

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
end
```

# Function - Shorten Number

```lua
function ShortenNumber(number, significant)
    if type(number) ~= "number" then
        number = tonumber(number)
    end
    if not number then
        return
    end
    
    if type(significant) ~= "number" then
        significant = tonumber(significant)
    end
    significant = significant or 3
    
    local affixes = {
        "k",
        "m",
        "b",
        "t",
    }
    affixes[0] = ""
    
    local log, floor, max, abs = math.log, math.floor, math.max, math.abs
    
    local powerTen = floor(log(abs(number)) / log(10)) --get the log base 10
    powerTen = powerTen < 0 and 0 or powerTen --catch negative powers for numbers with an absolute value below 1
    local affix = floor(powerTen / 3) --every third power of ten (so thousands) results in a new affix
    local divNum = number / 1000^affix --get the "new" number by division with the floored amounts
    local before = powerTen%3 + 1 --determine how many digits before the .
    local after = max(0, significant - before) --and how many digits after
    
    return string.format(string.format("%%.%df%s", after, affixes[affix]), divNum)
end
```

# Function - readableNumber

```lua
local function readableNumber(num, places)
    local ret
    local placeValue = ("%%.%df"):format(places or 0)
    if not num then
        return 0
    elseif num >= 1000000000000 then
        ret = placeValue:format(num / 1000000000000) .. " Tril" -- trillion
    elseif num >= 1000000000 then
        ret = placeValue:format(num / 1000000000) .. " Bil" -- billion
    elseif num >= 1000000 then
        ret = placeValue:format(num / 1000000) .. " Mil" -- million
    elseif num >= 1000 then
        ret = placeValue:format(num / 1000) .. "k" -- thousand
    else
        ret = num -- hundreds
    end
    return ret
end```

# Format Large Numbers

```lua
aura_env.formatLargeNumber = function(number)
    if number<1000 then return number end
    if number<10000 then return string.sub(number,1,1).."."..string.sub(number,2,2).."k"; end
    if number<100000 then return string.sub(number,1,2).."."..string.sub(number,3,3).."k"; end
    if number<1000000 then return string.sub(number,1,3).."k"; end
    if number<10000000 then return string.sub(number,1,1).."."..string.sub(number,2,3).."m"; end
    if number<100000000 then return string.sub(number,1,2).."."..string.sub(number,3,4).."m"; end
    return string.sub(number,1,3).."."..string.sub(number,4,5).."m";
end
```