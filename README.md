# KiCad 8 Library for Commander X16 expansion ports

This is a pair of symbols, and a footprint for the card edge
of expansion cards & cartridges.

## Using this library

This library is available as a KiCad module, that you can install
with the KiCad "Plugin and Content Manager". I suggest doing so.
I suggest including this repo as a git submodule within your project.
Look for the latest release in the "Releases" section of the Github
project.

I'll keep things stable in here. I'm about done adding stuff, and then
it'll just be tweaked to fix errors.

## Footprints

### Expansion card

The mating card edge connector on the X16 is the TE Connectivity
1-5530843-8. The card edge footprint matches the suggested layout
of that part's data sheet. I also sanity checked it against the
appearance of various ISA cards I've got. Per the data sheet, and
PCB manufacturer recommendations, there is space at the bottom of
the card edge for the bevel to be routed away. That's marked by
a keep out zone, which disallows everything, since it'll be machined
away.

The connector is about 0.4mm narrower than the data sheet suggests.
That is to allow for PCB manufacturing tolerances while staying under
the suggested width. And it made the width a whole number. Having
handled the prototype card, I hope that this will make cards designed
with this footprint easier to insert.

The footprint includes a number of user drawing lines.
* Distances to the rear and front of the motherboard
* Two heights ("low profile" and "full height") are marked along the
  rear end of the card. "Low profile" is the height of the TexElec
  prototyping card. "Full height" is what I think you can safely get
  away with before you should start taking your own measurements of
  whatever slot cover you're going to use.
* The rear end of the card has a number of lines indicating what space
  you've got available to go below the top of the card socket on the
  motherboard.
* I suggest not going below the top of the socket on the
  other side, as that will require 3D reasoning to ensure you remain
  clear of C53 (a tall electrolytic).

### Cartridge

Most of the above applies to the cartridge as well. Confirmation of
dimensions was carried out with a blank case purchased from Muramasa
Entertainment.

Be aware that the front copper layer faces DOWN relative to the CX16
motherboard, not up as one might assume. The footprint is designed
assuming the front of the cartridge case will face UP.

The cart has more of the board edge defined, included in an internal
hole.  There are also a number of keep out zones defined, for places
where the case comes into contact with the PCB for retention purposes.

The top edge of the board is missing from the footprint, so you may
make a board of minimum height, or longer if you wish. The reasonable
maximum is marked on the user drawings layer, but you might have
larger cases available to you, or you might machine away a portion of
the case and have your board extend out. (Connectors? A display?  I
look forward to seeing what you come up with.)

Keep in mind your limited vertical space. There is approximately
11.8mm of total internal height for board; I think it is divided about
evenly between the two sides, but I made no attempt to measure
this. Certainly if any of your components exceed ~4mm in height, you
should take some careful measurements and do some prototyping of your
own.

## Symbols

### Expansion Port / Cartridge

There are two symbols, the main one is the "physical" symbol, where
all pins are positioned to correspond to their physical pin. The "Alt"
symbol rearranges things, so that groups of pins are in order, the
groups have a small separation between them, and the duplicated power
pins are on top of each other. (This tells KiCad that they're all the
same net, and reduces the number of lines you've got to draw.)

#### Notes about the pins

The IOx pins are driven by a 74ACT138, making them active low signals.
I've added bars to their signal names to reflect this. I realize that
the 6502 derived signals use a "B" to indicate that they're active
low, but I'm leaving them be to better match existing 6502 docs.
The X16 docs don't mark the lines with any character, so I figure the
bar conveys the information best while ensuring nobody goes searching
for the "IO4B" signal and wonders why they can't find it.

The AUDIO_L and AUDIO_R pins are marked as bidirectional, because
those pins are connected to the summing node of the system mixer
(through a resistor). An expansion card could choose to "listen" to
that signal, or emit audio of its own into that node.  The VERA and
YM2151 outputs are buffered by TL074 op amps before being fed into the
overall mixer. If you want to emit audio, you might do the same.
Please keep in mind that other expansion devices might be driving
these pins directly with op amps. Consider putting a resistor between
you and the bus, so as to avoid your amp fighting another amp. Or
your device being back-driven by another device's amp.

Finally, while the SDA pin is bidirectional, the SCL pin is marked as
an output of the edge connector, to match the KiCad convention of I2C
masters having SCL as an output. (This is despite the ability of I2C
slaves to stretch the clock signal as necessary.)

### I2C Port

This is a simple symbol intended to be used with a right-angled
2x5 female header, to make a little board that sticks up from the
motherboard. Something like the Würth Elektronik 613010243121.

### User port

Same idea, but you'd need a larger connector.

### Audio option

TODO

### Other headers

* The extra 65C22 pin header doesn't have enough to support a complete
  device (no power), so I don't consider it interesting enough to
  bother with.
* SNES 3/4 really only has one thing you could do with it, which is
  bring it out to some more controller ports, so there's not much
  reuse value for a symbol.
* Front panel should just go to LEDs via cables, not much sense in one
  larger connector to it.
* For the VERA header, I'd sooner make a symbol representing a place
  to plug a VERA into, to encourage reuse of them in other applications.

## !!! WARNING !!!

I've had the cartridge footprint manufactured, and it seems to be fine.
Everything here remains use-at-your-own-risk, of course, but I will be
using them without hesitation going forward.

## Fixes welcomed

If you spot any errors, please report them. PRs welcomed.
I do ask that you make a point of including evidence for any
dimensional changes.

## Related

You might be interested in the following:

* https://www.kicad.org/
  * Consider donating to KiCad if you find it, or this library, useful.
* https://github.com/Alarm-Siren/6502-kicad-library
  * RetroLib, a KiCad symbol library for 6502 and related retro microchips
