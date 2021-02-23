# always_win_minesweeper
Hacked Minesweeper to count everything as a win!

### Tools
- IDA
- Hex Workshop
- [Online x86 Assembler](https://defuse.ca/online-x86-assembler.htm)

### Steps
- Losing or winning plays a sound, so I looked in the IDA imports tab and found the function PlaySoundW
- It had 3 callers - the one at 0x1003937, it was the only one with variating value for the sound !!!!!!!!!!!!!!!!!!!!!!!!!!!!add picture
- 
