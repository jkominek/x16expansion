# KiCad 7 Library for Commander X16 expansion ports

This is a pair of symbols, and a footprint for the card edge
of expansion cards & cartridges.

### Footprint

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
the suggested width. And it made the width a whole number.

Whenever I can get some additional mechanical information about
the cards (maximum extents, mainly) I will update the footprint
with lines to indicate those.

### Symbols

There are two symbols, the main one is the "physical" symbol, where
all pins are positioned to correspond to their physical pin. The "Alt"
symbol rearranges things, so that groups of pins are in order, the
groups have a small separation between them, and the duplicated power
pins are on top of each other. (This tells KiCad that they're all the
same net, and reduces the number of lines you've got to draw.)

#### Notes about the pins

The IO pins are driven by a 74ACT138, making them active low signals.
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

## !!! WARNING !!!

I haven't manufactured anything based on these. Use at your own risk.
I've got an X16 coming in the February batch. If I can get mechanical
information before then, I'll have cards ready for testing when it
arrives. If I can't get mechanical information before then, I'll only
be able to place orders some time after I get it.

## Fixes welcomed

If you spot any errors, please report them. PRs welcomed.
I do ask that you make a point of including evidence for any
dimensional changes.