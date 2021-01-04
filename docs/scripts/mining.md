#Mining

The following skills can be trained by running these scripts.

| Primary Skills      |
| ----------- |
| `Mining`     |

Simple mining macros for UOSteam and CUO/Razor clients. The scripting interface between the two clients is very different, as are its capabilities. Please read the instructions carefully for each version.

##captcha

I do not attempt to detect or bypass the Outlands server captcha system. The way I approach this in script is to check if your gatherer is holding a tool - if they are not, the script assumes you have removed it manually to answer the captcha, and will resume in 15 seconds.

>When you see a captcha appear, manually drop the gatherer's tool to your pack. Answer captcha. You will have 15 seconds before it attempts to use it again.

###how to bind tool drop to key

My preferred method for dropping the tool from my miner's pack is to bind a key *in game* in cast some simple spell. This way, it will not interrupt the script while it is running - and just drop the pick out of the gatherer's hand. To do this:

1. Bind F12 to some simple spell (Strength, Agility, etc)

Now when the captcha appears, you wont have to fumble with dropping the pick from your miner's hand with a mouse drag - just hit the key. You will get a target prompt for the spell cast - just ignore that, it will timeout.

## Script Tips

For the UOSteam client script, you will see I use 'autotargetself' then use tool. This saves milliseconds in a round trip request to server over the - use tool, `waitfortarget`, `target self` approach but time is money. Keep this in mind if you make any script modifications. You will notice the difference, albeit barely.


<details>
<summary>UOSteam Macro</summary>
<p>
```
// UOSteam Mining
// courtesy : Xotl
// https://xotl-uoo.github.io/scripts/
//
// Steps:
// ======
// 1. Buy a pack animal
// 2. Place pickaxes in your pack
// 3. Recommended!! Bind F12 key in game to Strength.
// 4. Run script
// 5. When captcha appears, hit F12 to drop pick
//
sysmsg "UOSteam Mining" 73
sysmsg "When captcha appears, drop the pick" 66
sysmsg "   you will have 15 seconds to continue" 66
@useobject 'backpack'
pause 1000
if not @findalias 'packy'
  sysmsg "Select pack horse" 73
  promptalias 'packy'
endif
usetype 0xe86 'any' 'backpack'
pause 1500
while hits > 0
  if diffweight < 50
    sysmsg "I am too heavy, moving ore" 48
    headmsg "wait..." 48
    useobject 'backpack'
    pause 1500
    while @findtype 0x19b9 'any' 'backpack'
      @movetype 0x19b9 'backpack' 'packy'
      pause 1500
    endwhile
    if diffweight < 50
      headmsg "Unable to move ore" 48
      stop
    endif
  endif
  if not @findlayer 'self' 1
    if not @findtype 0xe86 'any' 'backpack'
      headmsg 'No pickaxes!' 48
      stop
    endif
    sysmsg "Either pick broke or a captcha appeared, pausing..." 48
    sysmsg "Continuing in 15 seconds" 48
    pause 15000
  endif
  @clearjournal
  autotargetself
  useobject 'found'
  removetimer 'mine'
  createtimer 'mine'
  while timer 'mine' < 2500
    if @injournal 'ore' 'system'
      break
    elseif @injournal 'loosen' 'system'
      break
    elseif @injournal 'harvest' 'system'
      headmsg 'Find a new spot' '48'
      break
    else
      pause 100
    endif
  endwhile
endwhile
```

</p>
<i>remember to use copy to clipboard icon in upper right of code window</i>
<p>last tested : 1/3/2021</p>
</details>  

<details>
<summary>CUO Razor Script</summary>
<p>
```

// CUO-Razor Mining
// courtesy : Xotl
// https://xotl-uoo.github.io/scripts/
//
// Steps:
// ======
// 1. Set a variable named 'packy' to your pack horse/llama
// 2. Place pickaxes in your pack
// 3. Modify -weight- checks in script
//    a. Open [Status] button
//    b. Find max stones number
//    c. Set to (max stones - 30)
//    d. Example, 390 stones max, set to 360
//    e. [Save] script modification
// 4. Recommended!! Bind F12 key in game to Strength.
// 5. Run script
// 6. When Captcha appears hit F12 to drop pick
//
sysmsg "CUO Mining" 73
sysmsg "When captcha appears, drag pick to pack" 66
sysmsg "   you will have 15 seconds to continue" 66
dclicktype 0xe86
pause 1500
while hits
    // !! modify
    if weight > 360
        sysmsg "I am too heavy, moving ore" 48
        overhead "wait..." 48
        while findtype 0x19b9 true
            lifttype 0x19b9 999
            drop 'packy' 0 0 0
            pause 1500
        endwhile
        // !! modify
        if weight > 360
            overhead "Unable to move ore" 48
            stop
        endif
    endif
    if not findtype 0xe86
        overhead "I have no pickaxes!" 48            
        stop
    endif
    if rhandempty
        sysmsg "Either pick broke or a captcha appeared, pausing..." 48
        overhead "Continuing in 15 seconds" 48
        dclicktype 0xe86
        pause 15000
    endif
    clearsysmsg
    hotkey "Use Item in Right Hand"
    wft 3000
    target 'self'
    removetimer 'mine'
    createtimer 'mine'
    while timer 'mine' < 2500
        if insysmsg 'ore'
            break
        elseif insysmsg 'loosen'
            break
        elseif insysmsg 'harvest'
            overhead "Find a new spot" 73
            break
        else
            pause 100
        endif
    endwhile
endwhile
```
</p>
<i>remember to use copy to clipboard icon in upper right of code window</i>
<p>last tested : 1/3/2021</p>
</details>  

Common Errors | Resolution |
| ----------- | ----------- |
| `Cannot convert argument to uint` | You didnt set your 'Packy' variable in CUO-Razor|

***
I love feedback, tips, tricks, ideas - just PM me at Xotl (Outlands Discord). Thanks for the support!
