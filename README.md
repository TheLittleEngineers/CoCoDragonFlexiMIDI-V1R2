# CoCoDragonFlexiMIDI-V1R2
 CC-FlexiMIDI-V1R0-03.09.2009 and CC-FlexiMIDI-V1R1-03.09.2009 : Updated to CC-FlexiMIDI-V1R2-12.29.2019 on December 29, 2019  ================================ BACKGROUND: CC-FlexiMIDI-V1R0-03.09.2009 - A Hardware M.I.D.I. Interface Program Pak Cartridge for the Tandy Radio Shack TRS-80 Color Computer 1, 2 and 3, including clones and compatibles (Tano Dragon 64, Dragon Data D32, D64 and D200, Tandy Data Products TDP-100, etcetera) by "Little" John Eric Turner and his father "Big" John Robert (J.R.) Turner. Copyright 09 March 2009. Originally released as Open-Source Hardware on March 9, 2009. Subsequently released under a Creative Commons Attribution-ShareAlike 4.0 International (CC BY-SA 4.0) License on 21 May 2019. ENJOY! Note that the original design is crap, however, a debugged version is forthcoming from "Uncle" Robert "The R.A.T." Allen Turner.  CC-FlexiMIDI-V1R0-03.09.2009 has been updated to CC-FlexiMIDI-V1R1-03.09.2009 by R.A. Turner on May 21, 2019, just over ten years after the initial release of Version 1, Revision 0. Version 1, Revision 1 is Copyright (C) 2019 by the above mentioned parties and is released under a Creative Commons Attribution-ShareAlike 4.0 International (CC BY-SA 4.0) License on 21 May 2019.  =============================== DESCRIPTION: This project is an updated version of the "CC-FlexiMIDI-V1R0-03.09.2009", a hardware MIDI Interface Card for the Tandy Radio Shack TRS-80 Color Computer 1,2 and 3, Dragon Data Dragon D32, D64 and D200, Tandy Data Products TDP-100, Tano Dragon 64 and other clones and compatibles. The original "CC-FlexiMIDI-V1R0-03.09.2009" was designed by my nephew, "Little John", on March 8-9, 2009 as a learning excercise. He was teaching himself to use E.A.G.L.E. in order to design products for the TRS-80 Color Computer line of computers, with the help of his father, my brother, "Big John" or "J.R." as he is known to me. The "CC-FlexiMIDI-V1R0-03.09.2009" was among his very first (learning the art of circuit design) works. It is a terrible design only because he knew nothing about circuit design at the time and it does not appear that his father, "Big John" (J.R.) offered any input in regards to this particular design.  I, "Uncle" Robert "The R.A.T." Allen Turner, have decided to polish up the design a bit and lay out a manufacturable Printed Circuit Board which I will release under a Creative Commons Attribution-ShareAlike 4.0 International (CC BY-SA 4.0) License. As such, I will analyze "Little John's" original design and then provide my improved version. The original "CC-FlexiMIDI-V1R0-03.09.2009" design files, as provided by "Little John" and his father (J.R.) are contained in the "Original (Deprecated)" folder of this archive and should be referenced for this initial analysis of the design.  ================================ ANALYSIS OF ORIGINAL DESIGN: Load up the "CC-FlexiMIDI-V1R0-03.09.2009.sch" schematic file and have it handy for this discussion. Starting with Page 1 of the schematic, we see the expected cartridge (program pak) plug followed by an oddly interesting series of "purported" interrupts, labeled IRQ0* and IRQ1* (the "*" indicates low-level triggering, or active low). These "Interrupts", IRQ0* and IRQ1* appear to go, through disable jumpers, to Pins 1 and 2 of the CoCo Cartridge (Program PAK) Connector. This is both ODD and INTERESTING because Pins 1 and 2 of that connector are -12V and +12V, respectively, on ALL CoCo 1's, TDP-100's and ALL Multi-Pak Interfaces. Both of these pins  are +12V on the Dragon computers. Those self-same pins, however, are NOT connected to anything on ANY stock, unmodified, CoCo 2 or 3 computers. I am thus forced to draw the following conclusion: "Little John" apparently allowed for using Pins 1 and 2 of a CoCo 2 or 3 Cartridge Slot to connect to any desired interrupt within the CoCo 2 or 3. That is, a CoCo 2 or 3 might be modified to connect Pins 1 and/or 2 of the Cartridge Slot to any of the CPU Interrupts, the PIA Interrupt (CART*), G.I.M.E. (A.C.V.C.) Interrupts (CoCo 3 only), etcetera. This is an UNECCESSARY feature of the "FlexiMIDI" design and my initial inclination was to omit it from the design. However, since there are jumpers that allow these "hacked in" custom interrupts to be disabled (removed) via JP1 and/or JP2 or connected together (wire or'ed) via JP3, I have decided to leave them in the design should anyone be so inclined as to use this custom interrupt scheme for experimentation or otherwise.  Also on Page 1 is a fairly standard RESET switch which I would imagine could prove to be quite convenient but potentially problematic if the device is plugged into a Multi-Pak Interface (RESET* is buffered in a single direction in the M.P.I. and should not be triggered from any cartridge plugged into the M.P.I.), Power ON L.E.D. (which I assume might be quite distracting) and some pull-up resistors for the interrupts, custom and legit. Lastly, there is a 220uF Electrolytic Capacitor for Vcc (+5V) filtering. Ideally, a low ESR Electrolytic should be used, however, paralleling a 220uF Electrolytic with a .1uF Ceramic Disc should provide approximately the same result as a single Low ESR type.  Moving on to Page 2, we see a crystal oscillator comprised of three inverters, three resistors and a crystal rated at 1 to 2 MHz. The third inverter actually acts as a buffer and "shaper". Schmitt Trigger inverters are used, although this is not actually necessary it does provide a nice, sharp square wave. Without the hysteresis provided by the schmitt triggers the waveform would appear quasi-sinusoidal at the crystal frequency if viewed on an oscilloscope, but would still function just fine. The output of that third inverter, the buffer stage, is fed into a pair of toggling D type Flip-Flops which provide a divide by two output and a divide by four output, either of which may be selected by jumper JP4. Ideally, we want a solid 500KHz squarewave as the ACIA Clock (which "Little John" labeled "MIDICLOCK" or "MIDICLK"). If a 1MHz crystal is used we would place JP4 on Pins 1 and 2. With a 2MHz crystal we would connect JP4 Pins 2 and 3. This is flexible in that it allows the use of either a 1MHz or 2MHz crystal, whichever might be handy. In my case, and for the redesign, I have a large stock of 16MHz half-can oscillators and so this is what I will be using in the redesign. The 500KHz then, will be derived from the 16MHz oscillator by using the 16MHz to clock a binary counter. At the bottom left of Page 2, we also see three inverters used to invert A7, A4 and A3. This appears to be part of the "address decoding" scheme. Lastly, we see the decoupling capacitors for the inverters and "d-flops". This page (page 2) of the design is fairly solid and well designed.  Moving on to page 3, we see the "heart" of the "CC-FlexiMIDI-V1R0-03.09.2009" MIDI Interface Pak. A 74LS133 13-Input NAND is used for address decoding. The 74LS133 in conjunction with the aforementioned inverters and the ACIA enable lines fully decode the ACIA into two consectutive memory addresses. With this, we can now decipher the addressing of the device. This will be done by writing A15 - A0 and filling in the "bit status" required to enable the ACIA, as follows: ========================================================================= | A15 A14 A13 A12 | A11 A10 A09 A08 | A07 A06 A05 A04 | A03 A02 A01 A00 | |=================|=================|=================|=================| |  1   1   1   1  |  1   1   1   1  |  0   1   1   0  |  0   1   1   x  | |=================|=================|=================|================== |        F        |        F        |        6        |        x        | ========================================================================= Looking at the above table and noting that A0 selects one of the two internal ACIA registers, it is clear that "Little John" mapped the ACIA to 0xFF66 and 0xFF67. This seems ODD because the most popular MIDI Packs designed for use with the Tandy Radio Shack TRS-80 Color Computer decode the ACIA to 0xFF6E and 0xFF6F. A bit of research, however, led to the discovery that the original CoCo MIDI Pack, "The Colorchestra", mapped the ACIA at 0xFF66 and 0xFF67. The "Colorchestra" was released in 1985 by "Color Horizons" and I own two of them. The aforementioned "research" was simply me looking at the "Colorchestra" P.C.B. and deciphering the address decoding which turns out to be 0xFF66-67. I assume that "Little John" arrived at the 0xFF66-67 addressing in a similar manner as to that just mentioned. It would be relatively easy to redesign the "FlexiMIDI" to respond to both sets of addresses thus guaranteeing compatibility with everything. I have decided, however, that the redesign will feature a semi-programmable address decoder allowing the ACIA to be mapped to any two consecutive addresses within the 0xFF6n area. This will allow the "Flexi-MIDI" to be even more flexible. Setting the address decoder to respond to 0xFF66-67 will make the device "Colorchestra" compatible, whilst setting the decoder to respond to 0xFF6E-6F will make it compatible with the MIDI Interfaces produced by Speech Systems, MusicWare, Rulaford Research, Glenside CoCo Club and other CoCo MIDI Packs. As mentioned, it would be relatively easy to hardwire the decoder to respond to both the 0xFF66-67 and the 0xFF6E-6F address ranges, but I feel that this is unneccesary.  Next, we see the 6850 ACIA. This is the "true heart" of the device - a hardware serial port. Looking at the 6850 section of this page of the schematic, we see yet another oddly interesting Interrupt Selection circuit. It is in the form of a 2x4 Jumper Block. This appears to allow selection of any 1 of 4 interrupts to be triggered by the ACIA. IRQ0* and IRQ1* are the previously mentioned "custom" interrupts. NMI* is the 6809 or G.I.M.E./A.C.V.C. Non-Maskable Interrupt Input. The last interrupt on the 2x4 block is the CART* interrupt. This is actually the 6809 or G.I.M.E. IRQ* line that is passed through a PIA inside the CoCo/Dragon. This, the CART* interrupt is the one that should be used for compatibility.  The remaining circuitry on Page 3 are fairly standard circuits for MIDI IN, OUT and THROUGH. These go to 5-pin headers. It appears that "Little John" intended for MIDI Cable ends to be soldered to these headers. The redesign will feature 5-pin DIN MIDI connectors. I do see some potential problems with these MIDI IN, OUT and THRU connections on "Little John's" original design. The first problem that I notice is that the MIDI Ground Pins are connected to the same Ground (common or GND) as the computer and MIDI Pack circuitry. This is no good as it violates the MIDI specification and defeats the purpose of the opto-isolator. Thus, the redesign will sever the ground connection of the DIN connectors from the ground connection of the MIDI Interface Pak circuitry. Next, the 330 Ohm (330R) pull-up resistor connected to the output of the opto-isolator should probably be 270R, however, the device should work fine with the 330R resistor. The redesign will have this changed to 270R. The 10K resistor connected to the "BASE" of the opto-isolator darlington-transistor pair should not be needed. I will allow for it in the redesign for testing purposes. The output of the opto-isolator is sent through two schmitt trigger inverters before being applied to the "Receive Data" input of the ACIA. I am drawing the following conclusion in regards to those two inverters: It seems the design was originally intended for use with a Sharp PC-900 or PC-900V digital opto-isolator which has an internal schmitt trigger, the hysteresis of which provides nice, sharp waveform edges. It appears that "Little John" decided, instead, to use a 6N138 opto-isolator, which does not have hysteresis (schmitt triggering) and thus he must have included the two inverters to alleviate this perceived problem. I am relatively certain, however, that these two inverters are unneccessary and thus I will remove them in the redesign. Had I not used two of the inverters in the hex-inverter package for address decoding, I might have left these two inverters in the redesign, however, I decided the savings of one chip was worth eliminating these two inverters. Hopefully, results will be satisfactory. That is about it for the initial analysis of "Little John's" original design. I shall now proceed to design a slightly improved and, hopefully, manufacturable version of "Little John's CC-FlexiMIDI-V1R0-03.09.2009" Hardware M.I.D.I. Interface Pack. This redesign will be titled: "CC-FlexiMIDI-V1R1-03.09.2009".  ================================ THE REDESIGN: Load up the NEW design from the CURRENT folder in the archive and use it to follow this discussion. Starting with Page 1 of the schematic, I will start the redesign with the Cartridge Program Pak Slot Plug (Edge-Card or Edge-Fingers). This is what will actually plug into the cartridge port on the computer or Multi-Pak Interface (M.P.I.). Next, I will add an edge card socket wired in parallel to the edge-fingers. This is based on "Little John's" Universal Footprint which means that you can fit either a 40-pin card socket or a 40-pin header. This will allow an additional cartridge or other hardware to be plugged directly into the MIDI Interface, thus eliminating the need for a "y-cable" or Multi-Pak Interface. The +5V is filtered with a 220uF Electrolytic Capacitor in parallel with a .1uF Ceramic Disc or Dacron/Polyester/Mylar capacitor. A Power ON L.E.D. indicator is included here, along with an enable/disable jumper. Removing the jumper disables the Power ON L.E.D. should it become a distraction. Next, I'll add in the "CUSTOM" Interrupts, including their jumpers. The jumpers should be REMOVED from all of these if the device is to be used with a CoCo 1 and/or M.P.I. (Multi-Pak Interface) or with ANY of the CoCo Clones and/or compatibles, including the Dragon. In actuality, these jumpers should never be needed and thus should never be installed - they are for experimental purposes only. Removing the jumpers prevents the accidental application of +/-12V to the IRQ* output pin of the ACIA which would fry the ACIA. I have included 680R "failsafe" resistors, but it is likely that they would not prevent a fried ACIA. Lastly, I have included the RESET Switch for convenience. The RESET switch should NEVER be pressed if the device is inserted in a Multi-Pak Interface as you may blow the 74LS367 in the M.P.I. That is about it for Page 1 of the redesign.  Moving on to Page 2: This page is exclusively dedicated to the Baud Rate Generator for the ACIA. Starting at the left, we see the bypass capacitors for the 74LS590 counter. I have used both a 10uF Electrolytic and a .1uF (100nF) Ceramic Disc. This would be important for a ripple counter, however, the LS590 is a synchronous counter and so the Electrolytic could be omitted. I chose to leave it in. There is also a bypass capacitor for the 16MHz oscillator can. I created a dual-footprint for the oscillator can which allows the use of a full or half can oscillator. The 16MHz is fed into the LS590 counter which provides a choice of ten different clocks for the ACIA. For compatibility with existing standard MIDI packs for the CoCo, the 500KHz clock should be selected. The LS590 has an output register which is clocked by the same 16MHz that clocks the counter section. The enable pin of the oscillator is connected to system RESET* which prevents it from oscillating when the system is in a reset state. This pin could have been left floating causing the oscillator to always oscillate. It will work either way.  Page 3 is the semi-programmable address decoder. The 74LS133 in conjunction with the two inverters decodes 0xFF6n - the output will go low on any access to the 0xFF6x range. Only 12 of the 13 inputs to the LS133 were needed. The unused input could be connected to Vcc, E or RESET*. It is important to gate the E Clock in at some point and it could have been done here. I chose to connect the input to RESET*. The ACIA actually has an E Clock input which gates it with the E-Clock so it probably does not need to be gated to the address decoder, though, as you'll see, I gated the E-Clock into the next stage. The 74LS138 decodes 1 of 8 sets of even/odd addresses in the 0xFF6n range (it is enabled by the output of the LS133 and the E-Clock).) So, when any address in the 0xFF6n range appears on the address buss during the high time of the E-clock, the LS138 is enabled and decodes A1-A3 into 1-of-8 chip selects. For maximum compatibility, the 0xFF6E-F output should be selected.  Page 4: This is the 6850 ACIA. This should be either a 68B50 or a 63B50 or 63C50 for operation at up to 2MHz CPU Clocks. This should be pretty self-explanatory. The 6850 datasheet can fill in any necessary details.  Page 5: This is a fairly standard MIDI IN circuit. There are two optocouplers here: a 6N138 and a PC-900 - You should use ONLY ONE, not both. R9 is only needed if you use the 6N138. The diode is a 1N4148 or 1N914A.  Page 6: This is a fairly standard MIDI Out circuit.  Page 7: This is the final page and is a fairly standard MIDI Thru circuit. It simply echoes the MIDI In.  Well, that's about it for a redesign of "Little John's" original. I am ordering some prototype boards to see if this thing will work.  Updated to CC-FlexiMIDI-V1R2-12.29.2019 on December 29, 2019 - This minor update: A Universal 5-Pin DIN component was created and the GND Connection was reconnected to MIDI OUT and MIDI THRU. NO GND connection was made to MIDI IN. This should now create a proper MIDI Interface.
