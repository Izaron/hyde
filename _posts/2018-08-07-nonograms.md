---
layout: post
title: Solving colored Japanese crosswords with the speed of light
---

*This is a translation of my post from the finest technical news and programming site for Russian-speaking people - habr.com. The post got rating +88 and has been the post of a day (the post which got the biggest rating that day).*

Japanese crosswords (aka nonograms) are picture logic puzzles. The solution of puzzles based on numbers which are placed to the left of the rows and on the top of the columns.

The size of the puzzle may be as big as 150x150. A player calculate the color of each cell with some special logical methods. The solution process can take a couple of minutes on puzzles for newcomers and dozens of hours on hard ones.

A good algorithm is capable to solve the task too much faster. In the post I described how to write a solution which works almost instantly, using most convenient algorithms (which lead to a solution in general), and their optimizations and features of C++ (which decrease the running time in dozens of time).

<img align="center" src="https://habrastorage.org/webt/w1/tw/-c/w1tw-caq1d9vswcrqfapdufcbfk.gif" />
<!--break-->

## Game rules
At the start the canvas of the image is filled with white. For vanilla black-white crosswords it is needed to restore the positions of black cells.

In black-white crosswords the amount of numbers for each row (to the left of canvas) or for each column (on the top of canvas) defines how many groups of black cells (placed sequentially) are placed in the corresponding row or column, and the numbers themselves - the lengths of each group. The list of numbers $inline$[3, 2, 5]$inline$ means that in the corresponding line (row/column) there is three groups placed sequentially, of $inline$3$inline$, $inline$2$inline$ and $inline$5$inline$ black cells. Groups can be located as you wish, not breaking their relative order (since the numbers define their lengths, but the positions have to be guessed), but they should be divided by at least one white cell.


<img align="center" src="https://habrastorage.org/webt/bl/w9/zt/blw9ztswh2gvpy0lfqr76ei_xcq.png" />
_A correct example_

In colored crosswords each group has its color - any color except white, which is reserved for background. Neighboring groups of _different_ colors can be placed back to back, but for the _same_ color the division by at least one white cell is still necessary.

## What is not a japan crossword
Every pixel image can be encoded to a puzzle. But it may be impossible to restore the image back - the resulting puzzle may have more than one solution, either have the single solution but can't be solved in a logical way. Then the remaining cells are to be guessed used <s>quantum computing</s> shamanic technologies.

Such crosswords aren't crosswords, but graphomania. It goes that a correct crossword - such a crossword in which using only a logical way it's possible to reach the single solution.

A "logical way" is a possibility to restore each cell one by one, considering a row/column one by one either their intersection. If there isn't such an opportunity, the amount of possible variants can be huge, way too bigger than people can calculate with a pen and paper.

<img src="https://habrastorage.org/webt/qs/nu/2m/qsnu2mmr0lutke0xzprtow8jv-i.png" />
_A wrong nonogram - the solution is single, but impossible to be found with a deterministic algorithm. "Unsolvable" cells filled with orange. The source of the image is Wikipedia._

This limitation is explained in such a way - in the most common case the japan crossword is an NP-complete problem. However, it isn't such a problem, if there is an algorithm which in every state uniquely calculates what cells are to be definitely guessed. All methods of solving crosswords, used by people (excepting ~~Monte-Carlo~~ trial and error method) are based on this.

The most orthodox crosswords have width and height what are divisible by 5, don't have instantly-calculated lines (what don't have groups at all or whose groups fill the entire line cell by cell definitely). These requirements are not mandatory.

The simplest wrong nonogram:
```Bash
  |1 1|
--+---+
 1|   |
 1|   |
--+---+
```
Encoded pixelart images often are not back-solvable, if they have a "chess order" to imitate gradient. You'll get what it means by searching "pixelart gradient". Pixelart gradients are similar to this fail 2x2 crossword.

<img align="center" src="https://habrastorage.org/webt/oz/mm/sj/ozmmsj7dzmqdn0dwgmspwymmohy.gif" />

