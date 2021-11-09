```
1 Mar 2017
Conversion SF2' to Ghouls'n'Ghosts (World, not resale) CPS1
1) Presentation

On CPS1 it's possible to convert any game to run with any B-board and with any C-board.

To use a different C-board:
- program ROMs have to be modified according to the tables of the C-board used

To use a different B-board:
- the graphic PAL has to be modified to handle graohic ROMs addressing
- ROMs have to be splitted/merged depending of the initial B-board and the conversion B-board

The conversion of SF2' to Daimakaimura is well know online, and there's a reason for it, it requires only ROM and PAL swap.
But I'm not happy with this conversion, as everyone outside of Japan I grew up on Ghouls'n'Ghosts not Daimakaimura (and I love the bat style title screen).
The conversion I propose is to convert a SF2' game using a 91634B-2 (EPROM) or 91635B-2 (mask ROM) B-board and a C-board with CPS-B-21 chip to the World (Export) version of Ghouls'n'Ghosts originally using a 88620B B-board with embedded CPS-B-01 chip (no C-board on this early CPS1 game).
The link to the files is available at the end of this post.

2) Material needed

2.1) If you use a 91634B-2 B-board (EPROM)
- 10 * 27C4096 ROM (8 for the graphics and 2 for the program)
- 1 * 27C010 ROM (audio)
- 1 * GAL16V8 (PAL)

2.2) If you use a 91635B-2 B-board (mask ROM)
- 8 * 27C400 ROM (graphics)
- 2 * 27C4096 ROM (program)
- 1 * 27C010 ROM (audio)
- 1 * GAL16V8 (PAL)

3) Technical explanation

3.1) C-board conversion
For the C-board there isn't any problem as the CPS-B-21 chip is a CPS-B-01 with volatile registers. If no suicide battery is used it's identical to the CPS-B-01 therefore no program modification is needed.

3.2) B-board conversion

3.2.1) Graphic PAL
For the B-board the job has already been done for the PAL: we can use the DAM63B file used for Daimakaimura to replace the S963B PAL present on the SF2' B-board.

3.2.2) ROM merging
Some files have to be merged other can be used as is.

Ghousl'b'Ghost (World) romset contains the following files:
09.4a
10.4b
11.4c
12.4d
13.4e
14.4f
15.4g
16.4h
18.7a
19.7b
20.7c
21.7d
22.7e
23.7f
24.7g
25.7h
26.10a
dm-05.3a
dm-06.3c
dm-07.3f
dm-08.3g
dm-17.7j
dme_27.9h
dme_28.9j
dme_29.10h
dme_30.10j

The goal of the conversion is to obtain graphic ROMs at positions 01 to 08, audio ROM at position 09 and program ROMs at positions 22 & 23.

dm-05.3a corresponds to ROM at position 01.
dm-06.3c corresponds to ROM at position 02.
dm-07.3f corresponds to ROM at position 03.
dm-08.3g corresponds to ROM at position 04.

It's a bit more complicated for ROM at position 05: early CPS1 games use four 128kb * 8 bit ROMs instad of big 256kb * 16 bit ROMs.
Four files have to be merged: 09.4a (LSB LOW), 18.7a (MSB LOW), 10.4b (LSB HIGH) and 19.7b (MSB HIGH). The file we obtain is only 256kb so we can leave the other half of the ROM blanked (filled with $FF just like in the Daimakaimura resale version) or we can double the file which is what I did.

11.4c (LSB LOW), 20.7c (MSB LOW), 12.4d (LSB HIGH) and 21.7d (MSB HIGH) correspond to ROM at position 06.
13.4e (LSB LOW), 22.7e (MSB LOW), 14.4f (LSB HIGH) and 23.7f (MSB HIGH) correspond to ROM at position 07.
15.4g (LSB LOW), 24.7g (MSB LOW), 16.4h (LSB HIGH) and 25.7d (MSB HIGH) correspond to ROM at position 08.
dam-09.12a corresponds to ROM at position 09 and have to be doubled.
dm-17.7j corresponds to ROM at position 22. Warning a byteswap is needed.
dme_30.10j (LSB LOW), dme_29.10h (MSB LOW), dme_28.9j (LSB HIGH) and dme_27.9h (MSB HIGH) correspond to ROM at position 23.

4) ROMs and PAL burning

Now it's time to burn the files on the appropriated devices.

4.1) If you use a 91634B-2 B-board (EPROM)
- ROMs 01/02/03/04/05/06/07/08/22/23 => 27C4096
- ROM 09 => 27C010
- GNG_PAL_1A.jed => GAL16V8

4.2) If you use a 91635B-2 B-board (mask ROM)
- ROMs 01/02/03/04/05/06/07/08 => 27C400
- ROMs 22/23 => 27C4096
- ROM 09 => 27C010
- GNG_PAL_1A.jed => GAL16V8

5) ROMs installation

All SF2' ROMs must be removed from the B-board.
The PAL named S963B at position 1A has to be removed too.
Double check you've put the devices the right way (the silkscreen should help you)!

5.1) If you use a 91634B-2 B-board (EPROM)
- Install the ROMs in the corresponding socket (ROM 01 in socket 01, etc.)
- Install the GAL16V8 in position 1A (where the S963B was)

5.2) If you use a 91635B-2 B-board (mask ROM)
- Install the ROMs 01/04/05/08/09/22/23 in the corresponding socket (ROM 01 in socket 01, etc.)
- Install ROM 02 in socket 03
- Install ROM 03 in socket 02
- Install ROM 06 in socket 07
- Install ROM 07 in socket 06
- Install the GAL16V8 in position 1A (where the S963B was)

```
