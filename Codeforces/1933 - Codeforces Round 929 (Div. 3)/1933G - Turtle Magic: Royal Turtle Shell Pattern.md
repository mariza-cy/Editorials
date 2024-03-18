# 1933G - Turtle Magic: Royal Turtle Shell Pattern
You can view the problem statement and submit [here](https://codeforces.com/contest/1933/problem/G)

### Step 1
Let's start by trying different ways to place the cookies on a 3x3 grid. To do that, we will try different combinations of shapes on the top left 2x2 grid and filling the rest based on the statement's restrictions (since there can't be three consecutive cells that contain the same shape of cookies, if we have two cookies of the same shape in a row, column or diagonal there must be a cookie of the other shape in the 3<sup>rd</sup> one).

First of all, we notice that we can't place three cookies of the same shape in a way that they form an L shape in the corner (no matter what shape the other cookie of the 2x2 grid is):
![](https://github.com/mariza-cy/Editorials/blob/main/Codeforces/1933%20-%20Codeforces%20Round%20929%20(Div.%203)/Corner.gif)

This a new restriction that we can use to fill our grid the way we used the rule about three consecutive cells. Now we have 4 rules for a 3x3 grid (as if 3 cookies form a line, they must be consecutive): 
 - There can't be 3 :purple_square:s in the same row, column, or diagonal.
 - Similarly, there can't be 3 :orange_circle:s in the same row, column, or diagonal.
 - There can't be 3 :purple_square:s that form a "corner". This means that if there are 3 :purple_square:s in a 2x2 section of the grid, the 4<sup>th</sup> one must be a :orange_circle: (see the image below).
 - The same is true for :orange_circle:s - if 3 of them are in a 2x2 section, the 4<sup>th</sup> must be a :purple_square:.

![](https://github.com/mariza-cy/Editorials/blob/main/Codeforces/1933%20-%20Codeforces%20Round%20929%20(Div.%203)/Corner%20usage.gif)
