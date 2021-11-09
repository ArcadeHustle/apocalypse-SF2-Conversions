15 Mar 2017
Conversion SF2' to Forgotten Worlds (CPS1)

I was about to post a repair log this week but I've been asked many times to release my Forgotten Worlds conversion with hacked controls.
So here it is!


1) Presentation

A conversion I made ~15 years ago cause I couldn't find the approriate spinners to play the game.
So I hacked the controls to use buttons instead of spinners (similar to the Megadrive version):
- button 1 is fire just as usual
- button 2 is used to rotate anti-clockwise
- button 3 is used to rotate clockwise
The hack supports 2 player game.
As usual a link to the files is available at the end of this post.

2) Technical info

Forgotten Worlds use a LWIO PAL instead of the classical IOB1 in order to be able to send requests (reset) and read from the NEC µPD4701AC used to handle spinners.
Related addresses are :
- 0x800040 to request a counter reset of spinner 1
- 0x800048 to request a counter reset of spinner 2
- 0x800052 to read counter (position) of spinner 1
- 0x80005a to read counter (position) of spinner 2
What I did is I searched within the program ROMs for occurences of "800052". There is only one @ 0x000660.
From that I've deducted :
- player 1 buttons status is stored in RAM @ 0xFFFFB26E
- player 2 buttons status is stored in RAM @ 0xFFFFB270
- player 1 angle is stored in RAM @ 0xFFFFB36A
- player 2 angle is stored in RAM @ 0xFFFFB3BA
- clockwise rotation increases the counter (anti-clockwise rotation decreases the counter indeed)
- a full round is 0x800
- if you go above 0xFFFF counter goes back to 0x0000
- if you go under 0x0000 counter goes to 0xFFFF

3) The hack

After having merged and concatenated the original ROMs to be used on a SF2' board I modified the program ROM 23 in order to:
- be able ro read buttons 2 & 3 for each player (which aren't used originally by the game)
- decrease counter if button 2 is pressed
- increase counter if button 3 is pressed
I've reused the lines used to read the spinners, there was enought space for my hack.

4) Material needed

4.1) If you use a 91634B-2 B-board (EPROM)
- 10 * 27C4096 ROM (8 for the graphics and 2 for the program)
- 2 * 27C010 ROM (audio)
- 1 * 27C512 ROM (audio)
- 1 * GAL16V8 (PAL)

4.2) If you use a 91635B-2 B-board (mask ROM)
- 8 * 27C400 ROM (graphics)
- 2 * 27C4096 ROM (program)
- 2 * 27C010 ROM (audio)
- 1 * 27C512 ROM (audio)
- 1 * GAL16V8 (PAL)

5) ROMs and PAL burning

Now it's time to burn the files on the appropriated devices.

5.1) If you use a 91634B-2 B-board (EPROM)
- ROMs 01/02/03/04/05/06/07/08/22/23 => 27C4096
- ROM 09 => 27C512
- ROM 18/19 => 27C010
- FW_PAL_1A.jed => GAL16V8

5.2) If you use a 91635B-2 B-board (mask ROM)
- ROMs 01/02/03/04/05/06/07/08 => 27C400
- ROMs 22/23 => 27C4096
- ROM 09 => 27C512
- ROM 18/19 => 27C010
- FW_PAL_1A.jed => GAL16V8

6) ROMs installation

All SF2' ROMs must be removed from the B-board.
The PAL named S963B at position 1A has to be removed too.
Double check you've put the devices the right way (the silkscreen should help you)!

6.1) If you use a 91634B-2 B-board (EPROM)
- Install the ROMs in the corresponding socket (ROM 01 in socket 01, etc.)
- Install the GAL16V8 in position 1A (where the S963B was)

6.2) If you use a 91635B-2 B-board (mask ROM)
- Install the ROMs 01/04/05/08/09/18/19/22/23 in the corresponding socket (ROM 01 in socket 01, etc.)
- Install ROM 02 in socket 03
- Install ROM 03 in socket 02
- Install ROM 06 in socket 07
- Install ROM 07 in socket 06
- Install the GAL16V8 in position 1A (where the S963B was)

[EDIT 31/03/17]
I just realised I put the wrong PAL file in the archive. That's fixed.

