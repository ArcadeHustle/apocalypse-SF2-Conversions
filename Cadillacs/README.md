```
4 Apr 2017
Conversion SF2' to Cadillacs & Dinosaurs (CPS1 no Q-sound)


I've been asked by a member of the Aussie Arcade forum if I could do a conversion of Cadillacs & Dinosaurs without Q-sound. It does exist a bootleg version named Dinosaur Hunter but the member pointed out that several things were modified and most annoying: the title screen was modified, you start on level 2, start button allows you to change your character in game, ammos are unlimited as long as you don't lose your weapon, etc.


What I did is I hacked the game to be able to play samples from the Chinese bootleg.


1) Material needed

1.1) If you use a 91634B-2 B-board (EPROM)
- 12 * 27C4096 ROM (8 for the graphics and 4 for the program)
- 2 * 27C010 ROM (audio)
- 1 * 27C512 ROM (audio)
- 1 * GAL16V8 (PAL)

1.2) If you use a 91635B-2 B-board (mask ROM)
- 8 * 27C400 ROM (graphics)
- 4 * 27C4096 ROM (program)
- 2 * 27C010 ROM (audio)
- 1 * 27C512 ROM (audio)
- 1 * GAL16V8 (PAL)

2) ROMs and PAL burning

Now it's time to burn the files on the appropriated devices.

2.1) If you use a 91634B-2 B-board (EPROM)
- ROMs 01/02/03/04/05/06/07/08/20/21/22/23 => 27C4096
- ROM 09 => 27C512
- ROM 18/19 => 27C010
- cd63b_1a.jed => GAL16V8

2.2) If you use a 91635B-2 B-board (mask ROM)
- ROMs 01/02/03/04/05/06/07/08 => 27C400
- ROMs 20/21/22/23 => 27C4096
- ROM 09 => 27C512
- ROM 18/19 => 27C010
- cd63b_1a.jed => GAL16V8

3) ROMs installation

All SF2' ROMs must be removed from the B-board.
The PAL named S963B at position 1A has to be removed too.
Double check you've put the devices the right way (the silkscreen should help you)!

3.1) If you use a 91634B-2 B-board (EPROM)
- Install the ROMs in the corresponding socket (ROM 01 in socket 01, etc.)
- Install the GAL16V8 in position 1A (where the S963B was)

3.2) If you use a 91635B-2 B-board (mask ROM)
- Install the ROMs 01/04/05/08/09/18/19/20/21/22/23 in the corresponding socket (ROM 01 in socket 01, etc.)
- Install ROM 02 in socket 03
- Install ROM 03 in socket 02
- Install ROM 06 in socket 07
- Install ROM 07 in socket 06
- Install the GAL16V8 in position 1A (where the S963B was)

```
