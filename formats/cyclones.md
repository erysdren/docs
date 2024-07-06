
# CyClones

CyClones (1994) stores its data in a RIFF-like chunk format. The chunks are
read until the end of the file is reached.

## Chunk Structure

| Offset | Size | Description |
|---|---|---|
| 0x00 | 4 | Chunk identifier |
| 0x04 | 2 | Unknown (chunk type?) |
| 0x06 | 4 | Length of chunk data in bytes |
| 0x0A | * | Chunk data |
