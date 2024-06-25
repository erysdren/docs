
# WAD

The WAD file format stores "lumps" of data in a table, either referenced by
name or index. It usually ends with the ".wad" file extension.

**NOTE:** The values here are assumed to be in little-endian byte order.

## Version 1

Version 1 was introduced with DOOM (1993).

Header structure:

| Offset | Size | Description |
|---|---|---|
| 0x00 | 4 | Magic identifier ("IWAD" or "PWAD" in ASCII) |
| 0x04 | 4 | Number of entries in the lump table |
| 0x08 | 4 | Offset to the lump table from the start of the file |

Lump table entry structure:

| Offset | Size | Description |
|---|---|---|
| 0x00 | 4 | Lump data offset from the start of the file |
| 0x04 | 4 | Lump data size in bytes |
| 0x08 | 8 | Lump Name |

### Prey

The leaked 1995 version of Prey by Apogee Software uses a slightly modified
version of the Version 1 WAD format. The structures are the same, except for
the lump table entry structure which looked like this:

| Offset | Size | Description |
|---|---|---|
| 0x00 | 4 | Lump data offset from the start of the file |
| 0x04 | 4 | Lump data size in bytes |
| 0x08 | 4 | Lump data type |
| 0x0C | 8 | Lump Name |

Known type value meanings:

| Value | Description |
|---|---|---|
| 0x08 | Palette |
| 0x0B | Mip texture |
| 0x11 | Colormap |

## Version 2

Version 2 was introduced with Quake (1996).

Header structure:

| Offset | Size | Description |
|---|---|---|
| 0x00 | 4 | Magic identifier ("WAD2" in ASCII) |
| 0x04 | 4 | Number of entries in the lump table |
| 0x08 | 4 | Offset to the lump table from the start of the file |

Lump table entry structure:

| Offset | Size | Description |
|---|---|---|
| 0x00 | 4 | Lump data offset from the start of the file |
| 0x04 | 4 | Lump data size in bytes (compressed) |
| 0x08 | 4 | Lump data size in bytes (uncompressed) |
| 0x0C | 1 | Lump type |
| 0x0D | 1 | Boolean signifying compression |
| 0x0E | 2 | Padding |
| 0x10 | 16 | Lump Name |

## Version 3

Version 3 was introduced with Half-Life (1998). It has no structure differences
whatsoever, but contains different lump data types from Quake. The magic
identifier is "WAD3" in ASCII.

## Versions 4 and 5

The leaked Half-Life 2 (2004) source code contains a handful of references to
later revisions of the WAD format. There are no known examples, but according
to the source code the magic identifiers were "WAD4" and "WAD5" in ASCII, and
the lump table entry structure looked like this:

| Offset | Size | Description |
|---|---|---|
| 0x00 | 4 | Lump data offset from the start of the file |
| 0x04 | 4 | Lump data size in bytes (compressed) |
| 0x08 | 4 | Lump data size in bytes (uncompressed) |
| 0x0C | 1 | Lump type |
| 0x0D | 1 | Boolean signifying compression |
| 0x0E | 2 | Padding |
| 0x10 | 128 | Lump Name |
