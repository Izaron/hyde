---
layout: post
date: '2017-07-14'
title: Competitive programming and cyberpunk
language: en
---

![](https://media.giphy.com/media/NKEt9elQ5cR68/giphy.gif)

_In the future world many things changed, but algoritms are still popular_

_The theme of the post is competitive programming, cyberpunk is contained in gifs. Get over here._

I've been doing competitive programming for some years, and have never regretted it. Competivive programming is pretty cool. If you don't believe in it, I can prove it. Just let's go!

<!--break-->

## Competitive programming is interesting

If someone does CP seriously, he learns a lot of interesting things. Data structures, graph theory, string algorithms, as well as dynamic programming, game theory, combinatorics... The list is pretty big.

![](https://static1.squarespace.com/static/543593e4e4b0bf8b316933e3/t/55224459e4b0ab9c8e348de9/1428309084880/firstattempt.jpg)

_And he knows why this code is as slow as an old phone with Android 2.3.2_

## Competitive programming helps understand the code better and keeps the brains turned on

![](https://s-media-cache-ak0.pinimg.com/originals/be/82/8c/be828c40bb5a13d25c1e2579514ed6f3.gif)

_Something went wrong, and I can't handle it for two hours_

#### Rubber Duck on steroids

Competitions have tight time limits. If you didn't debug your code in time, you lost valuable time/points, or got bigger penalty. The brains turned on CP will not stretch out time up to hour to find a bug.

#### You need to understand how it all works

A huge number of programmers have no idea how really these std::map/TreeMap and std::set/TreeSet work, not even speaking about other things. Some people treat it like black magic. Because of this, they allow themselves to write code which runs much slower than it could.

![](https://68.media.tumblr.com/001f6b1a313463ac3a0b4cbaf21a98cb/tumblr_nvi1n5aODQ1udwzyuo1_500.gif)

#### Hey, what is an array shift?

Well, I'm not going to go far to find an example. Imagine what we have an object of std::vector/ArrayList of 100'000 elements. We have to write a piece of code that sometimes deletes the first or the last element (suppose we don't want to clear the list instantly, only one by one). If you're not into data structures, you'd think that removing the first and the last element is the same. Let's remove the first element every time. And here is a surprise - it turns out that this code works a few seconds! If we delete the last element, it works almost instantly. A slightly experienced programmer understands immediately why it happens. (Though, if we use double-linked list, it works fast in both cases).

#### To be continued

There are other side effects, like the ability to write code *fast*, but it isn't the subject of this post anyway.

## Competitive programming is prestigious.
![](https://static.tumblr.com/0c4c1044875d8d4b73b616699b4f4c37/35ras8e/dfin2bmo6/tumblr_static_bg_cyberjam_night.png)
                                                                                                                                                                                        
CP has never been something *underground*. There are a lot of well-known contests with interesting tasks are held every year. The most famous of them is ACM ICPC for university students and IOI for high school students. In big contests like Google Code Jam, Facebook Hacker Cup, top contestants get paid trip to the "onsite" - the final round. Some strong companies hold domestic contests, in Russia there are VK Cup, Yandex.Algorithms and Russian Code Cup, they're all made by the country finest IT companies. Of course, everyone can take part in regular online contests on sites like TopCoder or Codeforces. 

Contests systems are different, but they have the same point - it's necessary to solve tasks fast and without mistakes.

## What does a typical task look like?

![](https://68.media.tumblr.com/6e5bf2cc7f2574aed534af35700ac94e/tumblr_opgyaiBTJo1v63h88o1_500.gif)

#### Legends and sagas

What is a _task_ (Sometimes called _problem_)? We get a bunch of numbers and strings from the input stream or a file, and we have to write a program that reads the data, *somehow* works with it and writes other data (the answer) to the output stream/a file. The description of the task is named "legend". Usually a few examples (1-3) are suggested after the legend. The legend creates a mathematical model of the task, that is, it says, "imagine that the next $N$ numbers are the elements of the array $A$, and the next $M$ numbers are the array $B$".

![](http://i.imgur.com/IbQ84iF.gif)

#### Recommended to solve

When Google Code Jam engineers were giving a talk about the competitions, they considered a pretty good problem from one of their round - [Evaluation](https://code.google.com/codejam/contest/6274486/dashboard#s=p2).

An unordered list of assignment statements is given. The task is to determine if it's possible to put the statements in an order in which all variables can be evaluated correctly, without using another undefined variable.

For example, if these statements are given:

```
a=f(b,c)
b=g()
c=h()
```

The answer is "possible". This is a valid order:

```
b=g()
c=h()
a=f(b,c)
```

Here all the variables calculated correctly, and to calculate $a$ we use already defined $b$ and $c$. Therefore, the order below is invalid, because we're going to evaluate variable $a$ when $c$ is undefined:

```
b=g()
a=f(b,c)
c=h()
```

![](http://i.imgur.com/dbidMbl.gif)

#### Scoring

There are up to 20 tests in every file. If we're able to find the answer for one hundred statements ($n \leq 100$), we get $12$ points. If there are up to one thousand statements ($n \leq 1000$), we get $12+15=27$ points. So, if we come up with a solution which isn't optimal enough, we may get partial score. It's assumed that $12$ points is given for a [$O(n^3)$](https://en.wikipedia.org/wiki/Big_O_notation) solution, and $27$ points is given for a [$O(n^2)$](https://en.wikipedia.org/wiki/Big_O_notation) solution. Let's write better solution at once.

#### Hack!

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    int t;
    cin >> t;
    for (int i = 0; i < t; i++) {
        cout << "Case #" << i + 1 << ": ";
        cerr << "Test #" << i + 1 << endl;
        cout << (solve() ? "GOOD" : "BAD") << endl;
    }
}
```

`solve()` reads the test and calculates the answer.

It's better to write a helper function for to split strings, so `val=func(argx,argy)` transforms to `{"val", "func", "argx", "argy"}`:

```cpp
vector<string> crop(const string& s) {
    vector<string> vec;
    string cur = "";

    for (size_t i = 0; i < s.size(); i++) {
        if (s[i] < 'a' || s[i] > 'z') {
            // If s[i] is not a lowercase English letter
            if (!cur.empty()) {
                vec.push_back(cur);
                cur = "";
            }
        } else {
            cur += s[i];
        }
    }

    return vec;
}
```

#### Graph theory

How can we solve it? It's time for the popular and legendary **graph theory**.

![](https://s-media-cache-ak0.pinimg.com/originals/37/71/58/377158fca2b73b083fd0aa4a4f703930.gif)

Let's represent our input data as a graph! Every variable is a **vertex** now, and we create a directed **edge** from each vertex to every dependent variable (other vertices).

#### Building the model

Our example looks this way - there are three vertices $a$, $b$ and $c$, and two edges, from $a$ to the other vertices. We can represent any test in the same way. Intuitively, the task can be solved by this method - we look at the graph and determine if there is a vertex **without** edges to other undefined vertices. If such a vertex was found, then we mark its variable as "defined" and go further.

![](http://i.imgur.com/noiejkr.gif)

#### Finding the answer

If we have undefined variables, but can't find any vertex without edges to undefined variables, it means that we have a **cycle**, that is unsolvable dependencies. Somewhere $x$ depends on $y$, $y$ depends on $z$, and $z$ depends on $x$, like this. Of course, if we defined all the variables, then the solution exists.

#### Finishing the code

```cpp
bool solve() {
    int n;
    cin >> n;

    vector<string> source(n);  // input lines
    map<string, int> indices;   // map "variable name" -> "index"
    vector<vector<string>> deps_raw(n);   // list of dependencies (as strings)
    vector<set<int>> deps(n);   // list of dependencies (as indices)

    for (int i = 0; i < n; i++) {
        string s;
        cin >> s;

        source[i] = s;

        vector<string> variables = crop(s);
        string name = variables[0];
        indices[name] = i;

        vector<string> var_names;
        for (size_t z = 2; z < variables.size(); z++)
            var_names.push_back(variables[z]);
        deps_raw[i] = var_names;
    }

    for (int i = 0; i < n; i++) {
        for (auto dep : deps_raw[i]) {
            if (!indices.count(dep))
                return false;
        }
    }

    for (int i = 0; i < n; i++) {
        set<int> deps_set;
        for (auto dep : deps_raw[i])
            deps_set.insert(indices[dep]);
        deps[i] = deps_set;
    }

    // main loop here!
    vector<bool> used(n);
    for (int count = 0; count < n; count++) {
        int v = -1;
        for (int i = 0; i < n; i++) {
            if (used[i])
                continue;
            if (deps[i].empty()) {
                v = i;
                break;
            }
        }
        if (v == -1)
            return false;

        cerr << count << "-th line: " << source[v] << endl;

        used[v] = true;
        for (int i = 0; i < n; i++) {
            if (!used[i]) {
                if (deps[i].count(v))
                    deps[i].erase(v);
            }
        }
    }

    return true;
}
```

[Full source code](https://gist.github.com/Izaron/43c521e5597550029de928c5fb8f3ef9).

![](https://media.giphy.com/media/IojlRkMFIgZVu/giphy.gif)

Compile in your IDE or using the console `g++ -std=c++11 main.cpp -o main`, launch the program with `./main <input_file >output_file`.

Output in the console from the author's example:

```
Test #1
0-th line: b=g()
1-th line: c=h()
2-th line: a=f(b,c)
Test #2
Test #3
Test #4
0-th line: x=f()
1-th line: y=g(x,x)
```

![](http://4.bp.blogspot.com/-3pgg2B2VrG0/U7ndQIaSTPI/AAAAAAAAAIM/jU3yDsl_wDk/s1600/title.gif)

That's all, thanks for reading! Stay tuned...

:wq
