# LeetCode-37-Sudoku-Solver
 解数独
 
 题目链接：https://leetcode-cn.com/problems/sudoku-solver/submissions/
 
 这个题目中只要求一个解即可（code1），数独本身可能有存在多个解（code2将所有解都计算一边存储起来,全部输出，这个牛课网上有原题，但是题目测测试案例似乎有问题，只考虑一个解，全部输出会报错，输出的不是给出的解也报错）
 
 ## code1

    class Solution:
      def solveSudoku(self, board: List[List[str]]) -> None:
          """
          Do not return anything, modify board in-place instead.
          """
          dic={0:0,1:0,2:0,3:3,4:3,5:3,6:6,7:6,8:6}
          def func(board,x,y,flag):
              if x==9:
                  flag[0]=True
                  return
              elif y==9:
                  func(board,x+1,0,flag)
              elif board[x][y]=='.':
                  #print(board)
                  sx=[board[x][t] for t in range(9)]
                  sy=[board[t][y] for t in range(9)]
                  sp=[board[t][p] for t in range(dic[x],dic[x]+3) for p in range(dic[y],dic[y]+3)]
                  for s in range(1,10):
                      s=str(s)
                      if s not in sx and s not in sy and s not in sp:
                          board[x][y]=s
                          func(board,x,y,flag)
                          if flag[0]:
                              return
                          board[x][y]='.'
              else:
                  func(board,x,y+1,flag)
          func(board,0,0,[False])


 ## code2
 
    dic={0:0,1:0,2:0,3:3,4:3,5:3,6:6,7:6,8:6}
    res=[]

    def func(m,x,y):
        if x==9:
            cur_res=[]
            for i in range(9):
                cur_res.append(' '.join([str(en) for en in m[i]]))
            res.append(cur_res)
        elif y==9:
            func(m,x+1,0)
        else:
            if m[x][y]!=0:
                func(m,x,y+1)
            else:
                for t in range(1,10):
                    subx,suby=dic[x],dic[y]
                    subm=[m[i][j] for i in range(subx,subx+3) for j in range(suby,suby+3) ]
                    subx=m[x]
                    suby=[m[i][y] for i in range(9)]
                    if t in subx or t in suby or t in subm:
                        continue
                    else:
                        m[x][y]=t
                        func(m,x,y+1)
                        m[x][y]=0

    while True:
        try:
            matrix=[list(map(int,input().split())) for i in range(9)]
            func(matrix,0,0)
            for r in res:
                for line in r:
                    print(line)
                print('\n')
        except:
            break

