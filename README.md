# qb-flashbang
FlashBang weapon for QBCore

## Dependencies
- [qb-core](https://github.com/qbcore-framework/qb-core)
- [qb-weapons](https://github.com/qbcore-framework/qb-weapons)

## Items (qb-core/shared/item.lua)
```
weapon_flashbang          = { name = 'weapon_flashbang', label = 'Flash Bang', weight = 1000, type = 'weapon', ammotype = nil, image = 'weapon_flashbang.png', unique = true, useable = false, description = 'Blinds and disorients targets briefly' },
```

## Weapons (qb-core/shared/weapons.lua)
```
[`weapon_flashbang`]          = { name = 'weapon_flashbang', label = 'Flash Bang', weapontype = 'Throwable', ammotype = nil, damagereason = 'Flash Bang' },
```

## Config (qb-weapons/config.lua)
```
Config.Throwables = {
    'flashbang'  -- Add FlashBang Here 
}

Config.DurabilityMultiplier = {
    weapon_flashbang             = 0.15,  -- Add FlashBang Here 
}
```

## Config (qb-weapons/client/main.lua)
```
-- or weaponName == 'weapon_flashbang'  -- Add This FlashBang Here In ( elseif weaponName == 'weapon_stickybomb' or weaponName == 'weapon_flashbang' )

RegisterNetEvent('qb-weapons:client:UseWeapon', function(weaponData, shootbool)
    local ped = PlayerPedId()
    local weaponName = tostring(weaponData.name)
    local weaponHash = joaat(weaponData.name)
    if currentWeapon == weaponName then
        TriggerEvent('qb-weapons:client:DrawWeapon', nil)
        SetCurrentPedWeapon(ped, `WEAPON_UNARMED`, true)
        RemoveAllPedWeapons(ped, true)
        TriggerEvent('qb-weapons:client:SetCurrentWeapon', nil, shootbool)
        currentWeapon = nil
    elseif weaponName == 'weapon_stickybomb' or weaponName == 'weapon_flashbang' or weaponName == 'weapon_pipebomb' or weaponName == 'weapon_smokegrenade' or weaponName == 'weapon_flare' or weaponName == 'weapon_proxmine' or weaponName == 'weapon_ball' or weaponName == 'weapon_molotov' or weaponName == 'weapon_grenade' or weaponName == 'weapon_bzgas' then
        TriggerEvent('qb-weapons:client:DrawWeapon', weaponName)
        GiveWeaponToPed(ped, weaponHash, 1, false, false)
        SetPedAmmo(ped, weaponHash, 1)
        SetCurrentPedWeapon(ped, weaponHash, true)
        TriggerEvent('qb-weapons:client:SetCurrentWeapon', weaponData, shootbool)
        currentWeapon = weaponName
    elseif weaponName == 'weapon_snowball' then
        TriggerEvent('qb-weapons:client:DrawWeapon', weaponName)
        GiveWeaponToPed(ped, weaponHash, 10, false, false)
        SetPedAmmo(ped, weaponHash, 10)
        SetCurrentPedWeapon(ped, weaponHash, true)
        TriggerServerEvent('qb-inventory:server:snowball', 'remove')
        TriggerEvent('qb-weapons:client:SetCurrentWeapon', weaponData, shootbool)
        currentWeapon = weaponName
    else
        TriggerEvent('qb-weapons:client:DrawWeapon', weaponName)
        TriggerEvent('qb-weapons:client:SetCurrentWeapon', weaponData, shootbool)
        local ammo = tonumber(weaponData.info.ammo) or 0

        if weaponName == 'weapon_petrolcan' or weaponName == 'weapon_fireextinguisher' then
            ammo = 4000
        end

        GiveWeaponToPed(ped, weaponHash, ammo, false, false)
        SetPedAmmo(ped, weaponHash, ammo)
        SetCurrentPedWeapon(ped, weaponHash, true)

        if weaponData.info.attachments then
            for _, attachment in pairs(weaponData.info.attachments) do
                GiveWeaponComponentToPed(ped, weaponHash, joaat(attachment.component))
            end
        end

        if weaponData.info.tint then
            SetPedWeaponTintIndex(ped, weaponHash, weaponData.info.tint)
        end

        currentWeapon = weaponName
    end
end)
```

## Preview's
![Preview Screenshot](https://cdn.discordapp.com/attachments/1076128858224467998/1374741394744279040/8b572a136ab37be493fdd0e636835e0093162ebd.jpeg?ex=682f2731&is=682dd5b1&hm=e9e7be3ae0a148263393a4d3e9fd143664ef90dd0940f25da8b753f6e5550503&)

## Discord
- [Join Discord](https://discord.gg/R6T)
