
# CSQC Outline

**CSQC** (Client-Side QuakeC) is a fan-made extension to the Quake engine,
allowing a separate QuakeC module to handle HUD drawing and other screen
management, which is traditionally hardcoded into the engine.

The actual numbered QuakeC extensions that CSQC provides are poorly defined. At
one time it seemed to cover the #300-#399 range, but this was not strictly
followed and is probably no longer true.

Different engines have varying levels of CSQC support. Here is a list, ordered
subjectively by "completeness":

1. [FTEQW](https://www.fteqw.org/)
2. [DarkPlaces](https://hemebond.gitlab.io/darkplaces-www/)
3. [QuakeSpasm Spiked](https://triptohell.info/moodles/qss/)
4. [vkQuake](https://github.com/Novum/vkQuake)
5. [Ironwail](https://github.com/andrei-drexler/ironwail)

The main vector by which CSQC is used is a separate QuakeC module compiled with
the `CSQC` preprocessor macro defined and the additional extensions made
available with a different "defs" file. When compiled, it should be named
`csprogs.dat` and placed in the game folder alongside the standard `progs.dat`.

CSQC is also capable of having its own set of entities, which only exist to
that client. This is primarily used for drawing things like particles, sprites,
complex HUD effects or anything else that other players shouldn't be able to
see.

**NOTE**: This document is absolutely not complete, and it likely never will
be. I just wanted to try and write out what I know to hopefully be useful to
others.

## QuakeC Entry Points

### CSQC_Init

`void(float apilevel, string enginename, float engineversion) CSQC_Init;`

This function is called when the CSQC module is first loaded. The function
arguments have no standard and may well contain anything.

### CSQC_Shutdown

`void() CSQC_Shutdown;`

Called when the engine is shutting down or the client has been disconnected
from a server. This is where you should save your persistent client settings.

### CSQC_DrawHud

`void(vector virtsize, float showscores) CSQC_DrawHud;`

This is part of the "SimpleCSQC" subset. If the user does not wish to handle
drawing the full screen (or the engine does not allow that control), you can
implement this instead. `virtsize` is the size of the screen area in virtual
pixels, and `showscores` is a boolean indicating whether or not the user is
requesting to see the multiplayer scoreboard.

### CSQC_DrawScores

`void(vector virtsize, float showscores) CSQC_DrawScores;`

This is also part of "SimpleCSQC". This will be called by the engine when the
client is viewing the multiplayer scoreboard.

### CSQC_UpdateView

`void(float vwidth, float vheight, float notmenu) CSQC_UpdateView;`

This function gives you full control over the rendered screen. `vwidth` is the
width of the screen in virtual pixels, and `vheight` is the height. `notmenu`
will be FALSE If the player has paused the game or is otherwise viewing an
engine-handled menu.
