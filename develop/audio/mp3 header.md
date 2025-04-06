## ID3


---

## MPEG

**Documents**
[mpeg audio layer I/II/III frame header] [1]


**BITRATE INDEX**

| bits | V1,L1 | V1,L2 | V1,L3 | V2,L1 | V2,L2 & L3 |
| ---- | ----- | ----- | ----- | ----- | ---------- |
| 0000 | free  | free  | free  | free  | free       |
| 0001 | 32    | 32    | 32    | 32    | 8          |
| 0010 | 64    | 48    | 40    | 48    | 16         |
| 0011 | 96    | 56    | 48    | 56    | 24         |
| 0100 | 128   | 64    | 56    | 64    | 32         |
| 0101 | 160   | 80    | 64    | 80    | 40         |
| 0110 | 192   | 96    | 80    | 96    | 48         |
| 0111 | 224   | 112   | 96    | 112   | 56         |
| 1000 | 256   | 128   | 112   | 128   | 64         |
| 1001 | 288   | 160   | 128   | 144   | 80         |
| 1010 | 320   | 192   | 160   | 160   | 96         |
| 1011 | 352   | 224   | 192   | 176   | 112        |
| 1100 | 384   | 256   | 224   | 192   | 128        |
| 1101 | 416   | 320   | 256   | 224   | 144        |
| 1110 | 448   | 384   | 320   | 256   | 160        |
| 1111 | bad   | bad   | bad   | bad   | bad        |

**MPEG HEADER**

| Length (bits) | DESC                                                                                                                                                                          |
| ------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 11            | Frame Sync                                                                                                                                                                    |
| 2             | MPEG Audio Version ID<br>00 - MPEG Version 2.5 (later extension of MPEG 2)<br>01 - reserved<br>10 - MPEG Version 2 (ISO/IEC 13818-3)<br>11 - MPEG Version 1 (ISO/IEC 11172-3) |
| 2             | Layer description<br>00 - reserved<br>01 - Layer III<br>10 - Layer II<br>11 - Layer I                                                                                         |
| 1             | Protection bit<br>0 - Protected by CRC<br>1 - Not Protected                                                                                                                   |
| 4             | Bitrate Index                                                                                                                                                                 |


---

### refrences

[1]: http://www.mp3-tech.org/programmer/frame_header.html