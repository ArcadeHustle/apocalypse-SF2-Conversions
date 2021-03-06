```
26 Jul 2017
Conversion SF2' to Carrier Air Wing (CPS1)
Originally the World version of Carrier Air Wing runs on a 89624B-3 B-board and 88622-C-5 C-board.

To use a different B-board, files need to be rearranged (merged, interleaved, ect.) and one of the B-board PALs (CA24B for Carrier Air Wing) must be adapated (handcrafted). This will not be covered by this article as I want to keep it at a reasonnable size (and prevent readers to fall asleep).

To use a different C-board, program ROMs must be modified to use different registers addresses and different layer masks.
I will try to give some useful information on how to proceed.

1) Modifying MAME source and recompiling

Carrier Air Wing originally uses a CPS-B-16 chip on the C-board while SF2' uses a CPS-B-21 chip with no battery.
Everything is detailed in MAME:
Carrier Air Wing (World 901009)                              1990  89624B-3   CA24B            IOB1  88622-C-5    CPS-B-16  DL-0411-10011  None
Street Fighter II': Champion Edition (World 920313)          1992  91635B-2   S9263B   BPRG1   IOB1  92631C-6     CPS-B-21  DL-0921-10014  C632    IOC1
In order to test my work in MAME I've modified the file cps1.cpp located in mame\src\mame\video
I've edited the following line:
{"cawing",      CPS_B_16,     mapper_CA24B },
To:
{"cawing",      CPS_B_21_DEF, mapper_CA24B },
This will force MAME to use the CPS-B-21 chip (no battery hence CPS_B_21_DEF in the line above) instead of CPS-B-16.

To recompile MAME you can follow the instructions on MAME's official website:
http://mamedev.org/tools/

2) Editing registers addresses

Again the needed information can be found in the cps1.cpp file mentionned earlier:
/*                     CPSB ID    multiply protection      unknown      ctrl     priority masks   palctrl    layer enable masks  */
#define CPS_B_16     0x00,0x0406,          __not_applicable__,          0x0c,{0x0a,0x08,0x06,0x04},0x02, {0x10,0x0a,0x0a,0x00,0x00}
#define CPS_B_21_DEF 0x32,  -1,   0x00,0x02,0x04,0x06, 0x08, -1,  -1,   0x26,{0x28,0x2a,0x2c,0x2e},0x30, {0x02,0x04,0x08,0x30,0x30}
The base address for registers is always 0x800140 for all CPS1 games.

So we can calculate the different adresses for a CPS-B16 chip (Carrier Air Wing):
Layer control = 0x800140 + 0x0C = 0x80014C
Priority mask 1 = 0X800140 + 0x0a = 0x80014A
Priority mask 2 = 0X800140 + 0x08 = 0x800148
Priority mask 3 = 0X800140 + 0x06 = 0x800146
Priority mask 4 = 0X800140 + 0x04 = 0x800144
Palette control = 0X800140 + 0x02 = 0x800142

And the same for a CPS-B-21 chip with no battery (SF2'):
Layer control = 0x800140 + 0x26 = 0x800166
Priority mask 1 = 0X800140 + 0x28 = 0x800168
Priority mask 2 = 0X800140 + 0x2A = 0x80016A
Priority mask 3 = 0X800140 + 0x2C = 0x80016C
Priority mask 4 = 0X800140 + 0x2E = 0x80016E
Palette control = 0X800140 + 0x30 = 0x800170

Now in the program ROMs of Carrier Air Wing we will replace address 0x80014A by 0x800166 (layer control). And so on for the priority masks and palette control.

But that's not all, base register 0x800140 is used to read the CPS-B chip ID, which is 0x0406 for a CPS-B-16 and -1 (0xFFFF) for a CPS-B-21.
Carrier Air Wing does read it several times and in different ways (sometimes as a word from 0x800140, sometimes as a byte from 0x800141 etc.).
So each time the game tries to read the ID we will have to make it to believe the chips is a CPS-B-16.
For instance
move.w  $800140.l, D0
will have to be replaced by
move.w #$406, D0 (followed by a NOP instruction as it needs only 4 bytes instead of 6).
For a byte
move.b  $800140.l, D0
can be replaced by
move.b #$4, D0 (first byte of 0x0406)
and
move.b  $800141.l, D0
can be replaced by
move.b #$6, D0 (second byte of 0x0406).

Once done game should boot in MAME but with the wrong layers enabled (as expected).

3) Editing layer masks

An other particularity of CPS-B chips is they used different values to enable layers.
Back to the MAME's source:
/*                     CPSB ID    multiply protection      unknown      ctrl     priority masks   palctrl    layer enable masks  */
#define CPS_B_16     0x00,0x0406,          __not_applicable__,          0x0c,{0x0a,0x08,0x06,0x04},0x02, {0x10,0x0a,0x0a,0x00,0x00}
#define CPS_B_21_DEF 0x32,  -1,   0x00,0x02,0x04,0x06, 0x08, -1,  -1,   0x26,{0x28,0x2a,0x2c,0x2e},0x30, {0x02,0x04,0x08,0x30,0x30}
So for a CPS-B-16 offsets are:
Layer 1 enable = 0x10
Layer 2 enable = 0x0A
Layer 3 enable = 0x0A
Layer 4 enable = 0x00
Layer 5 enable = 0x00
When 2 layers share the same offset it means they are simulteneously activated.
Here we can see layers 2 and 3 are activated together, so as layers 4 and 5.

For a CPS-B-21 offsets are:
Layer 1 enable = 0x02
Layer 2 enable = 0x04
Layer 3 enable = 0x08
Layer 4 enable = 0x30
Layer 5 enable = 0x30

To enable/disable layers, game must write to the layer control register. As seen before this is address 0x80014C for a CPS-B-16 and 0x800166 for a CPS-B-21.
The value must contain a base offset of 0x12C0 (identical for all CPS1 games) then add the desired values.
Example:
to activate only layer 1 with a CPS-B-21 chip, game must write 0x12C0 + 0x02 = 0x12C2 to the register 0x800166.

Using MAME dissasembler with the modified program ROMs I've found Carrier Air Wing accesses the layer control register either directly:
000482: 33FC 12D0 0080 0166        move.w  #$12d0, $800166.l
Or indirectly:
001CDE: 33ED 004A 0080 0166        move.w  ($4a,A5), $800166.l

For the direct access it's quite simple: 0x12D0 for a CPS-B-16 chip corresponds to 0x12FE for a CPS-B-21 chip, so the line
000482: 33FC 12D0 0080 0166        move.w  #$12d0, $800166.l
Is replaced by
000482: 33FC 12FE 0080 0166        move.w  #$12FE, $800166.l

For the indirect access I had to set breakpoints and found the mask value is written to RAM @ 0xFF804A before being written to the layer control register.
So I searched for all the writes to 0xFF804A and modified the values being written according to the different offsets.
Example:
000606: 3B7C 12DA 004A             move.w  #$12da, ($4a,A5)
is replaced by
000606: 3B7C 12FE 004A             move.w  #$12FE, ($4a,A5)

Here are the different values I've found being used and their equivalent for the CPS-B-21 chip:
CPS-B-16 CPS-B-21
0x12DA  0x12FE
0x139A  0x13BE
0x06DA  0x06FE
0x24DA  0x24FE
0x18DA  0x18FE
0x079A  0x07BE

Then I ran the game but found 2 problems:
- incorrect layers enabled during attract
- missing Game Over text (again incorrect layers enabled)

By watching the RAM I could see the wrong value written to RAM @ 0xFF804A during attract was 0x12DA so I set a conditionnal breakpoint in the MAME debugger:
wpset FF804A,1,w,wpdata==12DA
This informed me the game sometimes uses the layer control offset as a mask:
000F3C: 006D 001A 004A             ori.w   #$1a, ($4a,A5)
Here the value used is 0x1A (all layers enabled for a CPS-B-16) which corresponds the 0x3E for a CPS-B-21 chip.
I replaced the line as follow
ori.w   #$3E, ($4a,A5)
and now attract was correct.
I followed the same procedure for the missing Game Over text (value 0x12D0 written).
This time value 0x10 was used (layers 1, 4 and 5):
001234: 006D 0010 004A             ori.w   #$10, ($4a,A5)
Which I replaced by
ori.w   #$32, ($4a,A5)
And game was finally working perfectly!

4) Material needed

4.1) If you use a 91634B-2 B-board (EPROM)
 - 6 * 27C4096 ROM (4 for the graphics and 2 for the program)
 - 2 * 27C010 ROM (audio)
 - 1 * 27C512 ROM (audio)
 - 1 * GAL16V8 (PAL)

4.2) If you use a 91635B-2 B-board (mask ROM)
 - 4 * 27C400 ROM (graphics)
 - 2 * 27C4096 ROM (program)
 - 2 * 27C010 ROM (audio)
 - 1 * 27C512 ROM (audio)
 - 1 * GAL16V8 (PAL)

5) ROMs and PAL burning

Now it's time to burn the files on the appropriated devices.

5.1) If you use a 91634B-2 B-board (EPROM)
 - ROMs 01/02/03/04/22/23 => 27C4096
 - ROM 09 => 27C512
 - ROM 18/19 => 27C010
 - CAW_PAL_1A => GAL16V8

5.2) If you use a 91635B-2 B-board (mask ROM)
 - ROMs 01/02/03/04 => 27C400
 - ROMs 22/23 => 27C4096
 - ROM 09 => 27C512
 - ROM 18/19 => 27C010
 - CAW_PAL_1A => GAL16V8

6) ROMs installation

All SF2' ROMs must be removed from the B-board.
The PAL named S963B at position 1A has to be removed too.
Double check you've put the devices the right way (the silkscreen should help you)!

6.1) If you use a 91634B-2 B-board (EPROM)
 - Install the ROMs in the corresponding socket (ROM 01 in socket 01, etc.)
 - Install the GAL16V8 in position 1A (where the S963B was)

6.2) If you use a 91635B-2 B-board (mask ROM)
 - Install the ROMs 01/04/18/19/22/23 in the corresponding socket (ROM 01 in socket 01, etc.)
 - Install ROM 02 in socket 03
 - Install ROM 03 in socket 02
 - Install the GAL16V8 in position 1A (where the S963B was)
```
