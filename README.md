# Low-Latency-Optimization-For-Noobs
This repo will help you optimize your PC for the lowest latency possible without reducing the functionality of windows (By much).

Download [LatencyMon](https://www.resplendence.com/latencymon) to test latency!


RUN AS CMD AS ADMIN AND READ INSTRUCTIONS OR ELSE THE OPTOMIZATION WON'T BE FULLY COMPLETED!!!!

Make a restore point if you do something wrong!

Forces the kernal to contstanly poll for interrupts instead of waiting for it (Increases Power Usage)

Run on CMD

`bcdedit /set disabledynamictick yes`

Turnes off hypervision which is not needed for gaming (hyper vision is used for running other operating systems on windows ex. linux on windows) See https://learn.microsoft.com/en-us/virtualization/hyper-v-on-windows/about/

Run on CMD

`bcdedit /set hypervisorlaunchtype off` 

Disable Idle CPU mode (C-states). This will reduce jitter as the cpu will be running at max preformence and not switching between c-states and different clock speeds. This makes your CPU run hotter so make sure to have good cooling

If you dont have good cooling skip this step!!!

Go to the power management options in control panel and set the plan to "High Performance" or what the best performence is available, go to advanced settings, go to "Processor power management" and set “Processor idle disable" to “Disable idle” under processor power options.

On windows 10 it will show that your cpu is running at 100%. This is a bug.

Press WIN + r to open run and type in "optionalfeatures" then disable any feature that you don't need. If you don't know what a feature does don't disable it.

Download this app [MSI_Util_V3](https://www.mediafire.com/file/ewpy1p0rr132thk/MSI_util_v3.zip/file) and run it as admin. 
There should be a section that is labeled supported modes if MSI, check the msi box. Set your GPU to high interrupt policy.

If your running an AMD CPU plug your mouse and keyboard into a usb port that goes directly to the cpu you will have to test this to figure out which port does go to the cpu and not the chipset. (The fastest port does not always go to the cpu)
You can figure this out by going to your mother board specifications and check which ports go to the cpu or the chipset.

Follow this video (https://www.youtube.com/watch?v=KWEPjoit1_E&t=4s)

Use WIN + r and then type regedit go to `[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Class\{4d36e968-e325-11ce-bfc1-08002be10318}\0000]` then right click the screen and create a new dword and name it `DisableDynamicPstate` Then set the value of the dword to `1`
This disables the energy saving mode on your gpu making it not switch between energy saving mode `P2` and performence mode `P1`. This will cause more electricity to be used and increase heat. I don't recommened if your GPU is already overheating.

Download [Process Lasso](https://bitsum.com)
Search up whatever app or game you wan't to optimize and right click it and set CPU priority to high, I/O priority to High, set application performence mode to bitsum high preformence and enable induce performance mode.
Then go to Options/Tools/System Timer Resolution and click "max" and "set" then check "Set at every boot" and then ok.

if the timer resolution is not .5 then run these commands in cmd and repeat the steps above.

`bcdedit /set useplatformtick yes`

`bcdedit /set disabledynamictick yes`

`bcdedit /deletevalue useplatformclock`

its ok if an error is spit out.

I copied this section from Calypto's Latency Guide since I coudn't explain this well

Process scheduling
“Quantum” is the amount of time the Windows process scheduler allocates to a thread. You may choose between short or long quantum. Furthermore, you can choose to boost the foreground quanta by double or triple, meaning the currently highlighted program gets two or three times longer quantum. For gaming, it makes most sense to use long quantum and three times foreground boost, since we want to maximize CPU time the game gets. The higher the boost, the less the game will be interrupted by background programs. When not gaming, the drawback to using longer quantum is that apparent responsiveness when using multiple programs may be reduced. In general, the longer the duration of quanta, the more we minimize context switching. Context switching is computationally expensive and should be minimized to reduce jitter from background processes/threads when gaming.

The table below lists the possible configurations you can tell the scheduler to use. You may select short or long quantum, fixed or variable; and if you select variable, how much boost (2x or 3x) to give the foreground program. What quantum you decide depends on your use case. The default quantum is dec. 38 for non-server Windows editions, while for server it is dec. 24. This can be checked in Advanced System Settings → Performance → Advanced. My personal recommendation is dec. 22. 

To change the quantum, open regedit and go to:
`[HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001\Control\PriorityControl\Win32PrioritySeparation]`

From the table, add together the decimal values you want and enter that as a decimal to the Win32PrioritySeparation key. You cannot use the third column unless you use variable quantum. If you are using fixed quantum, ignore the third column
Changes to Win32PrioritySeparation apply instantly; no restart required

Examples: Short, 3x = 32+4+2 = dec. 38   Long, 3x = 16+4+2 = dec. 22    Short, fixed = 32+8 = dec. 40

Possible values, in decimal:

20 = Long, variable, no foreground boost (12:12)

21 = Long, variable, 2x foreground boost (24:12)

22 = Long, variable, 3x foreground boost (36:12)

24 = Long, fixed (36:36)

36 = Short, variable, no foreground boost (6:6)

37 = Short, variable, 2x foreground boost (12:6)

38 = Short, variable, 3x foreground boost (18:6)

40 = Short, fixed (18:18)

I chose 22 because I want the lowest input delay.

CPU, GPU and RAM overclock
Overclocking your GPU, CPU and RAM is a good way to squeeze extra performance and reduce latency since the pc is running more cycles. Overclocking is different for everyone's pc but
there are a lot of guides for a specific cpu/gpu. (If your are running a non-k intel cpu then you can't overclock the cpu!)

Nvidia has a feature that can auto-overclock your gpu but to get the most performence you have to do it manually.

Close all applications when testing input delay for more consistant results.

My Before and After

Before:

Average measured interrupt to process latency (µs):   6.030195

Average measured interrupt to DPC latency (µs):       2.706149

After:

Average measured interrupt to process latency (µs):   3.403655

Average measured interrupt to DPC latency (µs):       1.673678

Lower numbers are better

My specs:

CPU: Ryzen 5 2600

Motherboard: Aorus B450 M

Ram: 8GB

GPU: GTX 1650 OC

My cpu is worse for input delay because of infinity fabric and ccx core complexes

Note that intel cpus will be lower because of different timings.  Different timers (TSC/HPET/PMT etc.) will give different results. The different brands don't really have a difference.


I measured this using the LatencyMon App (https://www.resplendence.com/latencymon)

Data is found on the Stats page.

Your results may vary.
