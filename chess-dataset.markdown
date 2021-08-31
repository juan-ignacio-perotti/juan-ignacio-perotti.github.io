---
layout: page
title: Chess dataset
permalink: /chess-dataset/
exclude: true
---

# Introduction

Here, we provide the chess dataset in convenient formats (see below). You can also download it from the [original source][original-source]. The original dataset is stored in an efficient (in terms of space and access speed) custom format that cannot be easily transformed into **PGN** (Portable Game Notation) format without using a specially devised software called [chessDB][original-source].

The original dataset contains around 3.5 million chess games and useful metadata about them. If you study the number of games played as a function of time, you will see three periods. The last period, 1998-2007, contains most games, I suppose thanks to the increased use of the internet. From this last period, we filtered games with incomplete metadata, such as an incomplete date of play, from where around 1.4 million games remained. This is the dataset we use in our papers: [[Perotti (2013)]][perotti2013innovation] and [[Schaigorodsky (2014)]][schaigorodsky2014memory].

We provide the following versions of the dataset:

-  Original data transformed to **PGN** format [all.pgn.zip][all.pgn.zip]  [831 MB]
-  **A simple txt format** (created by us) with labels for easy filtering of the games according to their attributes [all_with_filtered_annotations.txt.zip][all_with_filtered_annotations.txt.zip]  [747 MB]

# Description of the txt format

This format is convenient for working with a combination of linux bash and Python. Below there is a short explanation of how to use it.

In the file **all_with_filtered_annotations.txt**, lines starting with the character # correspond to comments or the description of the columns:

    # SOME COMMENTS...
    # 1.t 2.date 3.result 4.welo 5.belo 6.len 7.date_c 8.resu_c 9.welo_c 10.belo_c 11.edate_c 12.setup 13.fen 14.resu2_c 15.oyrange 16.bad_len 17.game...

After the comments and description of the columns, each line corresponds to a given game. The last columns (that correspond to the game moves) are separated from the previous columns (that correspond to the game attributes) with the token "###". In this way it is easy to separate the string of game moves from the string of game attributes. From columns 1 to 16, you will find the game attributes:

1. Position of the game in the original **PGN** file. 
2. **Date** at which the game was played (the format is `year.month.day`).
3. Game **result** specified inside brackets in the **PGN** file. The value can be 1,0,-1 corresponding to white win, draw or loose, respectively.
4. **ELO** of **white** player (an integer number).
5. **ELO** of **black** player (an integer number).
6. **Number of moves** in the game (for some games it may be zero!)
7. **date corrupted?** : Flag indicating if the date (in `year.month.day`) is corrupted or missing. The label should be `date_true`, meaning the date is corrupted or missing, or `date_false` otherwise. Values `date_true` or `date_false`.
8. **result corrupted?**: Flag indicating if **result** is corrupted or missing. Values `result_true` or `result_false`.
9. **white ELO corrupted?**: Flag indicating if white ELO is corrupted or missing. Values `welo_true` or `welo_false`.
9. **black ELO corrupted?**: Flag indicating if white ELO is corrupted or missing. Values `belo_true` or `belo_false`.
11. **event data is corrupted?**: Flag indicating if the date of the event is corrupted or missing. Values `edate_true` or `edate_false`.
12. **setup**: If it is true, then the game initial position is specified. This is used when playing Fischer Random Chess for example. Values `setup_true` or `setup_false`.
13. **fen**: It is related to attribute 12. Values `fen_true` or `fen_false`. 
14. **result 2° corrupted**: In the original file the result is provided in two places. At the end of each sequence of moves and in the attributes part. This flag indicates if the result is (is not) properly provided after the sequence of moves (just for checking consistency in the **PGN** file). Values `result_true` or `result_false`.
15. **year out of range?**: This flag is false only for games with dates in the range of years [1998 - 2007]. Values `oyrange_true` or `oyrange_false`.
16. **bad length?**: when this flag is `blen_true` (`blen_false`), if the provided length of the game is (is not) good.
17. Finally, the **sequence of moves**. Each move has a number and a letter `W` (white) or `B` (black) indicating the th-move of the white or black player, respectively.

Below, I give an example of how one line (and therefore one game) looks like in this file:

    1 2000.03.14 1-0 2851 None 67 date_false result_false welo_false belo_true edate_true setup_false fen_false result2_false oyrange_false blen_false ### W1.d4 B1.d5 W2.c4 B2.e6 W3.Nc3 B3.Nf6 W4. cxd5 B4.exd5 W5.Bg5 B5.Be7 W6.e3 B6.Ne4 W7.Bxe7 B7.Nxc3 W8.Bxd8 B8.Nxd1 W9.Bxc7 B9.Nxb2 W10.Rb1 B10.Nc4 W11.Bxc4 B11.dxc4 W12.Ne2 B12.O-O W13.Nc3 B13.b6 W14.d5 B14.Na6 W15.Bd6 B15.Rd8 W16.Ba3 B16.Bb7 W17.e4 B17.f6 W18.Ke2 B18.Nc7 W19.Rhd1 B19.Ba6 W20.Ke3 B20.Kf7 W21.g4 B21.g5 W22.h4 B22. h6 W23.Rh1 B23.Re8 W24.f3 B24.Bb7 W25.hxg5 B25.fxg5 W26.d6 B26.Nd5+ W27.Nxd5 B27.Bxd5 W28.Rxh6 B28.c3 W29.d7 B29.Re6 W30.Rh7+ B30.Kg8 W31.Rbh1 B31.Bc6 W32.Rh8+ B32.Kf7 W33.Rxa8 B33.Bxd7 W34.Rh7+

**IMPORTANT**: In the file `all_with_filtered_annotations.txt` the games are NOT in chronological order because the date is not always specified.

# Bash magic

You can use a simple linux(bash) command to quickly filter games with undesired properties from this file. 
For instance, to filter games with missing or corrupted **white ELO** you can write

    $ cat all_with_filtered_annotations.txt | grep 'welo_false' > all_with_filtered_annotations_with_wELO.txt

To filter games with missing or corrupted **date**, you can write

    $ cat all_with_filtered_anotations.txt | grep 'date_false' > all_with_filtered_annotations_with_dates.txt

To sort the result, you can write

    $ sort -k2 all_with_filtered_annotations_with_dates.txt > all_with_filtered_annotations_sorted_by_date.txt

[original-source]: http://chessdb.sourceforge.net/
[all.pgn.zip]: https://drive.google.com/file/d/0Bw0y3jV73lx_NElnLWVlNG9KNkU/view?usp=sharing&resourcekey=0-9y_tHmhEmmCSa-pgtrB_vg
[all_with_filtered_annotations.txt.zip]: https://drive.google.com/file/d/0Bw0y3jV73lx_aXE3RnhmeE5Rb1E/view?usp=sharing&resourcekey=0-b46EjMSfJpiIxEL8Pl8QvQ
[perotti2013innovation]: https://iopscience.iop.org/article/10.1209/0295-5075/104/48005/meta
[schaigorodsky2014memory]: https://www.sciencedirect.com/science/article/abs/pii/S0378437113009126