## Possible ways to solve
We have the dimensions of the puzzle, description of the colors and all the rows and columns. How to build the image from this:
#### Brute force
Iterate all possible states for each cell and check the satisfability. The complexity of such a solution for black-white crosswords is $inline$O(N*M*{2}^{N*M})$inline$, for colored ones $inline$O(N*M*{colors}^{N*M})$inline$. Like [clown sort](https://en.wikipedia.org/wiki/Bogosort), this solution also can be named clownish. It's good for 5x5 dimension.

#### Backtracking
Iterate all possible ways to place horizontal cell groups. Place a group of cells in a row, check that it didn't break the description of columns cell groups. If it did - try to place the same group one cell further. If we placed successfully - go to the next group. If the group can't be placed anyway - rool back two groups, check again, and so on, while we haven't placed the last group correctly.

#### Separately for each line
This solution is way too better and is truly true. We can analyze each row and each column separately. For each line we try to reveal the maximum of information.

The algorithm reveals information for each cell in a line - it may turn out that it's impossible to set a cell in a certain color, groups then won't describe a right configuration anymore. We can't reveal the whole row, but new information "helps" some columns reveal better, and when we analyze them, they will "help" rows back, and so on, during multiple iterations, while haven't got a single possible color for each cell.

## The truly true solution
#### One line, two colors

Effective guessing of one-line crosswords, in which some cells are already guessed, is a greatly tough task. It appeared in such contests as:

-  *Quarter-Final of ACM ICPC 2006* - [you can try to solve the task yourself](http://acm.timus.ru/problem.aspx?space=1&num=1508&locale=en). Time limit - 1 second, limit of the groups number - 400, the length of the line - 400. It has a pretty strong level of difficulty compared to other tasks on the site.
-  *International Olympiad in Informatics 2016* - [legeng](http://ioinformatics.org/locations/ioi16/contest/day2/paint/paint-USA.pdf), [send solution (warning, Russian-language server)](https://contest.yandex.ru/contest/2647/problems/D/). Time limit - 2 seconds, limit of the groups number - 100, the length of the line - 200'000. These limitations are overkill, but a solution which passes the ACM task limits, takes 80/100 points in this task. The level of this contest is very hard, school students with cruel IQ from around the world train to solve bizarre tasks many years, then pass the competition to take part in this contest (only 4 people should pass to the contest from each country each year), solve it 2 rounds for 5 hours ~~and in case of epic win (bronze medal for 138-240 points of 600, in different years) land on Oxford, then get offers from you-know-what companies in the search department, have a long and happy life.~~

A monochrome one-liner can be solved using different ways, as with $inline$O(N*2^N)$inline$ (brute-forcing all states, check for correctness, detect the cells which have the same colors in all correct states), as well somehow less stupid.

The key idea is to use an analog of backtracking with avoiding redundant calculations. If we somehow placed some groups and currently we are in some cell, then we need to know, if it's possible to place remaining groups starting from the current cell.

<spoiler title="Pseudocode">
```python
def EpicWin(group, cell):                                                          
    if cell >= last_cell: # The win condition                                       
        return group == group_size                                              
                                                                                
    win = False                                                                 
                                                                                
    # Check if we can place the group to the position                                   
    if group < group_size  # There are some groups left                                    
            and CanInsertBlack(cell, len[group])  # Inserting black group        
            and CanInsertWhite(cell + len[group], 1)  # Inserting white cell    
            and EpicWin(group + 1, cell + len[group] + 1):  # Can fill further
        win = True                                                              
        can_place_black[cell .. (cell + len[group] - 1)] = True                 
        can_place_white[cell + len[group]] = True;                              
                                                                                                           
    # Check if we can place a white cell to the position                                
    if CanInsertWhite(cell, 1)                                                     
            and EpicWin(group, cell + 1):                                          
        win = True                                                                 
        can_place_white[cell] = True                                               
                                                                                   
    return win

EpicWin(0, 0)        
```
</spoiler>
Such a method called [dynamic programming](https://en.wikipedia.org/wiki/Dynamic_programming). Note that the pseudocode is pretty simplified and we don't even memoize the calculated values.

The functions `CanInsertBlack/CanInsertWhite` are needed to check if it's possible to place a group of certain size and color in some position. All what they have to do is to check that in the specified segment there are no any "100% white" cell (for the first function) or "100% black" (for the second one). It means they can work with $inline$O(1)$inline$ complexity since it can be done via partial sums:
<spoiler title="CanInsertBlack">
```python
white_counter = [ 0, 0, ..., 0 ]  # An array of length n                              
                                                                                
def PrecalcWhite():                                                             
    for i in range(0, (n-1)):                                                        
        if is_anyway_white[i]:  # 1, if the i-th cell is 100% white                
            white_counter[i]++                                                  
        if i > 0:                                                               
            white_counter[i] += white_counter[i - 1]                            
                                                                                
def CanInsertBlack(cell, len):                                                  
    # Omitting checks of [cell, cell + len - 1] segment bounds correctness              
    ans = white_counter[cell + len - 1]                                         
    if cell > 0:                                                                
        ans -= white_counter[cell - 1]                                          
    # ans contains the number of definitely white cells from \[cell\] to \[cell + len - 1\]
    return ans == 0        
```
</spoiler>
We do the same witchcraft for lines like
```python
can_place_black[cell .. (cell + len[group] - 1)] = True
```
Which imply changing a segment of an array. There we can increment (increase by 1) the element instead of `= True`. If we need to do a lot of summations on segments in an array, and we do not use the array somehow before different summations (it is said that the task "is solved offline"), then instead of the cycle:
```python
# This is called many times
for i in range(L, R + 1):
    array[i]++
```
We can do this:
```python
# This is called many times
array[L]++
array[R + 1]--
# After all summations
for i in range(1, n):
    array[i] += array[i - 1]
```

Thereby the algorithm works for $inline$O(k*n)$inline$ complexity, where $inline$k$inline$ is the number of black cell groups, $inline$n$inline$ is the length of the line. And we pass to the Semi-Final of ACM ICPC or get a bronze medal of IOI. [ACM solution (Java)](https://github.com/Izaron/ACM-ICPC/blob/master/Timus%20Online%20Judge/Volume%206/1508.%20Japanese%20Puzzle.java). [IOI solution (C++)](https://github.com/Izaron/ACM-ICPC/blob/master/International%20Olympiad%20in%20Informatics/IOI%202016/paint.cpp).

#### One line, many colors

When looking at many-colored nonograms, for which we even still don't know the solution, we learn a terrible truth - in fact, each row is affected by every column! It goes clear with an example:
<img align="center" src="https://habrastorage.org/webt/6m/ve/_d/6mve_d8spu5wyqai1osmtppdgr8.png" />
_Source - [Methods of solving Japanese crosswords](http://www.nonograms.org/methods)_

Bicolored nonograms go well without this effect, they don't need to look at orthogonal friends. The left example from the image has three empty cells at the right end, because the configuration breaks when we fill these cells with not white color.

But this connection can be resolved, if we give a number to each cell, where $inline$i$inline$-th bit means if it's allowed to give the cell the $inline$i$inline$-th color. At the start each cell has the number $inline$2^{colors} - 1$inline$. The dynamic programming solution won't change dramatically.

We can take notice on an effect: in the same left example, according to the rows, the most right cell can have either blue or white color.
According to the columns, this cell has either green, or white color.
According to common sense, this cell definitely has only white color. And we continue to calculate the answer.

If consider the 0-th bit "white", the first "blue", the second "green", then the row calculated for the last cell state $inline$011$inline$, the column $inline$101$inline$. It means that really the cell has state $inline$011\&101 = 001$inline$.

<spoiler title="Pseudocode">
```python
source = [...]  # States at the start of the algo
result = [0, 0, ..., 0]  # States at the end
len = [...]  # Lengths of cell groups
clrs = [...]  # Colors of cell groups

def CanInsertColor(color, cell, len):                                                  
    for i in range(cell, cell + len):
        if (source[i] & (1 << color)) == 0:  # Can't place this color in a certain cell
            return False;
    return True

def PlaceColor(color, cell, len):                                                                                                                      
    for i in range(cell, cell + len):                                           
        result[i] |= (1 << color)  # Add the color by the OR operation

def EpicWinExtended(group, cell):                                               
    if cell >= last_cell: # The win condition
        return group == group_size                                              
                                                                                
    win = False                                                                 
                                                                                
    # Can insert the group in the position                                   
    if group < group_size  # Have some groups left to be placed                                    
            and CanInsertColor(clrs[group], cell, len[group])  # Placing black group
            and SequenceCheck()  # If the next group has the same cell, check that
                                 # we can place the white cell after the group
            and EpicWin(group + 1, cell + len[group]):  # Can fill further
        win = True                                                              
        PlaceColor(clrs[group], cell, len[group])                               
        PlaceColor(0, cell + len[group], 1)  # Place the white cell if needed
                                                                                
    # Can insert the white cell in the position                                   
    # White color has bit index 0
    if CanInsertWhite(0, cell, 1)                                               
            and EpicWinExtended(group, cell + 1):                                                                                                      
        win = True                                                              
        PlaceColor(0, cell, 1)                                                  
                                                                                
    return win                      
```
</spoiler>

#### Many lines, many colors
Permanently update the state of all rows and columns, described in the last paragraph, while we have cells with more than one bit. In each iteration, after updating all rows and columns, update the states of all cells in them via mutual AND.

## First results
Let's say that we didn't write the code as woodpeckers, didn't copy objects instead of passing by reference by mistake, didn't mess up things in the code anywhere, weren't inventing bicycles, use `__builtin_popcount` and `__builtin_ctz` for bit operations ([gcc properties](https://gcc.gnu.org/onlinedocs/gcc/Other-Builtins.html)), and did things neat and clean.

Let's look at the running time of the program, which reads the puzzle from a file and solves it completely. It's worth to look at the working station properties, on which all the code has been written and tested:
<spoiler title="Specs of the engine of my 1932 Harley Davidson Model B Classic Motorcycle">
```bash
RAM - 4GB
CPU - AMD E1-2500, capacity 1400MHz
Cache L1 - 128KiB, 1GHz
Cache L2 - 1MiB, 1GHz
Cores - 2, threads - 2
Presented as «weakly productive SoC for compact laptops» mid-2013
Costs 28$
```
</spoiler>

It's clear that such a supercomputer was chosen, because optimizations have bigger effect on this, than on a flying devil-machines.

So, after launching our algorithm on the hardest puzzle (according to nonograms.org), we get not so good outcome - from **47** to **60** seconds (including reading from the file and solving). I need to notice that complexities of puzzles from nonograms.org are calculated well - the same crossword takes the top place of working time, as well as other most hard puzzles are in the top, in all versions of the program.
<spoiler title="Colored nonogram №9596, the hardest">
<img align="center" src="https://habrastorage.org/webt/ar/7n/rx/ar7nrxtbzch1wk8bsihz0puldt8.png" />
</spoiler>

I added to the program an option to launch a benchmark on a folder. To get data I parsed **4683** colored nonograms (out of **10906**) and **1406** black-white (out of **8293**) from nonograms.org (this site is one of the biggest archives of nonograms) by a script and saved it in a format understandable to the program. We can consider these puzzles as random samples, therefore the benchmark shows adequate median and average values. Also the numbers of a couple of dozens of the most "hard" puzzles (and the biggest, and those that have the most colors) I wrote to the script by hands.

<img align="center" src="https://habrastorage.org/webt/ox/xt/nd/oxxtnd1jdlth72q0zqnj3ic1kew.png" />

## Optimization
Here I show optimization methods, which have been applied (1)while writing the entire algorithm, (2)to pressure the running time down from half a minute to fractions of a second, (3)possible methods which may be useful in the future.
#### While writing the algorithm
- Special functions for bit operations, in our case `__builtin_popcount` to count ones in binary presentation of a number, and `__builtin_ctz` to find the amount of trailing zeros of a number. Those functions may not exist in some compilers. For Windows you can use code like:

<spoiler title="Windows popcount">
```cpp
#ifdef _MSC_VER                                                                                                
#  include <intrin.h>                                                                                          
#  define __builtin_popcount __popcnt                                                                          
#endif   
```
</spoiler>
- Arrays organization - the smaller dimension stands at the beginning. For example, it's better to use an array which looks like [2][3][500][1024], than [1024][500][3][2].
- The most important - adequacy of the code and avoiding to create redundant problems for CPU to calculate.

#### What decreases the execution time
- -O2 flag when compiling. (-O3 isn't always good, but it's not the theme of this article)
- To avoid "solving" already solved row/column again, store in a std::vector/array flags for each line, mark a linewhen solved, and don't allow to go further, when iterating the lines and meeting already solved one.
- Специфика многоповторного решения задачи на динамику предполагает, что специальный массив, который содержит флаги, помечающие уже "вычисленные" куски задачи, следует обнулять каждый раз при новом решении. В нашем случае это двумерный массив/вектор, где первое измерение - количество групп, второе - текущая клетка (см. псевдокод EpicWin сверху, где этого массива нет, но идея ясна). Вместо обнуления можно сделать так - пусть у нас будет переменная-"таймер", а массив состоит из чисел, показывающих последнее "время", когда этот кусок пересчитывался в последний раз. При запуске новой задачи "таймер" увеличивается на 1. Вместо проверки булевого флага следует проверять равенство элемента массива и "таймера". Это эффективно особенно в тех случаях, когда далеко не все возможные состояния покрываются алгоритмом (а значит, обнуление массива "считали ли мы уже это" занимает здоровый кусок времени).
- Change simple same-type operations (loops with assignments, etc.) to their analogs in STL or other more adequate things. Sometimes it works faster that an invented wheel.
- `std::vector<bool>` in C++ saves all boolean values as bits in an integer type to save memory. It works slightly slower on access, comparing with a regular value by an address. If a program accesses extremely often, changing bool to an integer type can affect program's perfomance in a good direction.
- Other weak places can be found via profilers. I use Valgrind, its dumps are easy to explore in kCachegrind. Profilers are embedded to many IDEs.

These changes were enough to get this benchmark outcome:
<spoiler title="Colored nonograms">
```Bash
izaron@izaron:~/nonograms/build$ ./nonograms_solver -x ../../nonogram/source2/ -e
[2018-08-04 22:57:40.432] [Nonograms] [info] Starting a benchmark...
[2018-08-04 22:58:03.820] [Nonograms] [info] Average time: 0.00497556, Median time: 0.00302781, Max time: 0.215925
[2018-08-04 22:58:03.820] [Nonograms] [info] Top 10 heaviest nonograms:
[2018-08-04 22:58:03.820] [Nonograms] [info] 0.215925 seconds, file 9596
[2018-08-04 22:58:03.820] [Nonograms] [info] 0.164579 seconds, file 4462
[2018-08-04 22:58:03.820] [Nonograms] [info] 0.105828 seconds, file 15831
[2018-08-04 22:58:03.820] [Nonograms] [info] 0.08827 seconds, file 18353
[2018-08-04 22:58:03.820] [Nonograms] [info] 0.0874717 seconds, file 10590
[2018-08-04 22:58:03.820] [Nonograms] [info] 0.0831248 seconds, file 4649
[2018-08-04 22:58:03.820] [Nonograms] [info] 0.0782811 seconds, file 11922
[2018-08-04 22:58:03.820] [Nonograms] [info] 0.0734325 seconds, file 4655
[2018-08-04 22:58:03.820] [Nonograms] [info] 0.071352 seconds, file 10828
[2018-08-04 22:58:03.820] [Nonograms] [info] 0.0708263 seconds, file 11824
```
</spoiler>
<spoiler title="Black-white nonograms">
```Bash
izaron@izaron:~/nonograms/build$ ./nonograms_solver -x ../../nonogram/source/ -e -b
[2018-08-04 22:59:56.308] [Nonograms] [info] Starting a benchmark...
[2018-08-04 23:00:09.781] [Nonograms] [info] Average time: 0.0095679, Median time: 0.00239274, Max time: 0.607341
[2018-08-04 23:00:09.781] [Nonograms] [info] Top 10 heaviest nonograms:
[2018-08-04 23:00:09.781] [Nonograms] [info] 0.607341 seconds, file 5126
[2018-08-04 23:00:09.781] [Nonograms] [info] 0.458932 seconds, file 19510
[2018-08-04 23:00:09.781] [Nonograms] [info] 0.452245 seconds, file 5114
[2018-08-04 23:00:09.781] [Nonograms] [info] 0.19975 seconds, file 18627
[2018-08-04 23:00:09.781] [Nonograms] [info] 0.163028 seconds, file 5876
[2018-08-04 23:00:09.781] [Nonograms] [info] 0.161675 seconds, file 17403
[2018-08-04 23:00:09.781] [Nonograms] [info] 0.153693 seconds, file 12771
[2018-08-04 23:00:09.781] [Nonograms] [info] 0.146731 seconds, file 5178
[2018-08-04 23:00:09.781] [Nonograms] [info] 0.142732 seconds, file 18244
[2018-08-04 23:00:09.781] [Nonograms] [info] 0.136131 seconds, file 19385
```
</spoiler>
Noticeable that black-white crosswords are "harder" than colored ones in average. It confirms observations of game lovers who also believe that "colored" puzzles are solved easily in average.

In this way, without radical changes (like rewritting all the code on C or pieces of assembler code with a fastcall convention and omitting a frame pointer) it's possible to get high perfomance, notice, on a strongly weak computer. Optimizations may have the Pareto principle - a small optimization may drastically affect the program, because this piece of code is critical and is called very often.

#### Further optimizations
Following methods can drastically improve the perfomance of a program. Some of them require special conditions and don't work in all cases.

- Rewritting the code to C-style and other 1972 stuff. Change ifstream to a C analog, vectors to arrays, learn all compiler options and struggle for each CPU's tick.
- Paralleling. For example, there is a piece in the code, where the rows and the columns are updated one by one:

<spoiler title="bool Puzzle::UpdateState(...)">
```cpp
    ...
    if (!UpdateGroupsState(solver, dead_rows, row_groups, row_masks)) {                                                                               
        return false;                                                              
    }                                                                              
    if (!UpdateGroupsState(solver, dead_cols, col_groups, col_masks)) {           
        return false;                                                              
    }                                                                             
    return true;
```
</spoiler>
These functions are independent one of each other and don't have mutual memory to access instead of *solver* variable (of type OneLineSolver), so it's possible to create two objects of OneLineSolver and start two threads in the function. The effect is - in colored crosswords "the hardest" is solved two times faster, in black-white the same is solved 1/3 faster, but the average time increased due to relatively expensiveness of creating threads.

But really I'd not suggest to do it right how is described here - firstly, creating threads is a expensive operation, it's not worth it to create threads for microseconds tasks, and secondly, using some combination of command line arguments in my program, these threads potentially can access some outer memory simultaneously, for example, when creating solution images - this should be taken into account and secured.

If the tasks was serious and I had many input data and multi-core machines, I would go further - it's possible to create some permanent threads, and each will have its own OneLineSolver object, and there would be a thread-safe structure to rule distribution of work, which gives a reference to current row/column to solver. Threads after solving their tasks ask the structure for the next piece of work. Some puzzles may start to being solved before the last puzzle is solved to the end - while some threads upsolving the puzzle, the rest go forward. Paralleling could be done via graphics processor unit with CUDA - there is a plenty of variants.

- Using classical data structures. Notice - when I showed the pseudocode of the solution of colored nonograms, the functions `CanInsertColor` and `PlaceColor` don't work for $inline$O(1)$inline$, unlike black-white ones. In the program it looks as:

<spoiler title="CanPlaceColor и SetPlaceColor">
```cpp
bool OneLineSolver::CanPlaceColor(const vector<int>& cells, int color,          
        int lbound, int rbound) {                                               
    // Went out of the border                                                   
    if (rbound >= cells.size()) {                                               
        return false;                                                           
    }                                                                           
                                                                                
    // We can paint a block of cells with a certain color if and only if it is  
    // possible for all cells to have this color (that means, if every cell     
    // from the block has color-th bit set to 1)                                                                                                       
    int mask = 1 << color;                                                                                                                             
    for (int i = lbound; i <= rbound; ++i) {                                    
        if (!(cells[i] & mask)) {                                               
            return false;                                                       
        }                                                                       
    }                                                                           
    return true;
}

void OneLineSolver::SetPlaceColor(int color, int lbound, int rbound) {                                                                                 
    // Every cell from the block now can have this color                        
    for (int i = lbound; i <= rbound; ++i) {                                    
        result_cells_[i] |= (1 << color);                                       
    }                                                                           
} 
```
</spoiler>
It work with a linear complexity, for $inline$O(N)$inline$ (I'll explain this later)

Let's look how to obtain a better complexity. Take `CanPlaceColor`. This function checks that for each number in the segment $inline$[lbound, rbound]$inline$ the $inline$color$inline$-th bit is set to 1. As well we can take the $inline$AND$inline$ of **all** numbers of the segment and check the $inline$color$inline$-th bit. Using the fact that the $inline$AND$inline$ operation is [commutative](https://en.wikipedia.org/wiki/Commutative_property) like sum, min/max, multiplication or the $inline$XOR$inline$ operation, to calculate the $inline$AND$inline$ of a segment quickly we can use any data structure which is good with commutative operations on segments. It includes:
1.  **SQRT-Decomposition**. Precalc for $inline$O(\sqrt{N})$inline$, query for $inline$O(\sqrt{N})$inline$.
2.  **Segment Tree**. Precalc for $inline$O(N\log{N})$inline$, query for $inline$O(\log{N})$inline$.
3.  **Sparse Table**. Precalc for $inline$O(N\log{N})$inline$, query for $inline$O(1)$inline$.

It's a pity that especially strong witchcrafts like the Farach-Colton and Bender algorithm (precalc for $inline$O(N)$inline$, query for $inline$O(1)$inline$) are useless for the task, because while learning the papers it turns out that the algorithm is created only for such operations $inline$\varphi$inline$, that $inline$\varphi(\alpha, \beta) \in \{\alpha, \beta\}$inline$}, it means, the result of an operator should be one of the arguments.

Now take `SetPlaceColor`. There it's needed to do an operation on segments of an array. It could be done via SQRT-Decomposition or Segment Tree with lazy propagation (aka "with pushes"). And for both of the functions simultaneously it's possible to use cool data structure - Implicit Treap with updating and querying for logarithm. 

Also we can expand the algorithm for black-white puzzles - use partial sums for all colors.

Итак, возникает вопрос - *почему* мы не используем все это богатство комплюктерн саенс, а делаем за линию? На это есть несколько ответов:
1.  Меньшая сложность вычислений не значит меньшее время работы. Алгоритм за $inline$\log$inline$ может потребовать различных преподсчетов, выделений памяти, другой тряски ресурсов - у этого алгоритма может быть довольно высокая константа (не в смысле magic number в нейроночках, а аффект по времени работы). Очевидно, что если $inline$N=10^{5}$inline$, то условный алгоритм за $inline$O(N^2)$inline$ будет работать условные 10 секунд, а условный алгоритм $inline$O(N\log{N})$inline$ за условные 0.150 секунд, но все может поменяться при достаточно маленьких $inline$N$inline$, особенно когда таких вычислений много. Еще непонятнее, когда сложности очень похожие и одной сложности сложно перебить другую сложность (сложные приколы): $inline$O(N\sqrt{N})$inline$ versus $inline$O(N\log^2{N})$inline$. В нашей задаче $inline$N$inline$ (длина ряда) очень маленькое - в среднем около 15-30.
2.  Запросов может оказаться достаточно мало, чтобы предпосчеты были бесполезными и жрали ресурсы просто так.

То есть объяснение простое - оба этих пункта выполняются и вставка этих чудес программерской мысли вместо тупого алгоритма либо очень слабо оптимизирует программу, либо только увеличивает время работы из-за специфики вычислений - очень маленького $inline$N$inline$ и не такого большого количества запросов, чтобы они грузили процессор. Факт про запросы доказывает то, что по мнению профайлера те функции занимают ~25% и ~11% времени соответственно, то есть довольно мало для такого потенциально слабого места программы. Даже если у нас из-за этого возникает большая оценка сложности алгоритма, стоит понимать, что в таких типах задач это оценка сверху, а реальное время работы на рандомном кроссворде всегда премного ниже.

Но не стоит забывать про структуры данных - они могут пригодиться в других случаях, так что я включил их в список оптимизаций. Переходим к следующему по списку.

- Правки алгоритма. Может оказаться так, что в среднем алгоритм на этих входных данных очень неплохо начинает работать, если поменять что-то неочевидное. В нашем случае этим может быть такое: логично же предположить, что если мы успешно обновили данные в строке, то обновленные в нем клетки скорее всего "триггерят" соответствующие столбцы? Значит, лучше эти столбцы обновить быстрее всех, сразу после этой строки! То есть образуется очередь из таких подзадач. Я не пробовал именно такой алгоритм, может это реально быстрее на нашем датасете.
- Внезапные изменения техзадания (окажется, что поступают кроссворды по 1337 цветов или размером 1000x1000) тоже требуют оптимизации. Для большого количество цветов можно использовать быстрый [std::bitset](https://en.cppreference.com/w/cpp/utility/bitset), для размера - те же структуры данных, и так далее.

В общем, вот такие прикольные оптимизации. "Пихание" бедного алгоритма в зависимости от условий это весело и познавательно. Можно узнать про разные крутые вещи, как встроенное декартово дерево по неявному ключу в C++ (это [rope](https://habr.com/post/144736/), но велосипедная писанина практически всегда работает быстрее), [особые встроенные сорта деревьев](http://codeforces.com/blog/entry/11080) и [спрятанный hashtable](http://codeforces.com/topic/61081/), работающий в 3-6 раза быстрее по вставке/удалению и в 4-10 раз по записи/чтению, чем unordered_map. Не говоря уже про различные нестандартные мазафаки со стороны, например из boost.

## ROFL Omitting Format Language
<img align="center" src="https://habrastorage.org/webt/rc/vp/jm/rcvpjmbinhow4aakwya-xgt3rm4.png" />
Вдохновившись гениями давно минувших дней и их мыслительными продуктами, в частности совершенно новыми алгоритмами архивации и операционками с нескучными обоями, я также придумал принципиально новый формат картинок, основанный на японских кроссвордах и эффекте Даннинга-Крюгера.

ROFL - рекурсивный акроним, прямо как "GNU's Not Unix". Собственно, смысл формата в том, что картинка кодируется в виде японского кроссворда, а редактор, чтобы прочитать ее, должен решить этот кроссворд. Отсюда и слово Omitting в названии - формат как бы скрывает истинное положение дел в картинке (что, кстати, может быть полезным в криптографии: можно передавать японские кроссворды с зашифрованными в нем паролями - все хакеры повесятся).

Лучше, если формат был бы похож на [Matroska](https://matroska.org/technical/specs/index.html) - в начале файла 4 байта [52][4F][46][4C], затем в трех байтах размер картинки и количество цветов, потом описание цветов, потом цвета, каждый по 3 байта, и потом описание всех групп - длина, количество клеток и цвет.

Формат свободный, лицензия MIT, финансовые отчисления добровольные - мне на кофе, как автору.

## Исходники
Исходники программы лежат на [GitHub](https://github.com/Izaron/Nonograms). У программы есть множество возможных аргументов, генерация кроссворда из картинки, генерация картинок из кроссворда (кстати, почти все картинки в статье сгенерированы через этот код). Дополнительными библиотеками были [Magick++](https://github.com/ImageMagick/ImageMagick) и [args](https://github.com/Taywee/args).

В репозитории картинки для кроссворда я добавил несколько [примеров](https://github.com/Izaron/Nonograms/tree/master/puzzles), взятых из интернета (они не являются частью проекта). Спарсенные тысячи с nonograms.ru никуда не выкладывал и не буду, чтобы их не потырили, потому что они могут быть защищены правами и тырятеля могут ожидать интересные последствия.

Особую благодарность хочу выразить автору nonograms.ru Чугунному К.А. @KyberPrizrak, за создание прекрасного, лучшего и одного из крупнейших в интернете сайта по нонограммам, и за разрешение использовать материалы для статьи! Все примеры в этой статье я взял с nonograms.ru, вот [список](https://gist.github.com/Izaron/518f6b8659d1385ce7047ae477881e8e) источников.

[Исходники](https://github.com/Izaron/Nonograms).

<img align="center" src="https://habrastorage.org/webt/es/zg/il/eszgil8btd-jjgaszlinfz2oodm.png" />
