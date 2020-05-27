---
title: leetcode 16.04 井字游戏 （面）
description: leetcode 数据题
categories: leetcode 
tags: array
---


> 设计一个算法，判断玩家是否赢了井字游戏。输入是一个 N x N 的数组棋盘，由字符" "，"X"和"O"组成，其中字符" "代表一个空位。
>
> 以下是井字游戏的规则：
>
> 1. 玩家轮流将字符放入空位（" "）中。  
> 2. 第一个玩家总是放字符"O"，且第二个玩家总是放字符"X"。  
> 3. "X"和"O"只允许放置在空位中，不允许对已放有字符的位置进行填充。  
> 4. 当有N个相同（且非空）的字符填充任何行、列或对角线时，游戏结束，对应该字符的玩家获胜。  
> 5. 当所有位置非空时，也算为游戏结束。  
> 6. 如果游戏结束，玩家不允许再放置字符。  
> 7. 如果游戏存在获胜者，就返回该游戏的获胜者使用的字符（"X"或"O"）；如果游戏以平局结束，则返回 "Draw"；如果仍会有行动（游戏未结束），则返回 "Pending"。
>
>
> 示例 1：  
> 输入： board = ["O X"," XO","X O"]  
> 输出： "X"  
>
> 示例 2：  
> 输入： board = ["OOX","XXO","OXO"]  
> 输出： "Draw"  
> 解释： 没有玩家获胜且不存在空位
>  
> 示例 3：  
> 输入： board = ["OOX","XXO","OX "]  
> 输出： "Pending"  
> 解释： 没有玩家获胜且仍存在空位
>  
> 提示：  
> 1 <= board.length == board[i].length <= 100  
> 输入一定遵循井字棋规则  
>
> 来源：力扣（LeetCode）  
> 链接：[力扣（LeetCode）](https://leetcode-cn.com/problems/tic-tac-toe-lcci)  
> 著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。  
>

解：
根据描述，仅存在以下三种情况：
1. 有人获胜
2. 没有玩家获胜且存在空位
3. 没有玩家获胜且不存在空位

1. 有人获胜：
无论是'X' 还是'O'获胜，必须满足以下条件之一
    * 全行相等  any([all([char == C for char in row]) for row in board]）
    * 全列相等  any([all([char == C for char in col]) for col in zip(*board)])
    * 左对角线相等 all([board[i][i] == C for i in range(N)] )
    * 右对角线相等 all([board[i][N-1-i] == C for i in range(N)] )
2. 没有玩家获胜且存在空位
    * 没人获胜
    * 存在空位 any([ " " in row for row in board])

> any()  x中有一个不为空、0、false, 返回True，否则返回False  x 可以为list、tuple  
> all()  x中的所有元素均不为空、0、false, 返回True，否则返回False x 可以为list、tuple



```
class Solution(object):
    def tictactoe(self, board):
        """
        :type board: List[str]
        :rtype: str
        """

        N = len(board)
        def check(C):
            if C == " ": # 判断是否有空位
                return  any([ " " in row for row in board])

            return  any((
                         any([all([char == C for char in row]) for row in board]), # 判断行
                         any([all([char == C for char in col]) for col in zip(*board)]), #判断列
                         all([board[i][i] == C for i in range(N)] ), #判断左对角线 \
                         all([board[i][N-1-i] == C for i in range(N)] ) #判断右对角线 /
                        ))

        if check('X'): return 'X'
        if check('O'): return 'O'
        if check(' '): return 'Pending'
        return 'Draw'

        
```


