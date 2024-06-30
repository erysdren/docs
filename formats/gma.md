
# GMA

The GMA (Garry's Mod Addon) file format is a basic container for user-created
files for use in the game Garry's Mod. The values here are assumed to be in
little-endian byte order.

**NOTE:** This file format is full of null-terminated data, and thus there are
no reliable offsets except for the header. Most offsets will be omitted.

**NOTE:** I don't know how the CRCs in this format are calculated.

## Header

File header structure:

| Offset | Size | Description |
|---|---|---|
| 0x00 | 4 | Magic identifier (`GMAD` in ASCII) |
| 0x04 | 1 | GMA version (the most recent version is 3) |
| 0x05 | 8 | Steam user ID |
| 0x0D | 8 | Addon creation timestamp |
| 0x15 | 1 | Padding |

What follows is three null-terminated strings starting at offset 0x16:

1. Addon name
2. Addon description
3. Addon author

After these three null-terminated strings, there is a 32-bit unsigned integer
indiciating the addon version.

## Directory

The directory is a null-terminated list of files contained in the addon. The
directory entry structure is as follows:

1. 32-bit unsigned integer containing the file's ID, starting at 1. If this value
is 0, then you've reached the end of the file directory.
2. Null-terminated string containing the path to the file, relative to the
Garry's Mod game directory.
3. 64-bit unsigned integer containing the file's size.
4. 32-bit unsigned integer containing the file's CRC.

## File data

After the directory is a large blob containing the file data. There are no
offsets into this blob, they must be calculated from the sizes in the
directory entries.

At the very end of the file is a 32-bit unsigned integer containing a CRC for
the GMA file.
