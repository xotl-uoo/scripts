#wrestling training scripts

The following skills can be trained by running these scripts.

| Primary Skills      |
| ----------- |
| `Wrestling`     |


**before you start**
> Melee skills can be raised on the training targets in Shelter Island to 70.

## training dummy

Raising wrestling requires actually hitting something. In this case, it can either be another character or a pack horse. Make sure to lock `Tactics`, `Anatomy`, `Arms Lore` all to 0.

There are two ways to run this script:

| Option | Notes |
| ----------- | ----------- |
| Packhorse | Buy 50 Veterinary and lock it |
| Character | Buy 50 Healing and lock it |

<details>
<summary>UOSteam Macro</summary>
<p>
```
// UOSteam Wrestling Training
// courtesy : Xotl
// https://xotl-uoo.github.io/scripts/
//
// Steps:
// ======
// 1. Place as many bandages as you can carry in backpack
// 2. Buy 50 Healing or 50 Vet depending on your target
// 3. Stand next to your "dummy"
// 4. Run script
// 5. Toggle training dummy out of war mode
//

if not findalias 'dummy'
    sysmsg "Select dummy for training" 88
    promptalias 'dummy'
endif
while not dead
    @canceltarget
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
    pause 60000
endwhile
```

</p>
<i>remember to use copy to clipboard icon in upper right of code window</i>
</details>  

<details>
<summary>CUO Razor Script</summary>
<p>
```
// CUO-Razor Wrestling Training
// courtesy : Xotl
// https://xotl-uoo.github.io/scripts/
//
// Steps:
// ======
// 1. Fill your pack with bandages
// 2. Set a variable named 'dummy'
// 3. Stand next to your "dummy"
// 4. Run script
// 5. Toggle training dummy out of war mode
//
setvar 'dummy'
while hits > 0
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
    pause 60000
endwhile
```
</p>
<i>remember to use copy to clipboard icon in upper right of code window</i>
</details>  

***

I love feedback, tips, tricks, ideas - just PM me at Xotl (Outlands Discord). Thanks for the support!
