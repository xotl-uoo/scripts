#melee training scripts

The following skills can be trained by running these scripts.

| Primary Skills      |
| ----------- |
| `Arms Lore`     |
| `Fencing`       |
| `Healing`       |
| `Mace Fighting` |
| `Swordsmanship` |
| `Tactics`       |


**before you start**
> Melee skills can be raised on the training targets in Shelter Island to 70.

> Outlands offers wooden *training weapons* you can purchase at any Blacksmith.

## training dummy script
The training dummy method is the most popular, since most people can log in another account to act as the "dummy". If you are not in same guild, then this will flag you grey. Make sure, if you are turning grey you do this in your house.

<details>
<summary>UOSteam Macro</summary>
<p>
```
// UOSteam Weapon Training Script
// courtesy : Xotl
// https://xotl-uoo.github.io/scripts/
//
// Steps:
// ======
// 1. Place as many bandages as you can carry in backpack
// 2. Purchase 7-10 *wooden training weapons* for your skill
// 3. Remove all other weapon types from your pack
// 4. Stand next to your "dummy"
// 5. Run script
// 6. Toggle training dummy out of war mode
//

@removelist 'weps'
@createlist 'weps'
pushlist 'weps' 0x13b9 // wooden sword
pushlist 'weps' 0x13b4 // wooden club
pushlist 'weps' 0x1401 // wodden kryss
if not findalias 'dummy'
    sysmsg "Select dummy for training" 88
    promptalias 'dummy'
endif
while not dead
    @canceltarget
    if not @findlayer 'self' 1
        for 0 to 'weps'
            if @findtype weps[] 'any' 'backpack'
                @equipitem 'found' 1
                pause 1000
                break
            endif
        endfor
    endif
    if not @inrange 'dummy' 1
        headmsg "Where is my training dummy?" 44            
        stop
    endif
    attack! 'dummy'
    if not findtype 0xe21 'any' 'backpack'
        headmsg "I have no bandages!" 44            
        warmode 'on'
        warmode 'off'
        stop
    endif
    useobject 'found'
    waitfortarget 5000
    target! 'dummy'
    pause 20000
endwhile
```

</p>
<i>remember to use copy to clipboard icon in upper right of code window</i>
<p>last tested : 12/30/2020</p>
</details>  

<details>
<summary>CUO Razor Script</summary>
<p>
```
// CUO-Razor Weapon Training
// courtesy : Xotl
// https://xotl-uoo.github.io/scripts/
//
// Steps:
// ======
// 1. Fill your pack with bandages
// 2. Set a variable named 'dummy' to your target
// 2. Purchase 7-10 *wooden training weapons* for your skill
// 3. Remove all other weapon types from your pack
// 4. Stand next to your "dummy"
// 5. Run script
// 6. Toggle training dummy out of war mode
//
@removelist 'weps'
@createlist 'weps'
pushlist 'weps' 0x13b9
pushlist 'weps' 0x13b4
pushlist 'weps' 0x1401
while hits
    if lhandempty
        foreach x in 'weps'
            if findtype x
                dclicktype x
            endif
        endfor
    endif
    attack 'dummy'
    if not findtype 0xe21
        overhead "I have no bandages!" 44            
        hotkey 'Toggle War/Peace'
        hotkey 'Toggle War/Peace'
        stop
    endif
    dclicktype 0xe21
    wft
    target 'dummy'
    pause 20000
endwhile
```
</p>
<i>remember to use copy to clipboard icon in upper right of code window</i>
<p>last tested : 12/30/2020</p>
</details>  

## advanced weapon trainer

This is a version of the script for folks who may not have a secure place to train in, have not yet found a guild, or prefer not to run multiple accounts. The drawback with this method is you must get at least `50 Veterinary` and buy a pack horse. A lot of characters have 50 spare points in their builds, making this a viable option.

In order to train healing, you will need a *magical wizards hat* in your pack. This is a trick to lower your own health bar temporarily so you can bandage. Most folks want to train healing alongside melee, so I have included it in this version.

We can take advantage of the UO Steam `diffhits` capability to seldom heal the pony, while concentrating heals on your character. You will see this code below, where it prioritizes raising your healing skill and only helping the pony when its required. You can `diffhits` a pet, you cannot use it on another character, making it optimal for this use case.

To put the proverbial cherry on top of this script, it uses your bank to pull bandages from a container. *You can literally run this for days.*

<details>
<summary>UOSteam Macro</summary>
<p>
```
// UOSteam Weapon Training Script
// courtesy : Xotl
// https://xotl-uoo.github.io/scripts/
//
// Steps:
// ======
// 1. Place 5000+ bandages in a bag, in your bank.
// 2. Buy a magical wizards hat
// 3. Purchase 7-10 *wooden training weapons* for your skill
// 4. Remove all other weapon types from your pack
// 5. Stand next to pony/horse, next to bank
// 6. Run script
//
@removelist 'weps'
@createlist 'weps'
pushlist 'weps' 0x13b9 // wooden sword
pushlist 'weps' 0x13b4 // wooden club
pushlist 'weps' 0x1401 // wodden kryss
pause 1000
msg "bank"
pause 1500
if not findalias 'bankbag'
  sysmsg "Select bag in bank with bandages" 88
  promptalias 'bankbag'
endif
if not findalias 'pony'
  sysmsg "Select pony for training" 88
  promptalias 'pony'
endif
while not dead
  @canceltarget
  @findlayer 'self' 6
  @useobject 'found'
  pause 1000
  if not @findlayer 'self' 1
    for 0 to 'weps'
      if @findtype weps[] 'any' 'backpack'
        @equipitem 'found' 1
        pause 1000
        break
      endif
    endfor
  endif
  if not @inrange 'pony' 1
    headmsg "Where is my training pony?" 44
    stop
  endif
  attack! 'pony'
  if counttype 0xe21 'any' 'backpack' < 10
       sysmsg "restocking bandages" 73
       movetype 0xe21 'bankbag' 'backpack' 0 0 0 'any' 100
       pause 1500
  endif
  if not @findtype 0xe21 'any' 'backpack'
    headmsg "I have no bandages!" 44
    warmode 'on'
    warmode 'off'
    stop
  endif
  // pony
  if diffhits 'pony' > 25
    sysmsg "healing pony" 73
    @removetimer 'healing'
    @createtimer 'healing'
    @canceltarget
    @clearjournal
    usetype 0xe21 'any' 'backpack'
    waitfortarget 5000
    target! 'pony'
    while timer 'healing' < 20000
      if @injournal 'heal' 'system'
        break
      endif
      if @injournal 'finish' 'system'
        break
      endif
    endwhile
  endif
  // you
  @findtype 0x1718 'any' 'backpack'
  @useobject 'found'
  pause 1500
  @useobject 'found'
  pause 1500
  @removetimer 'healing'
  @createtimer 'healing'
  @canceltarget
  @clearjournal
  sysmsg "healing me" 73
  @usetype 0xe21 'any' 'backpack'
  waitfortarget 5000
  target! 'pony'
  while timer 'healing' < 20000
    if @injournal 'heal' 'system'
      break
    endif
    if @injournal 'finish' 'system'
      break
    endif
  endwhile
endwhile
```
</p>
<i>remember to use copy to clipboard icon in upper right of code window</i>
<p>last tested : 12/30/2020</p>
</details>

***

I love feedback, tips, tricks, ideas - just PM me at Xotl (Outlands Discord). Thanks for the support!
