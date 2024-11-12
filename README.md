# Keep-engine-running
This will keep the engine running
Put this code into qb-smallresources/client/hudcomponents.lua

function engineLoop()
    local ped = PlayerPedId()
    if DoesEntityExist(ped) and IsPedInAnyVehicle(ped, false) and IsControlPressed(2, 75) and not IsEntityDead(ped) and not IsPauseMenuActive() then
        local engineWasRunning = GetIsVehicleEngineRunning(GetVehiclePedIsIn(ped, true))
        Wait(1000)
        if DoesEntityExist(ped) and not IsPedInAnyVehicle(ped, false) and not IsEntityDead(ped) and not IsPauseMenuActive() then
            local veh = GetVehiclePedIsIn(ped, true)
            if engineWasRunning then
                SetVehicleEngineOn(veh, true, true, true)
            end
        end
    end
end

CreateThread(function()
    while true do

        -- Hud Components

        for i = 1, #disableHudComponents do
            HideHudComponentThisFrame(disableHudComponents[i])
        end

        for i = 1, #disableControls do
            DisableControlAction(2, disableControls[i], true)
        end
        engineLoop()
        
        DisplayAmmoThisFrame(displayAmmo)
        -- Density
        SetParkedVehicleDensityMultiplierThisFrame(Config.Density['parked'])
        SetVehicleDensityMultiplierThisFrame(Config.Density['vehicle'])
        SetRandomVehicleDensityMultiplierThisFrame(Config.Density['multiplier'])
        SetPedDensityMultiplierThisFrame(Config.Density['peds'])
        SetScenarioPedDensityMultiplierThisFrame(Config.Density['scenario'], Config.Density['scenario']) -- Walking NPC Density
        Wait(0)
    end
end) 
