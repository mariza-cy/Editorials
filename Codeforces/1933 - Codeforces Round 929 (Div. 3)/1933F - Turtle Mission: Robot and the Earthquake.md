# 1933F - Turtle Mission: Robot and the Earthquake
You can view the problem statement and submit [here](https://codeforces.com/contest/1933/problem/F)

## Step 1
In order to find a path, we need to know what moves we can make from a certain cell at a certain time. However, we have $n⋅m$ cells and $m$ possible positions for the rocks (since every $n$ seconds they will return to their original position). That means we need to find a solution that only needs one of these factors.

The fact that the columns are cyclic allows us to do just that. Before we reach the cell $(n,m)$ we don't actually need to know the actual coordinates of RT - just it's position ralative to the rocks, so we can keep track of it's position in the array $a$, moving it one cell downwards every second instead of moving the rocks upwards.

This means that the operations will change, too:
 - When moving upwards, it will stay on the same cell (as the rocks will also move one cell upwands).
 - When moving downwards, it will move two cells cyclically downwards, i.e. from cell $(i,j)$ to cell $((i+2) \bmod n,j)$.
 - When moving to the right, it will move one cell to the right and one cell cyclically downwards, i.e. from cell $(i,j)$ to cell $((i+1) \bmod n,j+1)$.
