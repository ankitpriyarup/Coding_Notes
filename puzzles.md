# Puzzles

### The Heavy Pill

You have 20 bottles of pills, 19 bottles have 1.0 gram pills, but one has pills of weight 1.1 grams. Given a scale that provides an exact measurement, how would you find the heavy bottle? You can only use the scale once.

```text
Pick 1 pill from bottle 1, 2 from bottle 2, 3 from bottle 3 and so on
total weight would be: (1 + 2 + 3 + ... + 20) + 0.1(k)
so, (weight - 210)/0.1 will tell which bottle has heavy pill
```

### Backetball

You have a basketball hoop and someone says that you can play one of two games.  
Game 1: You get one shot to make the hoop  
Game 2: You get three shots and you have to make two of three shots  
If p is the probability of making a particular shot, for which values of p should you pick one game or the other?

```text
Prob of game 1: p
Prob of game 2:
[Making exactly 2 shots]            [Making exactly 3 shots]
    3C2 * p^2 * (1-p)                   p^3
    3(1-p)p^2
    = 3p^2 - 2p^3

p > 3p^2 - p^3
1 > 3p - p^2
(2p - 1)(p - 1) > 0
So we should play Game 1 if 0 < p < 0.5 and Game 2 if 0.5 < p < 1
```

### Dominos

There is an 8x8 chessboard in which two diagonally opposite corners have been cur off. You are given 31 dominos, and single domino can cover exactly two squares. Can you use the 31 dominos to cover the entire board? Prove your answer \(by providing an example or showing why its impossible\).

```text
We removed diagonal opposite corners which have same colors. On placing domino
each different color is covered. so 62 tiles are left out of 30 are white and 
30 are black but there are 2 left overs of same color which we can never cover.
```

### Ants on a Triangle

There are three ants on different vertices of a triangle. What is the probability of collision \(between any two or all of them\) if they start walking on the sides of the triangle? Assume that each ant randomly picks a direction, with either direction being equally likely to be chosen, and that they walk at the same speed. Similarly find the probability of collision with n ants on a n-vertex polygon.

```text
P(colliding) = 1 - P(Not Colliding)
Ants will not collide if they all either move clockwise or counter clockwise
P(Not colliding) = (1/2)^n + (1/2)^n = (1/2)^(n-1)
```

### Jugs of Water

You have a five-quart jug, a three-quart jug, and an unlimited supply of water \(but no measuring cups\). How would you come up with exactly four quarts of water? Note that the jugs are oddly shaped, such that filling up exactly "half" of the jug would be impossible.

```text
Remeber we can make 5-3 quarts easily, fill 5q jug then empty it to 3q jug you
will be left with 2.
Transfer 2quart from jug1 to 2. Now again pour all 5q to jug1. And again pour
from jug1 to 2. This time 1 additional will go and we are left with 4 in jug1
and 3 in jug2.

If jugs sizes are relatively prime, we can measure any value between 1 and sum
of jug sizes. It can be represented as a linear diaphotine equation:
ax + by = c
where a and b are size of jugs we have and c is amount that we form
The theory of Diophantine equations states you can create an amount c iff
it is a multiple of the gcd of a and b which is 1 (since they are
relative prime)
```

### Blue-Eyes Island

A bunch of people are living on an island, when a visitor comes with a strange order: all blue eyed people must leave the island as soon as possible. There will be a flight out at 8 pm every evening. Each person can see everyone else's eye color, but they do not know their own \(nor is anyone allowed to tell them\). Additionally they do not know how many people have blue eyes, although they do know that at least one person does. How many days will it take the blue-eyed person to leave?

```text
Assume there are n people in island and b have b eyes

Case b = 1: (exactly 1 person has blue eyes)
Everyone knows, atleast 1 has blue eyes and special person will see no one else
having blue eyes so he will assume he has so he will leave that evening.

Case b = 2: (exactly 2 persons have blue eyes)
Two special people see each other, but are unsure whether b is 1 or 2. So they
do nothing next day if they meet again means that b is not 1 since if it was
then the person would have left. so b = 2. Which means he himself has blue eyes

Case b > 2: (General case)
Say if b = k then those k people will immediately know that there are either k
or k-1 people with blue eyes. If there were k-1 then those would have left on
second night. So it follows dp relation. For b blue eyes it will take b nights.
```

### The Apocalypse

In the new post-apocalyptic world, the world queen is desperately concerned about the birth rate. Therefore, she decrees that all families should ensure that they have one girl or else they face massive fines. If all families abide by this policy - that is, they have continue to have children until they have one girl, at which point they immediately stop - what will be the gender ratio of the new generation be? \(Assume that the odds of someone having a boy or girl on any given pregnancy is equal\)

```text
G -> 1/2
BG -> 1/4
BBG -> 1/8
Infinite GP sum giving 1. This would mean that gender ratio is even
Families on an avg contribute exactly 1 boy and 1 girl.

It's wrong to think that way. Biology hasn't been changed. Half
of newborn babies are boys and girls abiding by some rule about
when to stop having children doesn't change this fact.

Concatenate all house hold data G - BBG - BBBBG on a whole it seems
just as probable as new born baby prob.

Therefore gender ratio is 50-50

We can simulate this on C++, for larger values it should give 0.5
probability
```

### 100 Lockers

There are 100 closed lockers in a hallway. A man begins by opening all 100 lockers. Next, he closes every second locker. Then, on his third pass, he toggles every third locker. This process continues for 100 passes, such that one pass i, the man toggles every ith locker. After his 100th pass in the hallway, in which he toggles only locker \#100, how much lockers are open.

```text
For door 100 its:
1    -    on
2    -    off
3    -    off
4    -    on
It will toggle for every factor
cnt_of_factors&1 == 1 means door is open

cnt_of_factors is odd when x(100) is a perfect square)

so there are 1, 4, 9, 16, 25, 36, 49, 64, 81, 100
10 perfect square so 10 lockers will be open in the end
```

### Poison

You have 1000 bottles of wine, and one them is poisoned. To test you let rats drink it. Minimize number of rats. 

```text
Way 1 (999 Rats) drink one of bottle, if out of those one dies means
that bottle was poisoned, if no one dies means 100th bottle which
wasn't tested was poisoned

Way 2 (32 rats) Divide search space by say half so we need 500 rats
each will taste 2 wines. If a kth rat dies means 2*k or 2*k+1 could
be that bottle so we will need additional to check those 2 bottles
so we need 1 rat, making 501
Generalising it:
->  require 1000/k + (k-1) rats

Let's find out minima of above function:
yk = 1000 + k^2 - k ....(i)
differentiate both sides and put dy/dk = 0
we will get k = (1+y)/2 ....(ii)
put this in eq i and solving quadratic equation:
y^2 + 2y - 3999 = 0 we get y = 62.246 putting in ii we get
k = 32 (take integer ignore fraction)
so we need 32 rats

Way 3 (log2(1000) rats) Why 32 in above? Let's denote each wine in
binary:
00001    -> feed to 1st rat
00010    -> feed to 2nd rat
00011    -> feed to 1st and 2nd rat
00100    -> feed to 3rd rat
00101    -> feed to 1st and 3rd rat
00110    -> feed to 2nd and 3rd rat
00111    -> feed to 1st, 2nd and 3rd rat

If a wine x is poisned then all it's set bit rats must be dead
```

