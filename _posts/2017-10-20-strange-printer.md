---
layout: post
title:  "Strange Printer"
date:   2017-10-20
categories: leetcode dynamic-programming
---

[Problem][problem]

The "run" of a letter doesn't matter, so the number of turns to print "aaaabaaaa"
is the same as the number of turns needed to print "aba". Once this transformation
is done, consecutive characters in the string are non-identical.

If the string is of the form "aXY...Za", then the min. number of turns needed is
the same as those required for "aXY...Z". Otherwise, if its of the form "aX1 X2..b",
look at the splits "a"/"X1 X2..b" , "a X1"/"X2 ...b", ... "a X1 X2..."/"b" and take
the one which gives the minimum.

__Order of solving__ : To find optimum for a string of length N, the optimum for
all substrings <= N-1 is required. Solve in order of substring length.

{% highlight c++ %}
class Solution {
public:
    void trimRuns(string s, string &res) {
        res.clear();
        for(auto &c: s) {
            if( c != res.back() ) {
                res.push_back(c);
            }
        }
    }
    int strangePrinter(string s) {
        if(s.size() ==  0) return 0;
        string condensed;
        trimRuns(s, condensed);

        vector<vector<int>> opt(condensed.size(), vector<int>(condensed.size(), 0));
        int N = condensed.size();
        for(int len = 1; len <= N; len++) {
            for(int i = 0; i + len - 1 < N; i++) {
                int j = i + len - 1;
                if(len == 1) {
                    opt[i][j] = 1;
                }
                else if(len == 2) {
                    opt[i][j] = 2 - (condensed[i]==condensed[j]);
                }
                else {
                    opt[i][j] = INT_MAX;
                    if(condensed[i] == condensed[j]) {
                        opt[i][j] = opt[i][j-1];
                    }
                    for(int k = i + 1; k <= j; k++) {
                        opt[i][j] = min(opt[i][j], opt[i][k-1]+opt[k][j]);
                    }
                }
            }
        }
        return opt[0][N-1];
    }
};
{% endhighlight %}

[problem]: https://leetcode.com/problems/strange-printer/description/
