---
layout: post
title: Competitive programming and cyberpunk
language: en
---

![](https://media.giphy.com/media/NKEt9elQ5cR68/giphy.gif)

_In the world of the future many things have changed, but algoritms are still popular_

I've been doing competitive programming for some years, and have never regretted about this. Competivive programming is cool! If you don't believe me, I'm going to prove it. Just let's go!

<!--break-->

## Competitive programming is interesting

Really, if someone does CP seriously, he learns a lot of interesting things. Data structures, graph theory, string algorithms, as well as dynamic programming, game theory, combinatorics... The list is pretty big.

![](https://static1.squarespace.com/static/543593e4e4b0bf8b316933e3/t/55224459e4b0ab9c8e348de9/1428309084880/firstattempt.jpg)

_And he understands, why this code is as slow as a phone with Android 2.3.2_

## Competitive programming helps understand the code more and keeps the brains turned on

![](https://camo.githubusercontent.com/93f40fae2493e5b8290d76256c315adffaf8209d/68747470733a2f2f732d6d656469612d63616368652d616b302e70696e696d672e636f6d2f6f726967696e616c732f62652f38322f38632f62653832386334306262356131336432356331653235373935313465643666332e676966)

_Something has gone wrong, and I can't deal with it two hours already_

#### Rubber Duck on steroids

Competitions have tight time limits. If you didn't debug your code in time, you lost valuable time/points, or got bigger penalty. The brains turned on CP will not stretch out time up to hour to find a bug.

#### Seriously, you need to understand how all it works

A huge number of programmers don't give a heck how really these std::map/TreeMap and std::set/TreeSet works, not speaking about other things. They treat it like black magic. Because of this, they allow themselves to have rubbish in the form of the code that runs much slower than it could works.

![](https://68.media.tumblr.com/001f6b1a313463ac3a0b4cbaf21a98cb/tumblr_nvi1n5aODQ1udwzyuo1_500.gif)

#### Hey, what is an array shift?

Well, I'm not going to go far for an example. Imagine what we have some std::vector/ArrayList with the 100000 elements. We have to write code that looks at the extreme element (either first or last element in the list), and then deletes it. It is logical and more comfortably to write the code that deletes the first element every time. And here is a surprise - it turns out that this code works a few seconds! If we would delete the last element, it would works in a flash. An experienced programmer understands at once, why it is happens. (Though, if we would use the double-linked list, it would works instantly in both cases).

#### To be continued

There are other side effects, like an ability to write the code *fast*, but anyway it isn't the topic of this post.

## Competitive programming is prestigious.
![](https://static.tumblr.com/0c4c1044875d8d4b73b616699b4f4c37/35ras8e/dfin2bmo6/tumblr_static_bg_cyberjam_night.png)
                                                                                                                                                                                        
CP has never been something *underground*. There are a lot of well-known contests held with interesting tasks. The most famous of them are ACM ICPC - for the university students, and IOI - for high school students. Also such the big contests like Google Code Jam, Facebook Hacker Cup are held, where the top contestants have paid trip to the "onsite" - the final round. In particular, in Russia there are VK Cup, Yandex.Algorithm, Russian Code Cup (from MailRu), all of them are from top companies here. And of course, regular online contests are exist on the sites like TopCoder and Codeforces. 

Contests systems are different, but they have the same point - it's necessary to solve tasks.

## What a typical task looks like

![](https://68.media.tumblr.com/6e5bf2cc7f2574aed534af35700ac94e/tumblr_opgyaiBTJo1v63h88o1_500.gif)

#### Legends and sagas

What is a _problem_? We get a bunch of numbers and string from the input stream or a file, and we have to write a program that reads this data, *somehow* works with them and outputs other data (an answer) to the output stream/the file. The description of the task is named "legend". 1-3 examples are usually suggested after the legend. A legend creates the task model, that is, it says, "imagine that the next N numbers is the elements of an array A, and the next M numbers are an array B".

![](http://i.imgur.com/IbQ84iF.gif)

#### Recommended to solve

When Google Code Jam engineers were giving a talk about the CP, they considered quite good problem from one of their round - [Evaluation](https://code.google.com/codejam/contest/6274486/dashboard#s=p2).

An unordered list of assignment statements is given, the task is to determine if it is possible to put the statements in some order in which all variables can be evaluated.

For example, these statements are given:

```
a=f(b,c)
b=g()
c=h()
```

The answer is "possible". This is one valid order:

```
b=g()
c=h()
a=f(b,c)
```

But this order is invalid, because we're going to evaluate variable `a` without defined variable `c`:

```
b=g()
a=f(b,c)
c=h()
```

![](http://i.imgur.com/dbidMbl.gif)

#### Scoring

There are up to 20 tests in every file. If we able to find the answer when there are up to one hundred statements (n <= 100), we get 12 points. If there are up to one thousand statements, we get 12+15 points. It happens that you can only come up with not enough optimal solution, and you get only the part of the score. It is assumed that 12 points is given for [O(n^3)](https://en.wikipedia.org/wiki/Big_O_notation) solution, and 27 points is given for [O(n^2)](https://en.wikipedia.org/wiki/Big_O_notation) solution. Let's write the better solution at once.

#### Hack!

```cpp
#include <bits/stdc++.h>
using namespace std;

int main(int argc, char *argv[]) {
    int t;
    cin >> t;
    for (int i = 0; i < t; i++) {
        cout << "Case #" << i + 1 << ": ";
        cerr << "Test #" << i + 1 << endl;
        cout << (solve() ? "GOOD" : "BAD") << endl;
    }
}
```

`solve()` reads the next test and gives the answer.

It's better to write the helper function for cutting the string:

```cpp
vector<string> crop(string& s) {
    vector<string> vec;
    string cur = "";

    for (int i = 0; i < (int) s.size(); i++) {
        if (s[i] < 'a' || s[i] > 'z') {
            // if s[i] is not a lowercase english letter
            if (!cur.empty()) {
                vec.push_back(cur);
                cur = "";
            }
        } else {
            cur.push_back(s[i]);
        }
    }

    return vec;
}
```

#### Graph theory

How can we even solve it? It's time for the popular and legendary **graph theory**.

![](https://s-media-cache-ak0.pinimg.com/originals/37/71/58/377158fca2b73b083fd0aa4a4f703930.gif)

Let's represent our task as a graph! Every variable is the **vertex** now, from which in every dependent variable (his vertex) the directed **edge** is goes.

#### Building a model

Our example looks so - there are vertices (somewhere in space) `a`, `b`, `c`, and there are two edges-lines, directed from `a` to other vertices. In the same way we can represent any test. Intuitively, the task can be solved in this way - we're looking at a graph and detecting if there are the vertex **without** directed edges out of him. If the such vertex was found, then its variable we "define". And this vertex and edges to him throw out, and looking at a graph again.

![](http://i.imgur.com/noiejkr.gif)

#### Finding the answer

If we still have undefined variables, but we already can't find any vertex without directed edges, it means that we have a **cycle**, that is unsolvable dependencies. Somewhere `x` depends on `y`, `y` depends on `z`, and `z` depends on `x`, like this. Of course, if we have defined all the variables, then the solution is exists.

#### Finishing writting the code

```cpp
bool solve() {
    int n;
    cin >> n;

    vector<string> sources(n);  // source lines
    map<string, int> indexes;   // map of "variable name" -> "index"
    vector< vector<string> > deps_raw(n);   // list of dependencies (as variable string)
    vector< set<int> > deps(n);   // list of dependencies (as indexes)

    for (int i = 0; i < n; i++) {
        string s;
        cin >> s;

        sources[i] = s;

        vector<string> variables = crop(s);
        string name = variables[0];
        indexes[name] = i;

        vector<string> var_names;
        for (int z = 2; z < (int) variables.size(); z++)
            var_names.push_back(variables[z]);
        deps_raw[i] = var_names;
    }

    for (int i = 0; i < n; i++) {
        for (auto dep : deps_raw[i]) {
            if (!indexes.count(dep))
                return false;
        }
    }

    for (int i = 0; i < n; i++) {
        set<int> deps_set;
        for (auto dep : deps_raw[i])
            deps_set.insert(indexes[dep]);
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

        cerr << count << "-th line: " << sources[v] << endl;

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

Compile in your IDE or in the console `g++ -std=c++11 main.cpp -o main`, launch the program with `./main <input_file >output_file`.

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

That's all, thanks for reading! But don't leave for a long time...

:wq
