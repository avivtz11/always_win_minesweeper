# always_win_minesweeper
Hacked Minesweeper to count everything as a win!

![alt text](https://github.com/avivtz11/always_win_minesweeper/blob/master/screenshots/won_pressing_bomb.PNG)

### Tools
- IDA
- Hex Workshop
- [Online x86 Assembler](https://defuse.ca/online-x86-assembler.htm)

### Steps
- Losing or winning plays a sound, so I looked in the IDA imports tab and found the function *PlaySoundW*
- It had 3 callers - the one at 0x1003937, it was the only one with variating value for the sound. Good for us.
- Debugging with breakpoints at the pushing of the values for the sound, revealed the purpose of every value (win, lose, and tick of the clock)
![alt text](https://github.com/avivtz11/always_win_minesweeper/blob/master/screenshots/pushing_sound.PNG)
- This calling function (0x10038ED) had 3 callers. Debugging with breakpoints showed that one of them was the calling one in a lose and a win (0x100347C)
- It was calling 0x10038ED with value of 2 when winning, and 3 when losing. It got this information as a parameter (1 when winning, 0 when losing)
- The callers of 0x100347C pushed 1 or 0 as constants and not from memory or registers so this is the *gameover function*
- Now patching.

I patched 0x100347C argument to always be 1 by orring with 1 (since it gets just a boolean argument)

I overrode:

```
1003484                 mov     esi, [esp+4+arg_0]
1003488                 add     al, al
```

With:

```
1003484                 jmp     0x1004A60
```

And added this:

```
1004A60                 or      [esp+4+arg_0], 1
1004A65                 mov     esi, [esp+4+arg_0]
1004A69                 xor     eax, eax
1004A6B                 jmp     loc_100348A
```
