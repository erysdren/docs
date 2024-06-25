
# PAK

The PAK file format stores uncompressed files in a table. It usually ends with
the ".pak" file extension. It was introduced by id Software with Quake (1996).

**NOTE:** The values here are assumed to be in little-endian byte order.

Header structure:

| Offset | Size | Description |
|---|---|---|
| 0x00 | 4 | Magic identifier ("`PACK`" in ASCII) |
| 0x04 | 4 | Offset to the file table from the start of the file |
| 0x08 | 4 | Number of entries in the file table |

File table entry structure:

| Offset | Size | Description |
|---|---|---|
| 0x00 | 56 | File name and path in ASCII |
| 0x38 | 4 | Offset to the file data from the start of the PAK file |
| 0x3C | 4 | Size of the file data in bytes |

## HROT

HROT (2023) also uses the PAK format, but slightly modified. The magic
identifier is "`HROT`" in ASCII, and the file table entry structure looks like
this:

| Offset | Size | Description |
|---|---|---|
| 0x00 | 120 | File name and path in ASCII |
| 0x78 | 4 | Offset to the data from the start of the file |
| 0x7C | 4 | Size of the data in bytes |
