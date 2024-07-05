
# VGA Text Modes

VGA text mode cells are stored in memory as 2 bytes. The low byte contains an
ASCII code point, and the high byte contains text attributes.

The memory address for VGA's text screen is `0xB8000`.

Here is some C code to break a cell (a `uint16_t`) into its component parts:

```c
// get ascii code point
uint8_t code = (uint8_t)(cell & 0xFF);

// get attributes
uint8_t attr = (uint8_t)(cell >> 8);

// break out attributes
uint8_t blink = (attr >> 7) & 0x01;
uint8_t bgcolor = (attr >> 4) & 0x07;
uint8_t fgcolor = attr & 0x0F;
```

- `blink` is a boolean controlling whether the text in that cell should blink.
(the blink frequency is about 4.380 Hz).
- `bgcolor` in an index into the 16 color EGA palette controlling the
background color of the cell. Since it only uses 3 bits, you only have access
to the lower eight colors.
- `fgcolor` is an index into the 16 color EGA palette controlling the text
color. Since it is 4 bits wide, it can use the whole 16 color range.

## EGA Palette

![](./images/ega_palette.png)

## Further Reading

- [StackExchange: What are the blinking rates of the caret and of blinking text on PC graphics cards in text mode?](https://retrocomputing.stackexchange.com/questions/27803/what-are-the-blinking-rates-of-the-caret-and-of-blinking-text-on-pc-graphics-car)
- [Wikipedia: VGA text mode](https://en.wikipedia.org/wiki/VGA_text_mode)
