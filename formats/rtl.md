
# RTL

The RTL file format stores levels for Rise of the Triad (1994). It's based on
the TED5 file format used by id Software for Wolfenstein 3D and the Commander
Keen games. The RTL format usually has the `.rtl`, `.rtc`, `.rtlx` or `.rtcx`
file extensions.

**NOTE:** The values here are assumed to be in little-endian byte order.

The file format has five different magic identifiers, each identifying its
use to the engine and how it was made:

| Identifier | Description |
|---|---|
| `RTL\0` | Singleplayer levels |
| `RTC\0` | COMM-BAT levels |
| `RTR\0` | Generated with RANDROTT |
| `RXL\0` | Singleplayer levels (Ludicrous Edition) |
| `RXC\0` | COMM-BAT levels (Ludicrous Edition) |

There's also two different known versions:

| Version | Description |
|---|---|
| `0x0101` | Version 1.1 |
| `0x0200` | Version 2.0 (Ludicrous Edition) |

## Header

File header structure:

| Offset | Size | Description |
|---|---|---|
| 0x00 | 4 | Magic identifier in ASCII |
| 0x04 | 4 | Version identifier |

In the 1.1 version of the format, there are exactly 100 map header structures
immediately following the file header:

| Offset | Size | Description |
|---|---|---|
| 0x00 | 4 | Boolean signifying if the map is used |
| 0x04 | 4 | Map CRC value |
| 0x08 | 4 | RLE encoding tag |
| 0x0C | 4 | Map flags |
| 0x10 | 4 | Offset to compressed walls plane from the start of the file |
| 0x14 | 4 | Offset to compressed sprites plane from the start of the file |
| 0x18 | 4 | Offset to compressed info plane from the start of the file |
| 0x1C | 4 | Size of compressed walls plane in bytes |
| 0x20 | 4 | Size of compressed sprites plane in bytes |
| 0x24 | 4 | Size of compressed info plane in bytes |
| 0x28 | 24 | Map name |

## Extended Data Chunks

In the Ludicrous Edition version of the format, there is a secondary header
following the file header which gives information on the extended data
available:

| Offset | Size | Description |
|---|---|---|
| 0x00 | 8 | Offset to extended chunks table from the start of the file |
| 0x08 | 8 | Number of entries in the extended chunks table |

Each extended chunk has this structure:

| Offset | Size | Description |
|---|---|---|
| 0x00 | 16 | Chunk type identifier in ASCII |
| 0x10 | 8 | Offset to chunk data from the start of the file |
| 0x18 | 8 | Size of chunk data in bytes |

Here are the known extended chunk types:

| Value | Description |
|---|---|
| `MAPSET` | Standard map headers |
| `MAPINFO` | JSON metadata |
