# cfs-suite-bridge-brd02

OTA releases for the **CFS-SUITE-BRIDGE-02** bridge board. `manifest.json` is
what the PC app reads; the `_IMG.bin` files beside it are app-slot images.

## v3.0.3 — KEY is the reset button

Press and release the KEY button and the board restarts. This board has no
other way back: nRESET is reconfigured as a GPIO in its option bytes and the
part ships read-protected, so neither a reset pin nor a debugger can restart a
bridge whose USB has stopped answering.

The reset fires on the **release** edge, after a 50 ms debounce on each. That
ordering matters — holding KEY through power-on is how the board is told to
boot with pinmap v1, so a reset issued while the button is still down would
land back in v1. For the same reason the button is ignored until it has been
seen released once, or booting into v1 would end the moment you let go.

Three LED pulses acknowledge the press before the board resets.

## A rename happened here

Builds before v3.0.0 were published under the product name
**CFS-BRIDGE-CWM30C8**. Those files are still in this repository and their
URLs still work, but they are **not** listed in `manifest.json` and cannot be
installed on a renamed board: the product name is hashed into every image
header and a bootloader only accepts its own. Converting a board from the old
name to the new one needs an SWD factory flash, not an OTA.
