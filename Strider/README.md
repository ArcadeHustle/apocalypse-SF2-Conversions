```
12 Jul 2017
Conversion SF2' to Strider and first donator!
First I'd like to thank my first donator: Daniel Ladner.
Your support is much appreciated.
To show my thankfulness here is the Strider conversion you were expecting.
Enjoy !

1) Material needed

1.1) If you use a 91634B-2 B-board (EPROM)
 - 10 * 27C4096 ROM (8 for the graphics and 2 for the program)
 - 2 * 27C010 ROM (audio)
 - 1 * 27C512 ROM (audio)
 - 1 * GAL16V8 (PAL)

1.2) If you use a 91635B-2 B-board (mask ROM)
 - 8 * 27C400 ROM (graphics)
 - 2 * 27C4096 ROM (program)
 - 2 * 27C010 ROM (audio)
 - 1 * 27C512 ROM (audio)
 - 1 * GAL16V8 (PAL)

2) ROMs and PAL burning

Now it's time to burn the files on the appropriated devices.

2.1) If you use a 91634B-2 B-board (EPROM)
 - ROMs 01/02/03/04/05/06/07/08/22/23 => 27C4096
 - ROM 09 => 27C512
 - ROM 18/19 => 27C010
 - STH63B.jed => GAL16V8

2.2) If you use a 91635B-2 B-board (mask ROM)
 - ROMs 01/02/03/04/05/06/07/08 => 27C400
 - ROMs 22/23 => 27C4096
 - ROM 09 => 27C512
 - ROM 18/19 => 27C010
 - STH63B.jed => GAL16V8

3) ROMs installation

All SF2' ROMs must be removed from the B-board.
 The PAL named S963B at position 1A has to be removed too.
 Double check you've put the devices the right way (the silkscreen should help you)!

3.1) If you use a 91634B-2 B-board (EPROM)
 - Install the ROMs in the corresponding socket (ROM 01 in socket 01, etc.)
 - Install the GAL16V8 in position 1A (where the S963B was)

3.2) If you use a 91635B-2 B-board (mask ROM)
 - Install the ROMs 01/04/05/08/09/18/19/22/23 in the corresponding socket (ROM 01 in socket 01, etc.)
 - Install ROM 02 in socket 03
 - Install ROM 03 in socket 02
 - Install ROM 06 in socket 07
 - Install ROM 07 in socket 06
 - Install the GAL16V8 in position 1A (where the S963B was)

```
