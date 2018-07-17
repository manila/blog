---
layout: post
title: "Tic-Tac-Toe with Bitboards"
date: 2018-07-16 08:42:00 -0600
---

A Tic-Tac-Toe game using bitboards to store player state.

This post assumes you can read a bit of C code and you have a basic understanding of how numbers can be represented in binary.

What is a bitboard you might ask? It is a data structure that represents player positions on a board by using a series of bits.

Here is an example of how individual bits correspond to positions on a tic-tac board (3x3 grid), 

```
 1 | 2 | 3
---|---|---      1|2|3|4|5|6|7|8|9 -> Position on Board
 4 | 5 | 6   ->  1|0|0|0|1|0|0|0|1 -> Occupying Bit 
---|---|---
 7 | 8 | 9

```

Let’s transform that same bit sequence from above to better represent a board.

```
                     1 | 0 | 0
             100    ---|---|---
100010001 -> 010 ->  0 | 1 | 0
             001    ---|---|---
                     0 | 0 | 1 

```

See how each individual bit corresponds to a place on the board after we’ve wrapped it around to form a square, this is a bitboard.

Let implement this in C

Now since there isn’t a datatype in C that is 9 bits long we’ll need to use a full blown integer which is 16 bits long ¯\\\_(ツ)\_/¯

Two boards one for each player (X's and O's)\:

```c
int bitboard[2] = {0, 0}; 

```

Why use bitboards to represent the game state? They are fast and relatively easy to manipulate.

For example to check a horizontal win for each player board we shift (>>) the bitboard over a few places and run an and (&) operation. If the value is greater than 1 we have a win\:

```c
int check_win_horizontal(int bitboard)
{
     return ((bitboard) & (bitboard >> 1) & (bitboard >> 2));
}

```

The function above checks for horizontal wins, here’s how\:

&nbsp;&nbsp;&nbsp; **111**000000 RIGHT-SHIFT 1 place(s)
&nbsp;&nbsp;= 0**111**00000

&nbsp;&nbsp;&nbsp; **111**000000 RIGHT-SHIFT 2 place(s)
&nbsp;&nbsp;= 00**111**0000

&nbsp;&nbsp;&nbsp; 11**1**000000 (original Bitboard)
&nbsp;&nbsp;&nbsp; 01**1**100000 (bitboard right-shifted 1 place)
AND 00**1**110000 (bitboard right-shifted 2 places)
&nbsp; = 00**1**000000 (1 bit set makes this a “true” value)


If we represent the bits from above as a board by wrapping after 3 characters

11**1**&nbsp; &nbsp;01**1**&nbsp; &nbsp;00**1**&nbsp; &nbsp;00**1**
000 & 100 & 110 = 000
000&nbsp; &nbsp;000&nbsp; &nbsp;000&nbsp; &nbsp;000

Simply put if there is overlap in placement after shifting the board a couple times places we know there were three in a row.

Cool! as you can see bitboards are powerful in that they can be manipulated using fast low level operations, [here](https://github.com/manila/match4) is another example of bitboards in a match 4 implentation.

You can download the [source code here](https://github.com/manila/tic-tac-toe/archive/master.zip) ([from github](https://github.com/manila/tic-tac-toe)).

Questions? Corrections? Bored?

Email me at hi at this domain.

-Manila

