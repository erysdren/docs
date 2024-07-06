
author: Justme

answered; Oct 5, 2023 at 20:20

edited: Oct 8, 2023 at 15:30

source: https://retrocomputing.stackexchange.com/a/27805

---

Yes, your hunch is correct.

MDA, original card from IBM, reverse-engineered from schematics :

The cursor blink signal is not handled by the Motorola 6845 CRTC. It is handled by dividing VSYNC with 74LS393 by 16, so it is on for 8 frames and off for 8 frames. As the original MDA has 16.257 MHz pixel clock, 882 dot clocks per HSYNC, and 370 lines per frame, the VSYNC rate is 49.816 Hz and cursor blinks at 3.114 Hz. Blinking text blinks at the cursor rate divided down by 2.

CGA, original card from IBM, reverse engineered from schematics :

Practically same as MDA but with different vertical rate causing different blink rate. VSYNC is externally divided by 16 with 74LS393 for cursor blink and again by two more for text blink. As the original CGA card runs from motherboard oscillator signal, the pixel clock is 315/22 MHz, and as there are 912 dot clocks per HSYNC, and 262 lines per frame, the VSYNC rate is 59.923 Hz, and cursor blinks at 3.745 Hz. Text blink again with cursor rate divided by 2.

For original IBM EGA and VGA, it is harder to reverse-engineer as the signals are internal to the LSI chipsets. I will double check the hypothesis on a real PS/2.

In general, EGA and VGA should be assumed similar. Cirrus Logic 542X series VGA technical reference says that characters blinks at VSYNC divided by 32, which matches MDA and CGA, so the assumption is cursor blinks again at VSYNC/16 rate.

As VGA text mode uses 28.322 MHz dot clock and 900 dot clocks per HSYNC, and 449 lines per frame, VSYNC rate is 70.087 and cursor should blink at 4.380 Hz.

For EGA, text mode uses 16.257 MHz dot clock and 744 dot clocks per HSYNC, and 364 lines per frame, VSYNC rate is 60.030 Hz, and cursor should blink at 3.752 Hz, almost same rate as on CGA due to almost same refresh rate.

Now, I would have to guess that the blink rates should have relatively straightforward implementation in clones, and any differences are due to using different vertical total lines, horizontal dot clocks per line, or different pixel clock rate. One example is the HGC Hercules Graphics Card. While otherwise the IBM BIOS sets the video text mode identically, it uses a slower 16.000 MHz pixel dot clock.

I managed to verify the VGA results on a genuine IBM PS/2 Model 30-286 and fixed a typo in the values, VGA blinks at 4.380 Hz.

I attached a photosensor to sound card microphone port, aimed it at both blinking white text blocks and blinking white cursor blocks. and recorded the "sound" in Audacity. Ten on/off pulses of blinking text was measured to be approximately 219168 samples at 48000 sampling rate, which equals 2.190 Hz text blink, and ten on/off pulses of blinking cursor was measured to be 109584 samples at 48000 Hz.

From the measurements it can be deducted that 16 text blink periods is approximately 70.08 Hz and 32 cursor blink periods is also approximately 70.08 Hz. By using the measured sample counts to compare with the theoretical VGA VSYNC or pixel clock rates, the measurements are only -26 PPM and +114 PPM in error compared to actual value.
