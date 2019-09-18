# 292 Nim Game

题目的意思是，两个人玩取石子游戏，每个人每次只能取1,2,3个石子，并且轮流取，玩家A先手，给定的初始石子数量，问玩家A能否取得胜利。

首先，当石子的数量是1,2,3时，先手玩家必然胜利

如果石子数量是4，先手取1,2,3个石子之后，剩余的个数分别为3,2,1，后手玩家都必然会胜利

如果石子数量是5，先手取1,2,3个石子后，剩余个数为4,3,2，当剩余为4时，后手玩家可以取1,2,3个，留给先手玩家的数量就是3,2,1，此时先手也会胜利。

如果石子数量是6，先手取2个后，剩余4个石子，后手玩家必败

如果石子数量是7，先手取3个后，剩余4个石子，后手玩家必败

因此，**如果能够使得先手玩家取完之后剩余4个石子，那么先手玩家就赢了**。



Let's go on. How to guarantee the "win" status of "5 or 6 or 7 stones left when it's my turn"? We should give the "lose" status to my friend that is "8 stones left when it's my friend's turn".Because 8 - 1 = 7, 8 - 2 = 6, 8 - 3 = 5. Again, how to give the "lose" status to my friend? We must be at status of 9, 10, 11 stones left when it's my turn. Because then we have the chance to make the count of left stones be 8: 9 - 1 = 8, 10 - 2 = 8, 11 - 3 = 8. We can find 9 = 1 + 8 = (1 + 4) + 4 = 5 + 4, 10 = 2 + 8 = (2 + 4) + 4 = 6 + 4, 11 = (3 + 4) + 4 = 7 + 4. The formula is now going out: T(n) = T(n - 1) + 4. T(n) is the left stones of the n times of my turn. The basic case T(0) = {1, 2, 3}.



Now we can write a solution like: Test if the number is one of T(n). If yes, we can win. Otherwise we'd lose. Actually it takes O(n) time complexity.



The above solution is really slow to this problem and will lead to a "Time Limit Exceeded" complain.How can we optimize it? Let's expand the formula T(n) = T(n - 1) + 4:



T(n) = {(((1 + 4) + 4) + 4 ..., ((2 + 4) +4) + 4 ..., ((3 + 4) +4) + 4 ...}
= {1 + 4*n, 2 + 4*n, 3 + 4*n}



Can you get the idea? If T(n) % 4 is 1 or 2 or 3, we can win the game. And beacuse the mod value can only be 0, 1, 2 or 3, that implies if T(n) % 4 != 0, we'll win the game. Now we get the one liner:
