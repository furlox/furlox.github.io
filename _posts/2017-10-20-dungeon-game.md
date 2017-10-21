---
layout: post
title:  "Dungeon Game"
date:   2017-10-20 11:55 AM
categories: leetcode dynamic-programming
---

<a href="https://leetcode.com/problems/dungeon-game/description/" target="_blank">Problem</a>

To figure out the order of filling in the table, understanding
the sub-problems with a pencil helps. For this problem, the knight can move down or
right, and initially, the bottom-right square (princess cell) value is known. Whenever
a cell must be filled in, need to know value of the neighbour to the immediate left and
down. So, start filling in column-wise, right-to-left; filling in each column bottom-to-top.


{% highlight c++ %}
class Solution {
public:
    int calculateMinimumHP(vector<vector<int>>& dungeon) {
        int grid_depth = dungeon.size();
        int grid_width = dungeon[0].size();
        for(int col = grid_width-1; col >= 0; col--) {
            for(int row = grid_depth-1; row >= 0; row--) {
                int cell_bonus = dungeon[row][col];
                int hp_left = (col == grid_width-1) ? INT_MAX : dungeon[row][col+1];
                int hp_down = (row == grid_depth-1) ? INT_MAX : dungeon[row+1][col];
                if (col==grid_width-1  && row==grid_depth-1) {
                    dungeon[row][col] = 1 - cell_bonus;
                }
                else {
                    dungeon[row][col] = min(hp_left, hp_down) - cell_bonus;
                }
                if(dungeon[row][col] <= 0) {
                    dungeon[row][col] = 1;
                }
            }
        }
        return dungeon[0][0];
    }
};
{% endhighlight %}

[problem]: https://leetcode.com/problems/dungeon-game/description/
